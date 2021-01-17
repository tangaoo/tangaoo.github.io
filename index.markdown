---
layout: default
title: Tangaoo's Corner
---

Hi there, I am Tangaoo, an Linux/DSP/C programmer. 

This site is dedicated to providing notes of my work.


<p><br /><b>My Blog:</b></p>
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>

<p><b>Find me on:</b></p>

<ul>

<li><a href="http://github.com/happypeter/">Github</a></li>

</ul>
<p><br /><b>Contact Information:</b></p>

<blockquote>
你的指尖有改变世界的力量～
</blockquote>

[oss]:http://en.wikipedia.org/wiki/Open_source
