---
layout: archive
title: "journal articles"
permalink: /publications/
author_profile: true
---

{% for work in site.data.publications %}
  <div id="container">
    <div id="left">
        <b>{{ forloop.rindex }}.</b>
        <quotations>❝</quotations>{{ work.title }}<quotations>❞</quotations><br>
        {{ work.authors }}.<br>
        <i>{{ work.journal }}</i>. ({{ work.date }}) <a href="{{ work.url }}">DOI</a>. <i>{{ work.other }}</i>
        <br>
    </div>
    <div class="clear"></div>
  </div>
  <hr>
{% endfor %}

---

# book chapters

{% for work in site.data.books %}
  <div id="container">
    <div id="left">
        <b>{{ forloop.rindex }}.</b>
        <quotations>❝</quotations>{{ work.title }}<quotations>❞</quotations> in <quotations>❝</quotations>{{ work.booktitle }}<quotations>❞</quotations><br>
        {{ work.authors }}.<br>
        Chapter: {{ work.chapter }}. ({{ work.date }}) <a href="{{ work.url }}">DOI</a>. {{ work.other }}
        <br>
    </div>
    <div class="clear"></div>
  </div>
  <hr>
{% endfor %}

