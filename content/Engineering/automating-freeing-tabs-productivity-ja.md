Title: ブラウザビューを自動化してブラウザのタブを解放し、時間を節約、生産性をあげよう！
Date: 2018-12-21 20:00
Slug: automate-browsing-free-tabs-save-time-be-productive-en
Lang: ja
Tags: python; automation; lifehack; requests; beautifulsoup; xoxzo.cloudpy;
Author: Josef Monje
Summary: 私達は、ブラウザで多くの時間を費やしています。 時には、ページの変更を監視したりというような、シンプルな用事のためにいくつかのタブを当てたりもします。 しかし、一見シンプルな用事が、最終的にはブラウザのタブを多く独占し、その結果、もっと重要なことに取り組んで生産性を維持する妨げになったりもするのです。 この投稿では、簡単なスクリプトを作成してKickstarterプロジェクトの取り扱いを監視するプロセスを順を追って説明します。 初心者または非プログラマーを対象としています。


## タブ過負荷の問題

私達は、ブラウザで多くの時間を費やしています。 時には、ページの変更を監視したりというような、シンプルな用事のためにいくつかのタブを当てたりもします。 最終的には、すでに忘れていたことのためにまで、多くのタブを開いたままということも。 多くのタブを開いておくと、そのタブに「重要な」何かがあるようで、ブラウザアプリケーション自体を終了できなくなることさえあります。 そうです、これは私自身の経験です。

ブラウザは貴重なコンピューティング・リソースを多く占有します。 開きっぱなしのタブが多いほど、コンピューターが他のタスクを実行できる容量が少なくなります。 問題は最終的に溜まりに溜まって、コンピューターの処理速度が遅くなり、もっと重要なことをやり遂げたり、生産性を維持したりすることができなくなるのです。 動きの遅いブラウザーや遅いコンピューターなんて、誰がほしいと思いますか？ ブラウザやタブを、まるでペットや植物のように世話をして成長させ、たくさん開かせたいなんて、願う人はいません。

## 改善案

> _"絶対に改善案はある!"_

-- Raymond Hettinger, Python コア開発者

私が好きなPython開発者の1人、Raymond Hettinger (レイモンド・ヘッティンガー)なら、「絶対に改善案はある！」_と言うでしょう。 
こんなうっとおしいタブや動きの遅いブラウザに、邪魔されないようにすることは可能なんです。
この問題に対して、私は、Kickstarterプロジェクトが利用可能になった場合に備えて、Kickstarterプロジェクトを監視するスクリプトを作成することにしました。
初心者や非プログラマーの方には、これを単純な #lifehack または第一歩となり、新しいアニメや漫画のエピソードを待つ、といった、自分だけのユースケースにも使うことができるんです。


個人的に、クールな[Kickstarter](https://www.kickstarter.com/）プロジェクトを見ると、早いもの勝ちに間に合わなかったような、アンラッキーな気分になります。今回は、簡単なスクリプトを作成して、早いもの勝ちのスロットが利用可能になった場合に注意喚起するプロセスについて説明します。
これは、サポーターの気が前触れなしに変わったときに起こりますし、 起こり得るんです。
その場合、ページに突然「限定（10件中1件目）」などと表示され、すぐに誰かに取られてしまうんです。
だからといって、タブを開いたままにして、時々ページ更新し続けるのは無駄なことです。
そこで、Pythonスクリプトを作成します。 コマンドラインだけでなく、お好きなテキストエディターでも作業を行います。 

さぁ、始めましょう！




## Python Script を書く

まずは、 `python`と` pip`がインストールされていることを確認しましょう。 
Windowsコマンドプロンプト または macOS / Linuxターミナルで「Python」および「pip」コマンドをテストして、動作するか、エラーが発生するかを確認できます。 まだダウンロードされていない場合は、[最新のPythonバージョン](https://www.python.org/downloads/）をダウンロードできます。 `Python 3`の何らかのバージョンである必要があります。 

インストールや、エラーのトラブルシューティングの際に役立つサイトは[MakeUseOf](https://www.makeuseof.com/tag/install-pip-for-python/）（英語）


Then once we have those commands, we'll make a file for our script. I named my file `kickstarter-watcher.py`, Python files end with `py` extension. You can put it anywhere like your Desktop, it doesn't matter right now. Navigate to your file's folder in your Command Prompt/Terminal. Then open the file with your favourite text editor. We can now start writing our code, test it and learn some Python along the way.

コマンド `python` と `pip` を取得したら、スクリプト用のファイルを作成しましょう。
例として、私はファイルに「kickstarter-watcher.py」という名前を付けてみました。
Pythonファイルは「.py」拡張子で終わってください。
デスクトップなど、どこにでも置くことができますが、今は関係ありません。
コマンドプロンプト/ターミナルでファイルのフォルダーに移動します。
次に、お気に入りのテキストエディターでファイルを開きます。

これで、コードの記述を開始し、テストして、途中でPythonを学ぶことができます。
コマンド `python` と `pip` を取得したら、スクリプト用のファイルを作成しましょう。ファイルに「kickstarter-watcher.py」という名前を付けました。Pythonファイルは「py」拡張子で終わります。 デスクトップのようにどこにでも置くことができますが、今は関係ありません。 コマンドプロンプト/ターミナルでファイルのフォルダーに移動します。 次に、お気に入りのテキストエディターでファイルを開きます。 これで、コードの記述を開始し、テストして、途中でPythonを学ぶことができます。



### Using `sys`

Open your script file and paste this:

```python
import sys

urls = sys.argv[1:]
if len(urls) == 0:
    print('Please provide at least one Kickstarter project URL to check.')
else:
    print(urls)
```

* `sys` is part of the Python standard library so we don't have to download anything to use it.
* `sys.argv` gives us a list of command-line arguments passed to the script.
* `sys.argv[1:]` slices the list and returns a new one, starting from the index `1`, which is actually the second one because the first one is index `0`. The value is then assigned to a variable `urls`.
* `len(urls)` gives us the number of elements in our url list. If no URLs were given, a message is printed.

To use our script we can just run in the command-line:

```
python kickstarter-watcher.py
```

This should show us our message to provide a Kickstarter project URL. Let's test it with a URL.

```
python kickstarter-watcher.py https://www.kickstarter.com
```

For now, it just prints the URL. Later, we'll do some things with it. Next, we'll write a few functions to do what we want:

1. Download the HTML page in the URL
2. Get the parts that we want from the HTML

### Using `requests`

To download the URL, we'll use the `requests` library, which makes doing this super simple. In the command-line type:

```
pip install requests
```

You should see that it will be downloaded to your computer and we can use it in our script.

```python
import sys

import requests


def get_url(url):
    print('Checking:', url)
    response = requests.get(url)
    if response.ok:
        return response.content
    else:
        print('Something went wrong, try again later...')
        return


urls = sys.argv[1:]
if len(urls) == 0:
    print('Please provide at least one Kickstarter project URL to check.')

for url in urls:
    try:
        content = get_url(url)
        if content:
            print(content)
    except Exception as e:
        print(e)
```

* `get_url()` is a function that accepts the URL, we print it to check what page the script is browsing.
* `requests.get(url)`  here we use `requests` to send a [`GET`](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) request to the URL and then assign the result to the `response` variable.
* [`response.ok` ](http://docs.python-requests.org/en/master/api/#requests.Response.ok) is used to check if our request went through. Then either the HTML content of the response is returned, or an error message is printed and nothing is returned.
* `for url in urls:` uses the list of URLs we provided when we ran the script and loops through each of them. The indented codeblock below it is executed for each and each execution uses each `url` in the list.
* `try: ... except:` catches any kind of `Exception` or error, which allows us to handle it and our code will continue running. It would be best if we can specify the exceptions here so we can handle them individually. For now, we print it so we can find out what they might be.
* We call`get_url()` inside this block, and if there's content, we print it for now. If there's an error, we also print it.

When we run this code, it just prints the HTML content from the page, or the error if any:

```
python kickstarter-watcher.py https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
```

```
Checking: https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
... (some hard-to-read HTML)
```

What we need now is to make sense of the HTML content. We'll use `BeautifulSoup` for this.

### Using `BeautifulSoup`

To parse the HTML content, we'll use the `BeautifulSoup` library. In the command-line type:

```
pip install beautifulsoup4
```

This is the library that helps us parse the HTML easier.

```python
import sys

import requests
from bs4 import BeautifulSoup as soup


def get_url(url):
    print('Checking:', url)
    response = requests.get(url)
    if response.ok:
        return soup(response.content, 'html.parser')
    else:
        print('Something went wrong, try again later...')


def check_earlybird(content):
    sidebar = content.find_all('li', {'class': 'pledge-selectable-sidebar'})
    for selectable in sidebar:
        title = selectable.find('h3', {'class': 'pledge__title'})
        stats = selectable.find('div', {'class': 'pledge__backer-stats'})
    
        limit = None
        backer_count = None
        if stats:
            limit = stats.find('span', {'class', 'pledge__limit'})
            backer_count = stats.find('span', {'class', 'pledge__backer_count'})
    
        if title and limit:
            title = title.text.strip()
            limit = limit.text.strip()
            if 'limited' in limit.lower():
                message = "{0} - {1}".format(title, limit)
                return message


urls = sys.argv[1:]
if len(urls) == 0:
    print('Please provide at least one Kickstarter project URL to check.')

for url in urls:
    try:
        content = get_url(url)
        if content:
            message = check_earlybird(content)
            print(message)
    except Exception as e:
        print(e)
```

* `from bs4 import BeautifulSoup as soup` imports `BeautifulSoup` and assigns it to the alias `soup`. I did this to make it easier to write.
* `get_url()` was modified to return an instance of `BeautifulSoup` when successful.
* `soup(response.content, 'html.parser')` is the `BeautifulSoup` instance mentioned earlier. We passed our HTML content to it and told it to use the HTML parser.
* `check_earlybird()` function was added, it accepts the content, which is now a `BeautifulSoup` instance. Within this function, we find the specific HTML elements we're interested in.
* `content.find_all()` and `content.find()` are methods provided by `BeautifulSoup` to make it easy to look for elements. CSS classes can be a good way to select elements because CSS has to be specific in targetting an element. I inspected the Kickstarter project page template to see what class names were used. You can find it as `class="name here"` within an HTML element.
* The `sidebar` has many selectable pledge options so I use `find_all()` and it represents the HTML for the list of pledge options.
* I loop through the `selectable` elements in the `sidebar` to find the title and the stats. From stats, I also get the limit and backer count.
* `.text.strip()` is used on the `title` and `limit` to remove leading and trailing characters like spaces and new lines. I did two things here, which was to get the text, and the call `strip()` on it.
* Then we check if `limited` is present, these early deals usually accommodate a fixed number of backers and is display as `Limited (N left of X)`
* `.lower()` is used because we don't know the case the text is written in so we just make sure that everything is in lowercase.

To run this script on a page we have to paste the URL of the project page :

```
python kickstarter-watcher.py https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
```

```
Checking: https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
Early Bird! - Limited (96 left of 150)
```

Now that's what we're talking about!

One last thing we can add in this script are notificationss. For example, when the phrase "Limited (1 left of" suddenly appears, it could mean that a previously full pledge package suddenly has an open slot. That's something I want to know right away.

### Using `xoxzo.cloudpy`

To send notifications, we'll use our `xoxzo.cloudpy` library. In the command-line type:

```
pip install xoxzo.cloudpy
```

This is our [Xoxzo client library](https://github.com/xoxzo/xoxzo.cloudpy) that helps us use SMS or even voice calls easily using the Xoxzo API.

```python
import sys

import requests
from bs4 import BeautifulSoup as soup
from xoxzo.cloudpy import XoxzoClient


def get_url(url):
    print('Checking:', url)
    response = requests.get(url)
    if response.ok:
        return soup(response.content, 'html.parser')
    else:
        print('Something went wrong, try again later...')


def check_earlybird(content):
    sidebar = content.find_all('li', {'class': 'pledge-selectable-sidebar'})
    for selectable in sidebar:
        title = selectable.find('h3', {'class': 'pledge__title'})
        stats = selectable.find('div', {'class': 'pledge__backer-stats'})
    
        limit = None
        backer_count = None
        if stats:
            limit = stats.find('span', {'class', 'pledge__limit'})
            backer_count = stats.find('span', {'class', 'pledge__backer_count'})
    
        if title and limit:
            title = title.text.strip()
            limit = limit.text.strip()
            if 'limited' in limit.lower():
                message = "{0} - {1}".format(title, limit)
                return message


def send_notification(number, message):
    sid = "<your xoxzo sid>"
    auth_token = "<your xoxzo auth_token>"
    xc = XoxzoClient(sid=API_SID, auth_token=API_TOKEN)
    result = xc.send_sms(
       message = message,
       recipient = number,
       sender = number,
   )


urls = sys.argv[1:]
if len(urls) == 0:
    print('Please provide at least one Kickstarter project URL to check.')

for url in urls:
    try:
        content = get_url(url)
        if content:
            message = check_earlybird(content)
            if 'Limited (1 left of' in message:
                sms = '{0}\n{1}'.format(url, message)
                result = send_notification('<your number here>', sms)
                msgid = result.messages[0]['msgid']
                print(msgid, sms)
    except Exception as e:
        print(e)
```

The event we're watching for could happen anytime and it would be best to get notified right away. In this case, SMS I would choose SMS as a medium so I can receive it ASAP.

* `from xoxzo.cloudpy import XoxzoClient` here we import the client into our script
* `send_notification()` function was added to use the Xoxzo client and send the message to the given number. To use the Xoxzo API, we need to [register an account](https://www.xoxzo.com/accounts/signup/) and get an API SID and TOKEN that we can use in our script.
* `XoxzoClient(sid=API_SID, auth_token=API_TOKEN)` here we use our credentials to create a Xoxzo client instance and assign it to the variable `xc` in the script.
* `result = xc.send_sms()` we simply use the `send_sms()` method of XoxzoClient.
* Then we modified our script so that when the text we're watching for appears, it sends a notification.
* `msgid = result.messages[0]['msgid']` we get the msgid so we can check the message status just in case. Our API allows you to do that and much more. Be sure to checkout our [documentation](https://docs.xoxzo.com/) to see what our API can offer.

And that is the full script. There are many ways to improve it but it should let you get started with learning Python, using our API, getting your website updates and become more productive. You can save it in a folder where you have all your useful scripts.

I can think of several ways to use this script like enclose it in a loop that runs at certain intervals and leave it running in the background. You can also use [cron](https://www.makeuseof.com/tag/linux-task-scheduling-crontab-explained/) for macOS or Linux to run it according to some schedule. You can also use [Task Scheduler](https://www.makeuseof.com/tag/4-boring-tasks-can-automate-windows-task-scheduler/) for Windows.

Have any fun or useful scripts to share with us? Let us know!
