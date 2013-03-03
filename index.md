---
layout: posts
title: "Recent posts"
---
{% include JB/setup %}
<div class="row-fluid">
	{% for post in site.posts %}
		{% include JB/post_content %}
	{% endfor %}
</div>
  