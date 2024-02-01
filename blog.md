---
title: /blog
layout: page
permalink: /blog/
---

{%- if site.posts.size > 0 -%}
  <ul>
    {%- for post in site.posts -%}
      <li>
        {%- if post.categories == "CTF" -%}
          <h1> CTF </h1>
        {%- elsif post.categories == "Bug-Bounty" -%}
          <h1> Bug Bounty </h1>
        {%- endif -%}
        {%- assign date_format = "%m-%d-%Y" -%}
        [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </li>
    {%- endfor -%}
  </ul>
{%- endif -%}
