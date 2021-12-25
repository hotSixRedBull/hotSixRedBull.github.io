## Simplest setup with supported theme
This page is for testing how good default themes are.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="/github-pages-with-jekyll{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
