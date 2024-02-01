---
title: /blog
layout: page
permalink: /blog/
---

{%- if site.posts.size > 0 -%}
  <ul>
    {%- for post in site.posts -%}
    <li>
      # CTFs
      {%- if post.categories == "CTF" -%}
        {%- assign date_format = "%m-%d-%Y" -%}
        [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      {%- endif -%}
      # Bug Bounty
      {%- if post.categories == "Bug-Bounty" -%}
        {%- assign date_format = "%m-%d-%Y" -%}
        [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      {%- endif -%}
    </li>
    {%- endfor -%}
  </ul>
{%- endif -%}
