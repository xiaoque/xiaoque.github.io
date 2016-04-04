---
layout: page
type: index
title: X,que
---
{% include JB/setup %}

<div class="posts numberlist">
    {% for post in site.posts  %}
      {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
      {% capture this_month %}{{ post.date | date: "%B" }}{% endcapture %}
      {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}
      {% capture next_month %}{{ post.previous.date | date: "%B" }}{% endcapture %}

      {% if forloop.first %}
        <span class="index-year">{{this_year}}</span>
        <span class="index-month">{{this_month}}</span>
        <ol>
      {% endif %}

      <li> <div class="index-post-list-name"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }} </a> </div> </li><div class="index-post-description">{{ post.description }} </div>

      {% if forloop.last %}
        </ol>
      {% else %}
        {% if this_year != next_year %}
          </ol>
          <span class="index-year"><hr/>{{next_year}}</span>
          <span class="index-month">{{next_month}}</span>
          <ol>
        {% else %}
          {% if this_month != next_month %}
            </ol>
            
            <span class="index-month">{{next_month}}</span>
            <ol>
          {% endif %}
        {% endif %}
      {% endif %}
    {% endfor %}
</div>

<div class="index-right">
<div class="category-title"> Categories </div>
<div class="cate_box inline">
  {% assign categories_list = site.categories %}
  {% include JB/categories_list %}
</div>
<hr/>
<div class="tag-title"> Tags </div>
<div class="tag_box inline">
  {% assign tags_list = site.tags %}  
  {% include JB/tags_list %}
</div>
</div>