---
layout: posts
title: "Recent posts"
---
{% include JB/setup %}

<div>
	{% for post in site.posts %}
		{% include JB/post_content %}
	{% endfor %}
</div>
  