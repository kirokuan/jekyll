---
layout: page
title: tag

---

<div class="page-content wc-container">
	<div class="post">
		<h1>Tags</h1>  
		<ul>
			{% for post in site.tags.question limit: 20 %}
			<li>  <a href="{{ post.url }}">{{ post.title }}</a></li>
			{% endfor %}
		</ul>
	</div>
</div>
