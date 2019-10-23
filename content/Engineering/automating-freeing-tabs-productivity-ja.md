Title: ブラウザビューを自動化してブラウザのタブを解放し、時間を節約、生産性をあげよう！
Date: 2018-12-21 20:00
Slug: automate-browsing-free-tabs-save-time-be-productive-en
Lang: ja
Tags: python; automation; lifehack; requests; beautifulsoup; xoxzo.cloudpy;
Author: Josef Monje
Summary: ブラウザで多くの時間を費やしています。 時々、ページの変更を監視するなど、いくつかの簡単なアクティビティ専用のブラウザタブをいくつか用意しています。 しかし、これらの一見小さなアクティビティは最終的にブラウザのタブの多くを占有することになり、その結果、より重要なことをして生産性を維持することから私たちをそらします。 この投稿では、簡単なスクリプトを作成してKickstarterプロジェクトの取引を監視するプロセスを順を追って説明します。 これは、初心者または非プログラマを対象としています。


## Tab Overload Problems

We spend a lot of time in our browser. Sometimes we dedicate a few browser tabs to do some simple repetitive activity like monitoring a certain page for changes. Eventually, we end up with a lot of open tabs for some forgotten purpose in the past. Maintaining a lot of tabs can sometimes even prevent us from quitting our browser application just because there's something "important" about those tabs. I admit, this used to happen to me.

Browsers take up a lot of precious computing resources. The more tabs we keep, the less our computer can perform for other tasks. The problem eventually accumulates and we end up with a slow computer, preventing or slowing us down from accomplishing more important things and maintaining our productivity. Who wants a slow browser or computer? I sure wouldn't want to have to take care of my browser and its tabs like a pet or a plant, and grow many open tabs.

## A Better Way

> _"There must be a better way!"_

-- Raymond Hettinger, Python core developer

As Raymond Hettinger, one of my favourite Python developers, would say _"There must be a better way!"_. We can keep those distracting tabs and slow browsers from slowing us down. For this example, I chose to write a script to watch a Kickstarter project so I can get better deals just in case they become available. Beginners or non-programmers can use this as a simple #lifehack or as a starting point and apply it to their own use cases like waiting for that new anime or manga episode.

Personally, when I see some cool [Kickstarter](https://www.kickstarter.com/) project, I'm usually not lucky or fast enough to get the limited early bird deals. In this post, I'll walk through the process of creating a simple script to watch out for early bird slots in case they become available. This can happen when a supporter suddenly changes their mind. It can happen. In which case, we would suddenly see something like `Limited (1 left of 10)` in the page and it probably wouldn't last very long before someone takes it. It would be a total waste to keep an open tab and keep refreshing it every once in a while to do this task so we're going to write a Python script. We'll be working on the command-line as well as with our favourite text editor. Let's begin!

## Writing Our Python Script

First, let's make sure that `python` and `pip` are installed. You can test the `Python` and `pip` commands in the Windows Command Prompt or macOS/Linux Terminal to see if it works or if you get errors. You can download the latest Python version [here](https://www.python.org/downloads/) if you haven't downloaded it yet. It should be some version of `Python 3`. Here's a helpful page in [MakeUseOf](https://www.makeuseof.com/tag/install-pip-for-python/) to help you if you need to install or troubleshoot some error.

Then once we have those commands, we'll make a file for our script. I named my file `kickstarter-watcher.py`, Python files end with `py` extension. You can put it anywhere like your Desktop, it doesn't matter right now. Navigate to your file's folder in your Command Prompt/Terminal. Then open the file with your favourite text editor. We can now start writing our code, test it and learn some Python along the way.

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
