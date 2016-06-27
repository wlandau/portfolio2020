<ul>
  {% for post in site.posts limit:5 %}
  <li>
    <span>{{ post.date | date: "%b %d, %Y" }}</span>
    <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
  </li>
  {% endfor %}
  <li>
    <a href="{{ "/archives/" | prepend: site.baseurl }}">List all</a>
  </li>
</ul>