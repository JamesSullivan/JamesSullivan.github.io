---
layout: home
permalink: /james

title: "james.sullivan at solutions.asia"
excerpt: "Keeping busy with the intersection of Programming, ML, and Data Science"
action: true

feature_rows:
  - title: "Blog"
    excerpt: "Blog: Programming, ML, and DS"
    url: "/blog/"
    img_path: "feature_rows/pexels-pixabay-blog.jpg"
    img_alt: "Everything"
  - title: "Jupyter"
    excerpt: "Jupyter Notebooks"
    url: "/notebooks/"
    img_path: "feature_rows/Jupyter620.png"
    img_alt: "Google Colab"
  - title: "Dev"
    excerpt: "Links for Developers & Programmers"
    url: "/dev_links/"
    img_path: "feature_rows/vscode620.png"
    img_alt: "Programming"
  - title: "DS & ML"
    excerpt: "Data Science & Machine Learning Links"
    url: "/ml_links/"
    img_path: "feature_rows/dev_links620.png"
    img_alt: "Data Science and Machine Learning Links"
  - title: "LLMs"
    excerpt: "Links for Large Language Models"
    url: "/llm_links/"
    img_path: "feature_rows/financial620.jpg"
    img_alt: "Dev & ML Links"
  - title: "Math Links"
    excerpt: "Links for Math & Finance"
    url: "/math_links/"
    img_path: "feature_rows/Math620.jpg"
    img_alt: "Math & Finance Links"
---

## Introduction

A site for my blog, Jupyter notebooks, and various links associated with <span class="badge badge-primary">development</span>, <span class="badge badge-primary">machine learning</span> and <span class="badge badge-primary">data science</span>.

<br>
## Latest
<section>
  {% for post in site.posts limit:1 %}
    <a href="{{ post.url }}">
    <h3>{{ post.title }}</h3>
    <p class="blogdate">{{ post.date | date: "%d %B %Y" }}</p>
    <div>{{ post.content |truncatehtml | truncatewords: 150 }}</div>
    </a>
  {% endfor %}
</section>