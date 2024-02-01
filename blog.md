---
title: /blog
layout: page
permalink: /blog/
---

{%- if site.posts.size > 0 -%}
  <ul>
    {%- for post in site.posts -%}
      <li>
      <h1> CTF </h1>
        {%- if post.categories contains "CTF" -%}
          {%- assign date_format = "%m-%d-%Y" -%}
          [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </li>
      <li>
      <h1> Bug Bounty </h1>
        {%- elsif post.categories contains "Bug-Bounty" -%}
          {%- assign date_format = "%m-%d-%Y" -%}
          [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        {%- endif -%}
      </li>
    {%- endfor -%}
  </ul>
{%- endif -%}
