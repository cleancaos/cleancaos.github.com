---
layout: page
title: "Now here. Nowhere."
description: ""
---
{% include JB/setup %}

{% for post in site.posts limit: 10 %}
<div class="row-fluid">
  <div class="span12">
    <h2> <a href="{{ post.url }}">{{ post.title }}</a></h2>
    <!-- <h4>{{ post.date | date_to_long_string }}</h4>   -->
    <br/>
  </div>
</div>

{% endfor %}
