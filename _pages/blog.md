---
layout: default
permalink: /blog/
title: Blog
nav: true
nav_order: 6
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 10
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="post">

<div class="blog-page">

  <!-- Compact header -->
  <div class="blog-header">
    <p class="blog-header-desc">{{ site.blog_description }}</p>
  </div>

  <!-- Topic filter bar -->
  {% if site.display_tags and site.display_tags.size > 0 %}
  <div class="blog-filter-bar">
    {% for tag in site.display_tags %}
      {% assign tag_posts = site.tags[tag] %}
      {% if tag_posts and tag_posts.size > 0 %}
      <a href="{{ tag | slugify | prepend: '/blog/tag/' | relative_url }}" class="filter-chip">
        {{ tag }}
      </a>
      {% endif %}
    {% endfor %}
  </div>
  {% endif %}

  <!-- Series / Categories -->
  {% if site.display_categories and site.display_categories.size > 0 %}
  <div class="blog-series-section">
    {% for category_name in site.display_categories %}
      {% assign cat_posts = site.posts | where_exp: "post", "post.categories contains category_name" %}
      {% if cat_posts.size > 0 %}
      <div class="series-block">
        <div class="series-header">
          <a href="{{ category_name | slugify | prepend: '/blog/category/' | relative_url }}" class="series-title-link">
            <h2 class="series-title">{{ category_name }}</h2>
          </a>
          <span class="series-count">{{ cat_posts.size }} post{% if cat_posts.size != 1 %}s{% endif %}</span>
        </div>
        <div class="series-posts">
          {% for post in cat_posts %}
            {% if post.external_source == blank %}
              {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
            {% else %}
              {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
            {% endif %}
            <a href="{{ post.url | relative_url }}" class="series-post-card">
              <div class="series-post-meta">
                <time>{{ post.date | date: '%b %d, %Y' }}</time>
                <span class="meta-sep">&middot;</span>
                <span>{{ read_time }} min read</span>
              </div>
              <h3 class="series-post-title">{{ post.title }}</h3>
              <p class="series-post-desc">{{ post.description }}</p>
              {% if post.tags.size > 0 %}
              <div class="series-post-tags">
                {% for tag in post.tags %}
                  <span class="inline-tag">#{{ tag }}</span>
                {% endfor %}
              </div>
              {% endif %}
            </a>
          {% endfor %}
        </div>
      </div>
      {% endif %}
    {% endfor %}
  </div>
  {% endif %}

  <!-- All Posts (paginated) -->
  <div class="blog-all-section">
    <h2 class="all-posts-title">All Posts</h2>

    {% if page.pagination.enabled %}
      {% assign postlist = paginator.posts %}
    {% else %}
      {% assign postlist = site.posts %}
    {% endif %}

    <div class="all-posts-list">
      {% for post in postlist %}
        {% if post.external_source == blank %}
          {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
        {% else %}
          {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
        {% endif %}

        <a href="{{ post.url | relative_url }}" class="all-post-row">
          <div class="all-post-date">
            {{ post.date | date: '%b %Y' }}
          </div>
          <div class="all-post-info">
            <h3 class="all-post-title">{{ post.title }}</h3>
            <p class="all-post-desc">{{ post.description | truncate: 120 }}</p>
          </div>
          <div class="all-post-read">
            {{ read_time }} min
            <i class="fa-solid fa-arrow-right fa-xs"></i>
          </div>
        </a>
      {% endfor %}
    </div>

    {% if page.pagination.enabled %}
      {% include pagination.liquid %}
    {% endif %}
  </div>

</div>

</div>
