---
layout: page
---
#夜惊心的博客

<ul class="posts">
  {% for post in site.posts %}
    <li>
        <div class="post-list">
          <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a> <span class="post-time">{{ post.date | date_to_string }}</span>
        </div>
        <div class="excerpt">{{ post.content | split:'<!--more-->' | first }}</div>
        <p class="more"><a href="{{ post.url }}">继续阅读..</a></p>
	</li>
	<hr>
  {% endfor %}
</ul>



