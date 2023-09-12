---
layout: list
permalink: /ml/
title: "Machine Learning & Data Science"
excerpt: "Machine Learning and Data Science related posts"
---

<section>
  <ul>
    {% for post in site.categories.ML %}
      <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        <p class="font-italic text-muted">
          {{ post.date | date_to_string }} | {{ post.categories | join: ', ' }}
        </p>
      </li>
    {% endfor %}
  </ul>
</section>

