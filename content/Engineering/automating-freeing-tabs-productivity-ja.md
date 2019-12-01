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

これで、コードを書いてテストしながら、pythonを学ぶことができます。


### `sys` を使う

テキストエディターで開いたファイルに、下記を貼り付けてください。

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

このスクリプトを使うには、コマンドラインに次のように入力して実行します。

```
python kickstarter-watcher.py
```

これで、 provide a Kickstarter project URL つまり Kickstarter project のURLを入力してくださいとメッセージがあるはずです。
URLを併記して試してみましょう。

```
python kickstarter-watcher.py https://www.kickstarter.com
```

今は、URLを表示するだけです。
後でこれは使いますが、次にやりたいことを実行させるためのファンクションを書いてみましょう。

1. 該当のURLにあるHTMLページのダウンロード
2. そのHTMLのほしい部分を抜き出す

### `requests` を使う

URLのダウンロードには、作業を超簡単にするため `requests` ライブラリを使います。 コマンドラインにこう打ち込んでください。

```
pip install requests
```

自分のコンピューターにダウンロードされ、自分のスクリプト中で使えるようになるのがわかるはずです。

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

* `get_url（）`はURLを受け入れる関数で、スクリプトが参照しているページを確認するために表示します。
* `requests.get（URL）`ここでは、URLに[ `GET`](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods）リクエストを送信するために`requests`を使用し、その後に結果を`response`変数に代入します。
* [ `response.ok`](http://docs.python-requests.org/en/master/api/#requests.Response.ok）は、自分のリクエストが通過したかどうかを確認するために使用されます。すると、応答としてHTMLコンテンツが返されるか、何も応答なくエラーメッセージが出力されるかになります。
* `for url in urls：`は、スクリプトの実行時に指定したURLのリストを使用し、各URLをループします。
下に続くインデントしたコードブロックは、それぞれに実行され、各実行は、リスト内の各 `url`を使用して実行しています。
*「try：... except：」は、あらゆる種類の「例外」またはエラーをキャッチします。これにより、それを処理でき、コードの実行が継続されます。
ここでいう例外を個別に扱うことができるように、指定することができれば最高だろう。
今のところ、その例外が何であるかを見つけられるように、表示することにします。
*このブロック内で `get_url（）`を呼び出し、コンテンツがある場合は、今のところは表示させます。エラーがある場合にも、表示させます。


このコードを実行すると、HTMLの内容をページから表示したり、もしエラーが有る場合にはそのエラーを表示するだけです。

```
python kickstarter-watcher.py https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
```

```
Checking: https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
... (some hard-to-read HTML)
```

次に必要なのは、そのHTMLの内容が意味のあるものにすることです。
それには、 `BeautifulSoup` を使います。

###  `BeautifulSoup`を使う

HTMLの内容の解析には、 `BeautifulSoup` ライブラリを使用します。
コマンドラインにこう入力しましょう。

```
pip install beautifulsoup4
```

これが、HTML解析をラクにしてくれるライブラリです。

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

* `from bs4 import BeautifulSoup as soup`は`BeautifulSoup`をインポートし、エイリアス`soup`に割り当てます。 書きやすくするためにこうしてみました。
* `get_url（）`は修正されて、成功時に `BeautifulSoup`のインスタンスを返すようになりました。
* `soup（response.content、 'html.parser'）`は、前述の `BeautifulSoup`インスタンスです。 HTMLコンテンツを渡し、HTMLパーサーを使用するように指示しました。
* `check_earlybird（）`関数が追加され、コンテンツを受け入れるようになりました。これは現在は `BeautifulSoup`インスタンスです。 この関数内で、興味のある特定のHTML要素を探します。
* `content.find_all（）`と `content.find（）`は、要素の検索を容易にするために `BeautifulSoup`によって提供されるメソッドです。 CSSクラスは、要素を選択するのに適した方法です。CSSは、要素をターゲットに特定する必要があるためです。 Kickstarterプロジェクトページテンプレートを調べて、使用されたクラス名を確認しました。 HTML要素内で `class =" name here "`として探すことができます。
* `sidebar`には選択可能な誓約オプションが多くあるため、` find_all（） `を使用し、誓約オプションのリストのHTMLを表します。
* `sidebar`内で` selectable`要素をループして、タイトルと統計を見つけます。 統計から、limit と Backer の数も取得できます。
* `.text.strip（）`は `title`と` limit`で使用され、スペースや改行などの先頭と末尾の文字を削除します。 ここで2つのことを行いました。テキストを取得し、その上で `strip（）`を呼び出しました。
* 次に、`limited`が存在するかどうかを確認します。これらの初期取引は通常、一定数の支援者に対応し、`Limited（N left of X）`として表示されます。
* `.lower（）`が使用されるのは、テキストが書かれているケースがわからないため、すべてが確実に小文字になるようにするためです。

このスクリプトをページで実行するには、プロジェクトページのURLを貼り付ける必要があります。

```
python kickstarter-watcher.py https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
```

```
Checking: https://www.kickstarter.com/projects/1183795653/tracxcroll-change-your-trackball-to-the-scroller
Early Bird! - Limited (96 left of 150)
```

これぞ、お話していたことです！

このスクリプトに最後に1つ追加するのが、通知です。 たとえば、"Limited（1 left of）"というフレーズが突然表示された場合、それまで満杯だった誓約パッケージに、突然空きスロットができたことを意味するかもしれません。それが、私が即座にに知りたいことです。

### `xoxzo.cloudpy`　を使う

通知を送るには、 `xoxzo.cloudpy` ライブラリを使います。 コマンドラインにこうタイプします。

```
pip install xoxzo.cloudpy
```

こちらが、SMSや、音声コールでさえも XoxzoのAPIを使って 簡単に行える　[Xoxzo クライアントライブラリ](https://github.com/xoxzo/xoxzo.cloudpy) です。

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
監視しているイベントはいつでも発生する可能性があるため、すぐに通知を受け取ることをお勧めします。 この場合、SMSメディアとしてSMSを選択し、できるだけ早く受信できるようにします。

* `from xoxzo.cloudpy import XoxzoClient` here we import the client into our script
* `send_notification()` function was added to use the Xoxzo client and send the message to the given number. To use the Xoxzo API, we need to [register an account](https://www.xoxzo.com/accounts/signup/) and get an API SID and TOKEN that we can use in our script.
* `XoxzoClient(sid=API_SID, auth_token=API_TOKEN)` here we use our credentials to create a Xoxzo client instance and assign it to the variable `xc` in the script.
* `result = xc.send_sms()` we simply use the `send_sms()` method of XoxzoClient.
* Then we modified our script so that when the text we're watching for appears, it sends a notification.
* `msgid = result.messages[0]['msgid']` we get the msgid so we can check the message status just in case. Our API allows you to do that and much more. Be sure to checkout our [documentation](https://docs.xoxzo.com/) to see what our API can offer.

And that is the full script. There are many ways to improve it but it should let you get started with learning Python, using our API, getting your website updates and become more productive. You can save it in a folder where you have all your useful scripts.

I can think of several ways to use this script like enclose it in a loop that runs at certain intervals and leave it running in the background. You can also use [cron](https://www.makeuseof.com/tag/linux-task-scheduling-crontab-explained/) for macOS or Linux to run it according to some schedule. You can also use [Task Scheduler](https://www.makeuseof.com/tag/4-boring-tasks-can-automate-windows-task-scheduler/) for Windows.

Have any fun or useful scripts to share with us? Let us know!
