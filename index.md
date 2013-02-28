---
layout: posts
title: "Recent posts"
---
{% include JB/setup %}
<div class="row-fluid posts-summary">
	{% for post in site.posts %}
	{% if forloop.index == 1 or forloop.index == 5 or forloop.index == 9 or forloop.index == 13 %}
		{% if forloop.index > 1 %}
			</div>
		{% endif %}
		<div class="span12">
	{% endif %}
	{% include JB/post_content %}
	{% endfor %}
	</div> <!-- span12 -->
</div>
  