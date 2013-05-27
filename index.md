---
layout: page
---
#夜惊心的博客

<ul class="posts">
  {% for post in site.posts %}
    <li>
        <div class="post-list">
          <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a><span class="post-time"> &raquo; {{ post.date | date_to_string }}</span>
        </div>
        <p>{{ post.content | postmorefilter: post.url, "继续阅读.." }}</p>  
	</li>
	<hr>
  {% endfor %}
</ul>



