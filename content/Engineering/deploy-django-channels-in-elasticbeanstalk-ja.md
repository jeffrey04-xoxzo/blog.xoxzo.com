Title: AWS Elasticbeanstalk上で、Amazon Linux 2 AMI を使って Django Channels 2.x をデプロイする方法
Date: 2020-07-17 08:00
Author: Jules Capacillo
Tags: django-channels; tutorial; elasticbeanstalk;
Slug: deploy-django-channels-in-elasticbeanstalk
Lang: ja
Summary: AWS Elasticbeanstalk上で、Amazon Linux 2 AMI を使って Django Channels 2.x をデプロイする方法をご紹介します。

アマゾンウェブサービス（AWS）Elasticbeanstalk（EB）にアプリケーションをデプロイする際に難しいのは、
クラスター内にあるそれぞれのインスタンスで実行されるプロセスを処理することがあげられます。 
具体的には、ロードバランサーを介してElasticbeanstalkにDjangoチャネル2をデプロイする方法です。

There were a lot of tutorials/guides on the internet but none really quite helped, 
the reason being is that most if not all of them use a configuration file geared for Amazon Linux AMI instances, 
while the latest environment platforms of AWS now use Amazon Linux 2 (AMI 2) instances 
which have a different configuration and directory structure that renders previous tutorials obsolete. 
You can read more on how to migrate to AMI 2 
<a href="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html" target="_blank">here</a>
インターネット上には多くのチュートリアル/ガイドがありましたが、実際にはまったく役に立ちませんでした。
その理由は、AWSの最新の環境プラットフォームが現在Amazonを使用しているのに対し、
それらのすべてではないにしてもほとんどがAmazon LinuxAMIインスタンス用の構成ファイルを使用しているためです。 
Linux-2（AMI 2）インスタンスは、以前のチュートリアルを廃止するために異なる設定とディレクトリ構造を持っています。 
AMI2への移行方法の詳細を読むことができます
<ahref = "https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html" target = "_ blank"> ここ</a>




This tutorial is divided into three parts, feel free to choose the parts where you are more interested in:

- <a href="#assumptions">Set up assumptions</a>
- <a href="#deployment">Deploying your application</a>
- <a href="#using_https">Using Https</a>


<h3 id="assumptions" class="anchor-link">Set up assumptions</h3>
In order for this tutorial to be effective, I have shared my underlying assumptions below:

1. You are using an Application load balancer (ALB) routing setup, which just means that all traffic is handled by ALB. Classic load balancers do not support WebSockets, while Network load balancers were not used for this tutorial;

2. You already have an up and running Django application locally that already runs Django Channels 2;

3. You have an external Redis instance (Any would do as long as it is accessible through your EB instances);

4. lastly, you already have the EB Command Line Interface (CLI) installed, if not, you can follow the instructions <a href="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html" target="_blank">here</a> 


<h3 id="deployment" class="anchor-link">Deploying your application</h3>
Once everything is set up, let’s deploy! If you don’t yet have a .ebextensions folder, please do create it in your root directory. Inside, create a config file `01_<your_custom_name>.config` and paste the entries below:

    option_settings:
        aws:elbv2:listener:80:
            DefaultProcess: http
            ListenerEnabled: 'true'
            Protocol: HTTP
            Rules: ws
        aws:elbv2:listenerrule:ws:
            PathPatterns: /ws/*
            Process: websocket
            Priority: 1
        aws:elasticbeanstalk:environment:process:http:
            Port: '80'
            Protocol: HTTP
        aws:elasticbeanstalk:environment:process:websocket:
            Port: '5000'
            Protocol: HTTP

This tells our ALB to listen to port 80, and if a path comes in with the pattern `/ws/*` pass the request to port 5000 of the instances under EB. Note that port 5000 here is optional and you can use whichever port you want. You can also lookup other rules <a href="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options-general.html" target="_blank">here</a> 

Next, we use a `Procfile` to run our processes. Create a file named `Procfile` and save it in your root directory, inside paste the following code:

    web: gunicorn --bind :8000 --workers 3 --threads 2 <your_app>.wsgi:application
    websocket: daphne -b 0.0.0.0 -p 5000 <your_app>.asgi:application


Make sure you change `<your_app>` to point to your application. Also, note that we have configured our wsgi entry point here rather than in the config file we did above. Lastly, the daphne server port we’re binding should coincide with the one you specified in your ALB config file as this would receive the requests from WebSockets.

You can learn more on using Procfiles and other means to extend your EB Linux platform <a href="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/platforms-linux-extend.html" target="_blank">here</a> and <a href="(https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/python-configuration-procfile.html" target="_blank">here</a> 


<h3 id="using_https" class="anchor-link">Using https</h3>
Once you enabled https for your application, you would just need to do a minor tweak in your configuration file to listen for `wss` connections and pass them accordingly. In the config file you have created previously, just edit the code to look like the following:

    option_settings:
        aws:elbv2:listener:80:
            DefaultProcess: http
            ListenerEnabled: 'true'
            Protocol: HTTP
            Rules: ws
        aws:elbv2:listener:443:
            ListenerEnabled: 'true'
            Protocol: HTTPS
            SSLCertificateArns: <YOUR ARN FROM AWS CERTIFICATES MANAGER>
            SSLPolicy: ELBSecurityPolicy-2016-08
            Rules: ws
        aws:elbv2:listenerrule:ws:
            PathPatterns: /ws/*
            Process: websocket
            Priority: 1
        aws:elasticbeanstalk:environment:process:http:
            Port: '80'
            Protocol: HTTP
        aws:elasticbeanstalk:environment:process:websocket:
            Port: '5000'
            Protocol: HTTP


This just listens to port 443, then passes WebSocket related path to your instances inside EB as per before.  Make sure you replace `<YOUR ARN FROM AWS CERTIFICATES MANAGER>` with the actual arn resource from your approved certificates. 


There you have it, folks! Hope this guide helped.


References:

[https://medium.com/@elspanishgeek/how-to-deploy-django-channels-2-x-on-aws-elastic-beanstalk-8621771d4ff0](https://medium.com/@elspanishgeek/how-to-deploy-django-channels-2-x-on-aws-elastic-beanstalk-8621771d4ff0)

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html)

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options-general.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options-general.html)

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/platforms-linux-extend.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/platforms-linux-extend.html)

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/python-configuration-procfile.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/python-configuration-procfile.html)
