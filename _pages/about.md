---
permalink: /
title: "About Me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html

poster:
  - image_path: poster_screenshot.png
    alt: "Poster thumbnail"
    title: '<span style="font-size:0.95rem;">Investigating How Small Transformers Learn Arithmetic: Self-Improvement and Beyond</span>'
    excerpt: This project was conducted during URPS2025, presented in both URPS2025 Poster Session and [CSE’s First Undergraduate Research Symposium](https://cse.engin.umich.edu/stories/cses-first-undergraduate-research-symposium-showcases-outstanding-work)
    url: /files/URPS_Poster_Undergrad_symposium.pdf   # 图片与标题都会链接到这个 PDF
    btn_label: "View Poster"
    btn_class: "btn--small"


---
<style>
  .btn {
    text-decoration: none !important;
  }
  .poster-top {
    margin-top: 2rem;   /* 改这里，2rem 大概等于两行字高 */
  }
  /* 把整块改成左右两栏的 flex 布局，并顶对齐 */
  .poster-top .archive__item {
    display: flex !important;
    align-items: flex-start !important;
    gap: 1rem;
  }

  /* 左栏：缩略图 */
  .poster-top .archive__item-teaser {
    flex: 0 0 35% !important;   /* 改这里的 30% 为你想要的比例 */
    max-width: 35% !important;
    margin: 0 !important;       /* 取消主题给图片的默认外边距 */
    float: none !important;     /* 关闭 float，不然会干扰布局 */
  }

  /* 右栏：文字主体 */
  .poster-top .archive__item-body {
    flex: 1 1 65% !important;   /* 与上面 30% 对应 */
    max-width: 65% !important;
  }

  /* 细节：标题/摘要的间距 */
  .poster-top .archive__item-title { margin-top: 0 !important; margin-bottom: 0rem !important; }
  .poster-top .archive__item-excerpt { margin-top: 0rem !important; }
</style>
Hi👋! I am a senior undergraduate student from the University of Michigan, Ann Arbor, majoring in Computer Science and Data Science. 

My research interest lies in the scientific understanding of foundation models and their applications. I am very grateful to be advised by [Prof. Wei Hu](https://weihu.me/). Currently, I am working on foundational understanding of the Transformer's length generalization ability. I am also fortunate to collaborate with [Zhiwei Xu](https://zhiweixx.github.io/). Previously, I was a member of [URPS](https://lsa.umich.edu/stats/undergraduate-students/undergraduate-research-opportunities-.html), gratefully supervised by [Prof. Yixin Wang](https://yixinwang.github.io/). 

 <br><br>

# Previous Project

<div class="poster-top">
  {% include feature_row id="poster" type="left" %}
</div>

# Misc

I am originally from Beijing, China. I love travelling during the vacations. I have been playing the accordion🪗 for over 15 years. 

<iframe src="https://www.google.com/maps/d/u/0/embed?mid=1qRlN8nTq61C61jmh1ZTKZ-FRoDox6ec&ehbc=2E312F&noprof=1" width="680" height="400"></iframe>