+++
title = "Setting Up a Custom Domain for a Blog Created with Hugo and Github Pages"
date = 2024-04-29T13:39:24+09:00
draft = false
+++

## Setting Up a Custom Domain for a Blog Created with Hugo and Github Pages

### General Procedure
1. Purchase a domain.
2. Verify the domain with GitHub Pages.
3. Set up custom domain in the GitHub Pages repository settings.
4. Modify the config file of Hugo.

### Purchasing a Domain

You can purchase from any registrar, but I chose to buy from Cloudflare. Although Name.com is cheaper at the time of purchase, I prefer to buy domains from Cloudflare for long-term use due to domain renewal fees and ease of use of the site.

> **Note:** If you try to purchase with Rakuten Credit, as of April 29, 2024, it will be flagged as fraudulent activity and the purchase will fail. You will need to contact them via phone or chatbot to loosen security measures. Is there any way around this?

### Setting Up Custom Domain in the GitHub Pages Repository

Follow the documentation below:
[Verifying your custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)

Once you receive the configured values after running the command, it's okay:
```
$ dig _github-pages-challenge-satouyuuki.shafu-life.win +nostats +nocomments +nocmd TXT

; <<>> DiG 9.10.6 <<>> _github-pages-challenge-satouyuuki.shafu-life.win +nostats +nocomments +nocmd TXT
;; global options: +cmd
;_github-pages-challenge-satouyuuki.shafu-life.win. IN TXT
_github-pages-challenge-satouyuuki.shafu-life.win. 300 IN TXT "<configured value>"
```

### Setting Up Custom Domain in GitHub Pages

Follow the documentation below:
[Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

Once you receive the values of the configured CNAME and A records after running the command, it's okay:
```
$ dig www.shafu-life.win +nostats +nocomments +nocmd

; <<>> DiG 9.10.6 <<>> www.shafu-life.win +nostats +nocomments +nocmd
;; global options: +cmd
;www.shafu-life.win.     IN A
www.shafu-life.win. 300  IN CNAME satouyuuki.github.io.
satouyuuki.github.io. 3600 IN A 185.199.109.153
satouyuuki.github.io. 3600 IN A 185.199.110.153
satouyuuki.github.io. 3600 IN A 185.199.111.153
satouyuuki.github.io. 3600 IN A 185.199.108.153
```
It took some patience, but the custom domain DNS verification eventually succeeded.
![addcustomdomain.png](/images/addcustomdomain.png)

### Modifying Hugo Configurations
When you publish a Hugo site on GitHub Pages, the first path of the URL becomes the repository name. To set the entire site to relative paths, do the following:

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

### References
- [Understanding Relative URLs](https://discourse.gohugo.io/t/understanding-relative-urls/26012/2)
- [Hugo Documentation: Relative URLs](https://gohugo.io/content-management/urls/#relative-urls)

