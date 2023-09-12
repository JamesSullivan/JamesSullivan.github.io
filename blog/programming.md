---
layout: list
permalink: /programming/
title: "Programming Category"
excerpt: "List of programming related posts"
---


<section>
  <ul>
    {% for post in site.categories.programming %}
      <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        <p class="font-italic text-muted">
          {{ post.date | date_to_string }} | {{ post.categories | join: ', ' }}
        </p>
      </li>
    {% endfor %}
  </ul>
</section>

