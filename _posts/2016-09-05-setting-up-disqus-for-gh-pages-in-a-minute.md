---
layout:   post
title:    Setting up Disqus on GH-pages in a minute
tags:     [disqus]
comments: true
---

1 Sign up at https://disqus.com/

2 Since GH-pages is not on the list, get *Universal Code*


![disqus1](http://i.imgur.com/AbaPYLo.png)

3 Enter your gh-pages account. Disqus will create something like, `your-gh-pages-account.disqus.js`

![disqus2](http://i.imgur.com/TP6ayxR.png)

4 Disqus will give you a link to copy + paste. This link is really a JS code to display disqus on your site

![diqus1](http://i.imgur.com/04FBls2.png)

5 Create a folder on `_includes` and name it, say, `comments.html`. Inside paste the code from disqus. To turn on and off comments, add:

```
 if page.comments
 <copy+paste disqus code here>
 endif
```

6 On `_layouts`' `post.html` (or `page.html` or `default.html`) add `include comments.html` (wherever section you wish to have your disqus comments)

7 To display comments:
```
---
layout:   post
title:    Setting up Disqus on GH-pages in a minute
tags:     [disqus]
comments: true
---
```

Likewise, `comments: false` to disable it.

Easy-peasy.
