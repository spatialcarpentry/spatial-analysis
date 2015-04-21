---
layout: page
title: Hello World!
tagline: Supporting tagline
---
{% include JB/setup %}

## Goals/Objectives

After finishing this lesson, you will be able to:
  - understand basic spatial functions
  - know how to use spatial functions to analyze a dataset

## Topics

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## More Resources

