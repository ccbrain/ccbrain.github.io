---
layout: default
title: "Our team"
permalink: /people/alumni
---
{% assign people_sorted = site.people | sort: 'joined' %}
{% assign people_array = "alumni" | split: "|" %}
<section class="menuaa">
<div>
<h1>Alumni</h1>
</div>
</section>
<ul>
{% for item in people_array %}
  {% for profile in people_sorted %}
    {% if profile.position contains item %}
        <li >{{ profile.title }}
        {% if profile.oldposition == 'postdoc' %}
        (Postdoctoral Fellow,
        {% elsif profile.oldposition == 'pi' %}
        (Principal Investigator,
        {% elsif profile.oldposition == 'phd' %}
        (PhD Student,
        {% elsif profile.oldposition == 'ra' %}
        (Research Assistant,
        {% elsif profile.oldposition == 'visiting' %}
        (Visiting Scholar,
        {% elsif profile.oldposition == 'visitstudent' %}
        (Visiting Student,
        {% elsif profile.oldposition == 'intern' %}
        (Research Intern,
        {% endif %}{{profile.year}})

		<!--<li ><a href="{{ site.baseurl }}{{ profile.url }}">{{ profile.title }}
		{% if profile.oldposition == 'postdoc' %}
		(Postdoctoral Fellow,
		{% elsif profile.oldposition == 'pi' %}
		(Principal Investigator,
		{% elsif profile.oldposition == 'phd' %}
		(PhD Student,
		{% elsif profile.oldposition == 'ra' %}
		(Research Assistant,
		{% elsif profile.oldposition == 'visiting' %}
		(Visiting Scholar,
		{% elsif profile.oldposition == 'intern' %}
		(Research Intern,
		{% endif %}{{profile.year}})</a>-->
		
		
		</li>
		<!--<figure>
        {% if profile.avatar %}
        <a href="{{ site.baseurl }}{{ profile.url }}"><img class="author-img" src="{{site.baseurl}}/images/people/{{profile.avatar}}"></a>
        {% else %}
        <a href="{{ site.baseurl }}{{ profile.url }}"><img class="author-img" src="{{site.baseurl}}/images/avatar.jpg"></a>
        {% endif %}
        <figurecaption><a class="name" href="{{ site.baseurl }}{{ profile.url }}">{{ profile.title }}</a>
		</figurecaption>
		</figure>-->
    {% endif %}
  {% endfor %}
{% endfor %}
</ul>

