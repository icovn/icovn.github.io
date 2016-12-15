---
layout: post
title:  "Goodbye AOE, hello Jekyll"
date:   2016-12-10 18:10:14 +0700
categories: game technology
tags: aoe jekyll blog
---
Khoảng 2 tháng từ ngày thách thức anh DucTA5 solo Shang và sau vụ chơi cùng anh em đêm qua thì từ sáng nay mình cảm thấy không muốn chơi AOE nữa :neutral_face:.

1. Trình độ kém
2. Tốn thời gian

Thời gian dành cho những việc mình có thể giỏi và có ích tí. Ví dụ như code giỏi kiếm nhiều $ nuôi gia đình, đánh tennis giỏi thì chăm tập luyện nâng cao sức khoẻ, ...

Mình muốn viết blog một cái mà chưa khôi phục được account domain Yahoo trong khi hosting chuyển ip server :cry:. Google tí ra giải pháp này khá thú vị :grin::

- hosting blog trên [Github][link-github-page]
- quản lý nội dung, sinh HTML tĩnh bằng [Jekyll][link-jekyll] hoặc [Hexo][link-hexo] hoặc [Hugo][link-hugo]; ở đây mình chọn **Jekyll**

### Bước 1: Cài đặt github page
Xem hướng dẫn [ở đây][link-github-page]


### Bước 2: Cài đặt Jekyll
Xem hướng dẫn [ở đây][link-jekyll-install], với MacOS đơn giản là chạy các lệnh sau:
{% highlight ruby %}
#cài đặt
gem install jekyll bundler

#tạo project
bundle exec jekyll new icovn.github.io
cd icovn.github.io

#build
bundle exec jekyll build

#chạy
bundle exec jekyll serve
{% endhighlight %}
Truy cập link http://127.0.0.1:4000 xem nào, được đấy chứ :sweat_smile: :thumbsup:

<amp-img width="600" height="300" layout="responsive" src="{{ site.url }}/assets/161210-jekyll-homepage.png"></amp-img>


### Bước 3: Viết blog
Làm sao để tạo thêm 1 bài viết mới? Đọc [ở đây][link-jekyll-post] (đơn giản là thêm 1 file text viết bằng Markdown)

Sửa bài viết như thế nào?

- Đầu tiên cần đọc về [Front Matter][link-frontmatter].
- Sau đó tìm hiểu về [Markdown][link-markdown] ngôn ngữ markup viết bằng text thuần (không dùng thẻ tag nhưng có hỗ trợ thẻ tag của HTML) được thiết kế để chuyển sang HTML bằng công cụ cũng tên là **Markdown**. Ở đây mình sẽ dùng **Markdown** để viết blog.
- Tiếp đến là đọc về [template][link-jekyll-template] và engine [Liquid][link-liquid]


### Bước 4: Thêm mắm thêm muối
Về cơ bản làm theo các bước trên là mình đã có hệ thống blog chạy ngon rồi, như mình mình tìm hiểu thêm 1 số cái khác nữa để có nhiều chức năng hơn như:

- jekyll-paginate
- jekyll-sitemap
- jemoji
- jekyll-tagging
- [theme][link-amplify]

[link-github-page]: https://pages.github.com
[link-jekyll]: https://jekyllrb.com/
[link-frontmatter]: https://jekyllrb.com/docs/frontmatter/
[link-jekyll-install]: https://jekyllrb.com/docs/installation/
[link-jekyll-post]: https://jekyllrb.com/docs/posts/
[link-jekyll-template]: https://jekyllrb.com/docs/templates/
[link-hexo]: https://hexo.io/
[link-hugo]: https://gohugo.io/
[link-markdown]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[link-liquid]: https://help.shopify.com/themes/liquid
[link-amplify]: https://github.com/ageitgey/amplify
