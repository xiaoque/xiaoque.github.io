---
layout: page
type: index
title: X,que
---
{% include JB/setup %}

<div class="posts">
    {% for post in site.posts  %}
      {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
      {% capture this_month %}{{ post.date | date: "%B" }}{% endcapture %}
      {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}
      {% capture next_month %}{{ post.previous.date | date: "%B" }}{% endcapture %}

      {% if forloop.first %}
        <h3 class="index-year">{{this_year}}</h3>
        <h4 class="index-month"><hr/>{{this_month}}</h4>
        <ul>
      {% endif %}

      <li> <div class="index-post-list-name"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a> </div><span class="index-post-list-date">{{ post.date | date: "%B %e, %Y" }}</span> </li>

      {% if forloop.last %}
        </ul>
      {% else %}
        {% if this_year != next_year %}
          </ul>
          <h3 class="index-year"><hr/>{{next_year}}</h3>
          <h4 class="index-month"><hr/>{{next_month}}</h4>
          <ul>
        {% else %}
          {% if this_month != next_month %}
            </ul>
            
            <h4 class="index-month"><hr/>{{next_month}}</h4>
            <ul>
          {% endif %}
        {% endif %}
      {% endif %}
    {% endfor %}
</div>

<div class="index-right">
<div class="index-category">
    <div class="index-category-title"> Categories </div>
  {% assign categories_list = site.categories %}
  {% include JB/categories_list %}
</div>

<div class="index-tag">
    <div class="index-tag-title"> Tags </div>
  {% assign tags_list = site.tags %}  
  {% include JB/tags_list %}
</div>
</div>