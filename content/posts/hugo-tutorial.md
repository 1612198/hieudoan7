+++
#toml
draft = "false"
date = 2019-04-19T15:01:41+07:00
title = "Hugo Tutorial"
+++
# Hugo Tutorial
18.06.2019

## I. Install Hugo & Setup Theme 
2 best tutorial about hugo

[part 1](https://www.youtube.com/watch?v=oUjk6wpJn7I)<br/>
[part 2](https://www.youtube.com/watch?v=4W9sVoVYMLo)

Về cơ bản, chúng ta host blog lên github sẽ tạo 2 repository

* Repo 1: hieudoan7: chứa các file config của hugo, content, nói chung tất tần tật.
* Repo 2: 1612198.github.io: repo này chứa file html để render lên web

**Chú ý**: dateformat trong file config.toml (ở thư mục gốc, thư mục mà ta có thể sử dụng hugo command) phải để ở ngày 2.1.2006 ở bất kỳ format nào (vd: "January 02, 2006") (hình như là theo quy định của Golang :v)

  
## II. Post & Deploy
- Tạo post <br/>
  `hugo new posts/mypost.md`
- Chỉnh sửa config toml file `mypost.md`
    + draft = "false"
    + title = "My Title post"
- Add nội dung bài post dưới phần toml header
- Chạy trên local host để check:   <br/>
  `hugo server -w (--watch)`
- Deploy qua `1612198.github.io` để nó render lên web   <br/>
  ```bash
  hugo -d (--deploy) ../1612198.github.io/
  ```
- Push 2 repo lên github để host là được.   <br/>
  `git config --global user.name "1612198`  <br/>
  `git config --global user.gmail "hieudoan190598@gmail.com"`                <br/>
  `git config --list`                         <br/>
  Vào từng cái repo (trong 2 repo đã có file .git rồi nên ko cần dùng `git remote`)     <br/>
  `git add .`           <br/>
  `git commit -m "comment for your commit"`     <br/>
  `git push origin master`

## III. Add comment componet
[awesome guide](https://portfolio.peter-baumgartner.net/2017/09/10/how-to-install-disqus-on-hugo/) 
## IV. Hugo Tips
### 1. Insert Image in Hugo post
- Bỏ image vào folder static (cùng `config.toml`)
- Link đường dẫn image trong post (`/imgs/my_image.jpg`) (thư mục gốc `/` là tương đương với `static/`)

### 2. Xuống dòng
- 2 dấu cách (giống trên stackoverflow, codeforces)

### 3. Math Expression
Dùng **MathJax**

- Tạo file `mathjax_support.html` trong `theme/layouts/partials/`  
 
```html
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
  MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
  });
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
  });

  MathJax.Hub.Config({
  // Autonumbering by mathjax
  TeX: { equationNumbers: { autoNumber: "AMS" } }
  });
</script>
```
- Add file này vào `footer.html` or `header.html` (trước thẻ `</footer>`)  
  
```html
{{ partial "mathjax_support.html" . }}
```
Nguồn: https://divadnojnarg.github.io/blog/mathjax/