# View my live blog

[andongshen.com](https://andongshen.com/)

# How to build

This blog project is cloned from [huxblog-boilerplate](https://https://github.com/Huxpro/huxpro.github.io).You can find a good instruction there.

The project is based on <a href="https://pages.github.com/">GitHub Pages</a> and <a href="http://jekyll.com.cn/">Jekyll</a>.You don't need a private web server.It is very easy and cheap to build the blog web.

# How to be Https

I use a free special CDN serve to deploy the SSL certification. The service names [cloudflare](https://www.cloudflare.com). This service also has rewrite function. It is very good and easy to use.

# Troubles

> About some troubles when I build the blog.

- Can't use chinese for posts file name ,or it shows 404.

- When I change the permalink setting in config, local serve works will, but git blog can't show tag and about page. I don't know the reason. So just set ```permalink: pretty```, don't change it.
