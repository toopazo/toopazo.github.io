---
layout: archive
title: "Publicaciones"
permalink: /publications/
author_profile: true
---

{% include base_path %}

Ver publicaciones en Google Scholar (link en menu a la izquierda)

<u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>


<!--{% if author.googlescholar %}-->
<!--  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>-->
<!--{% endif %}-->

<!--{% include base_path %}-->

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
