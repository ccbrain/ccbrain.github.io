---
layout: people
title: "Our team"
permalink: /people/
---

{% assign people_sorted = site.people | sort: 'joined' %}
{% assign people_array = "pi|postdoc|phd|ra|intern|visiting|others" | split: "|" %}

<section class="menuaa">
<div>
<h1>Current members</h1>
{% for item in people_array %}
  {% for profile in people_sorted %}
    {% if profile.position contains item %}
		<figure>
        {% if profile.avatar %}
        <a href="{{ site.baseurl }}{{ profile.url }}"><img class="author-img" src="{{site.baseurl}}/images/people/{{profile.avatar}}"></a>
        {% else %}
        <a href="{{ site.baseurl }}{{ profile.url }}"><img class="author-img" src="{{site.baseurl}}/images/avatar.jpg"></a>
        {% endif %}
        <figurecaption><a class="name" href="{{ site.baseurl }}{{ profile.url }}">{{ profile.title }}</a>
		{% if item == 'postdoc' %}
		Postdoctoral Fellow
		{% elsif item == 'pi' %}
		Principal Investigator
		{% elsif item == 'phd' %}
		PhD Student
		{% elsif item == 'ra' %}
		Research Assistant
		{% elsif item == 'visiting' %}
		Visiting Scholar
		{% elsif item == 'intern' %}
		Research Intern
		{% endif %}
		</figurecaption>
		</figure>
    {% endif %}
  {% endfor %}
{% endfor %}
</div>
</section>
