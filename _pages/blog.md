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
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="post">

{% assign blog_name_size = site.blog_name | size %}
{% assign blog_description_size = site.blog_description | size %}

{% if blog_name_size > 0 or blog_description_size > 0 %}
  <div class="header-bar">
    <h1>{{ site.blog_name }}</h1>
    <h2>{{ site.blog_description }}</h2>
  </div>
{% endif %}

<!-- Category Cards Section -->
{% if site.display_categories and site.display_categories.size > 0 %}
<div class="blog-categories-section">
  <h3 class="section-title"><i class="fa-solid fa-layer-group"></i> Categories</h3>
  <div class="category-cards-grid">
    {% for category_name in site.display_categories %}
      {% assign cat_posts = site.posts | where_exp: "post", "post.categories contains category_name" %}
      {% assign latest_post = cat_posts | first %}
      <a href="{{ category_name | slugify | prepend: '/blog/category/' | relative_url }}" class="category-card">
        <div class="category-card-inner">
          <div class="category-card-header">
            <span class="category-card-name">{{ category_name }}</span>
            <span class="category-card-count">{{ cat_posts.size }} post{% if cat_posts.size != 1 %}s{% endif %}</span>
          </div>
          {% if latest_post %}
          <div class="category-card-latest">
            <span class="category-card-latest-label">Latest:</span>
            <span class="category-card-latest-title">{{ latest_post.title | truncate: 60 }}</span>
          </div>
          {% endif %}
          <div class="category-card-arrow">
            <i class="fa-solid fa-arrow-right"></i>
          </div>
        </div>
      </a>
    {% endfor %}
  </div>
</div>
{% endif %}

<!-- Tag Pills Section -->
{% if site.display_tags and site.display_tags.size > 0 %}
<div class="blog-tags-section">
  <h3 class="section-title"><i class="fa-solid fa-hashtag"></i> Topics</h3>
  <div class="tag-pills">
    {% for tag in site.display_tags %}
      {% assign tag_posts = site.tags[tag] %}
      <a href="{{ tag | slugify | prepend: '/blog/tag/' | relative_url }}" class="tag-pill">
        <span class="tag-pill-name">{{ tag }}</span>
        {% if tag_posts %}
        <span class="tag-pill-count">{{ tag_posts.size }}</span>
        {% endif %}
      </a>
    {% endfor %}
  </div>
</div>
{% endif %}

<!-- Featured Posts Section -->
{% assign featured_posts = site.posts | where: "featured", "true" %}
{% if featured_posts.size > 0 %}
<div class="blog-featured-section">
  <h3 class="section-title"><i class="fa-solid fa-thumbtack"></i> Featured</h3>
  <div class="container featured-posts">
    {% assign is_even = featured_posts.size | modulo: 2 %}
    <div class="row row-cols-{% if featured_posts.size <= 2 or is_even == 0 %}2{% else %}3{% endif %}">
      {% for post in featured_posts %}
        <div class="col mb-4">
          <a href="{{ post.url | relative_url }}">
            <div class="card hoverable">
              <div class="row g-0">
                <div class="col-md-12">
                  <div class="card-body">
                    <div class="float-right">
                      <i class="fa-solid fa-thumbtack fa-xs"></i>
                    </div>
                    <h3 class="card-title text-lowercase">{{ post.title }}</h3>
                    <p class="card-text">{{ post.description }}</p>
                    {% if post.external_source == blank %}
                      {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
                    {% else %}
                      {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
                    {% endif %}
                    {% assign year = post.date | date: "%Y" %}
                    <p class="post-meta">
                      {{ read_time }} min read &nbsp; &middot; &nbsp;
                      <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}">
                        <i class="fa-solid fa-calendar fa-sm"></i> {{ year }} </a>
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </a>
        </div>
      {% endfor %}
    </div>
  </div>
</div>
{% endif %}

<!-- All Posts Section -->
<div class="blog-posts-section">
  <h3 class="section-title"><i class="fa-solid fa-newspaper"></i> All Posts</h3>

  <ul class="post-list">
    {% if page.pagination.enabled %}
      {% assign postlist = paginator.posts %}
    {% else %}
      {% assign postlist = site.posts %}
    {% endif %}

    {% for post in postlist %}
      {% if post.external_source == blank %}
        {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
      {% else %}
        {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
      {% endif %}
      {% assign year = post.date | date: "%Y" %}
      {% assign tags = post.tags | join: "" %}
      {% assign categories = post.categories | join: "" %}

      <li>
        <div class="post-card">
          {% if post.thumbnail %}
          <div class="row">
            <div class="col-sm-9">
          {% endif %}

          <!-- Category badge -->
          {% if categories != "" %}
          <div class="post-card-categories">
            {% for category in post.categories %}
              <a href="{{ category | slugify | prepend: '/blog/category/' | prepend: site.baseurl}}" class="post-category-badge">
                <i class="fa-solid fa-tag fa-xs"></i> {{ category }}
              </a>
            {% endfor %}
          </div>
          {% endif %}

          <h3>
            {% if post.redirect == blank %}
              <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
            {% elsif post.redirect contains '://' %}
              <a class="post-title" href="{{ post.redirect }}" target="_blank">{{ post.title }}</a>
              <svg width="2rem" height="2rem" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
                <path d="M17 13.5v6H5v-12h6m3-3h6v6m0-6-9 9" class="icon_svg-stroke" stroke="#999" stroke-width="1.5" fill="none" fill-rule="evenodd" stroke-linecap="round" stroke-linejoin="round"></path>
              </svg>
            {% else %}
              <a class="post-title" href="{{ post.redirect | relative_url }}">{{ post.title }}</a>
            {% endif %}
          </h3>
          <p class="post-card-description">{{ post.description }}</p>
          <div class="post-card-footer">
            <p class="post-meta">
              <i class="fa-regular fa-clock fa-xs"></i> {{ read_time }} min read &nbsp; &middot; &nbsp;
              <i class="fa-regular fa-calendar fa-xs"></i> {{ post.date | date: '%B %d, %Y' }}
              {% if post.external_source %}
                &nbsp; &middot; &nbsp; {{ post.external_source }}
              {% endif %}
            </p>
            {% if tags != "" %}
            <div class="post-card-tags">
              {% for tag in post.tags %}
                <a href="{{ tag | slugify | prepend: '/blog/tag/' | prepend: site.baseurl}}" class="post-tag-inline">
                  #{{ tag }}
                </a>
              {% endfor %}
            </div>
            {% endif %}
          </div>

          {% if post.thumbnail %}
            </div>
            <div class="col-sm-3">
              <img class="card-img" src="{{post.thumbnail | relative_url}}" style="object-fit: cover; height: 90%" alt="image">
            </div>
          </div>
          {% endif %}
        </div>
      </li>
    {% endfor %}
  </ul>

  {% if page.pagination.enabled %}
    {% include pagination.liquid %}
  {% endif %}
</div>

</div>
