+++
title = 'HugoとGithub Pagesで作ったブログににカスタムドメインを設定した'
date = 2024-04-29T13:38:18+09:00
draft = false
+++

## HugoとGithub Pagesで作ったブログににカスタムドメインを設定しました

### 大まかな流れ
1. ドメインを購入する
2. github pagesにドメインを検証する
3. github pagesの設定してるリポジトリにcustom domainを設定する
4. hugoのconfigファイルを修正する

### ドメインを購入する

購入はどこのレジストラからでも良いですが自分はcloudflareで購入しました。購入時の金額はお名前ドットコムの方が安いのですがドメインの更新料と、サイトの使いやすさから長期運用するドメインはcloudflareで買う様にしています

> ※注意点として楽天クレジットで購入しようとすると2024年4月29日時点では不正利用と誤検知されて購入に失敗します。電話かチャットボットに問い合わせてセキュリティを緩める必要があるのですがどうにかならないですかね。。

### github pagesの設定してるリポジトリにcustom domainを設定する

以下のドキュメント通りに進めます
[Verifying your custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)

このようにコマンドを送って設定した値が帰ってきたらokです
```
$ dig _github-pages-challenge-satouyuuki.shafu-life.win +nostats +nocomments +nocmd TXT


; <<>> DiG 9.10.6 <<>> _github-pages-challenge-satouyuuki.shafu-life.win +nostats +nocomments +nocmd TXT
;; global options: +cmd
;_github-pages-challenge-satouyuuki.shafu-life.win. IN TXT
_github-pages-challenge-satouyuuki.shafu-life.win. 300 IN TXT "<設定した値>"
```

### github pagesにcustom domainを設定する

以下のドキュメント通りに進めます
[Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

このようにコマンドを送って設定したCNAMEとAレコードの値が帰ってきたらokです
```
$ dig www.shafu-life.win +nostats +nocomments +nocmd

; <<>> DiG 9.10.6 <<>> www.shafu-life.win +nostats +nocomments +nocmd
;; global options: +cmd
;www.shafu-life.win.		IN	A
www.shafu-life.win.	300	IN	CNAME	satouyuuki.github.io.
satouyuuki.github.io.	3600	IN	A	185.199.109.153
satouyuuki.github.io.	3600	IN	A	185.199.110.153
satouyuuki.github.io.	3600	IN	A	185.199.111.153
satouyuuki.github.io.	3600	IN	A	185.199.108.153
```
なかなかカスタムドメインのDNS検証が成功にならなかったのですが根気よくトライすると成功しました
![addcustomdomain.png](/images/addcustomdomain.png)



### hugoのconfigを修正する
hugoのサイトをgithub pagesで公開するとURLの第一パスがレポジトリ名になってしまうので、サイト全体を相対パスに設定します。こんな感じです

```
$ git diff HEAD^^ HEAD hugo.toml
diff --git a/hugo.toml b/hugo.toml
index f1a4792..6c0dd64 100644
--- a/hugo.toml
+++ b/hugo.toml
@@ -1,4 +1,5 @@
-baseURL = 'https://satouyuuki.github.io/shafu_blog/'
+baseURL = 'https://shafu-life.win/'
+relativeURLs = true
 title = 'shafuブログ'
 theme = 'ananke'
```

### 参考
- https://discourse.gohugo.io/t/understanding-relative-urls/26012/2
- https://gohugo.io/content-management/urls/#relative-urls
