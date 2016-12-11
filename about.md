---
layout: page
title: About
permalink: /about/
---

Southern-born nerd before it was cool, husband of one wife, card-carrying Catholic convert, a professional system administrator, known to wear lumberjack shirts, boots, and PPE when actually felling trees and splitting firewood, a proponent of playing outside, the swivel-arm battle grip, reading, learning, making, Linux, Liberty, and the Oxford comma.

<br>
<br>
<p>
<h1> A Word Cloud of My Posts </h1>

  <div class="well">
    <p>
    {% for tag in site.tags %}
       <span class="tag" style="font-size: {{ tag | last | size | times: 1000 | divided_by: site.tags.size | plus: 100 }}%">
      {{ tag | first | replace: ' ', '-'}}
      </span>
    {% endfor %}
  </p>
  </div> <!-- /.well -->



