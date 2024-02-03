---
title: /blog
layout: page
permalink: /blog/
---

<h1> Bug Bounty </h1>
{%- if site.posts.size > 0 -%}
  <ul>
    {%- for post in site.posts -%}
      {%- if post.categories contains "Bug-Bounty" -%}
      <li>
        {%- assign date_format = "%m-%d-%Y" -%}
        [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </li>
      {%- endif -%}
    {%- endfor -%}
  </ul>
{%- endif -%}

<h1> CTF </h1>
{%- if site.posts.size > 0 -%}
  <ul>
    {%- for post in site.posts -%}
      {%- if post.categories contains "CTF" -%}
      <li>
        {%- assign date_format = "%m-%d-%Y" -%}
        [ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </li>
      {%- endif -%}
    {%- endfor -%}
  </ul>
{%- endif -%}