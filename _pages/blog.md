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
  per_page: 50
  sort_field: date
  sort_reverse: true
  trail:
    before: 1
    after: 3
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
      <div class="category-card" data-category="{{ category_name }}">
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
        </div>
      </div>
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
      <button class="tag-pill" data-tag="{{ tag }}">
        <span class="tag-pill-name">{{ tag }}</span>
        {% if tag_posts %}
        <span class="tag-pill-count">{{ tag_posts.size }}</span>
        {% endif %}
      </button>
    {% endfor %}
  </div>
</div>
{% endif %}

<!-- Back button (hidden by default) -->
<div class="blog-back-bar" style="display:none;">
  <button class="blog-back-btn"><i class="fa-solid fa-arrow-left fa-xs"></i> Show all</button>
</div>

<!-- All Posts Section -->
<div class="blog-posts-section">
  <h3 class="section-title"><i class="fa-solid fa-newspaper"></i> All Posts</h3>

  <ul class="post-list">
    {% assign postlist = site.posts %}

    {% for post in postlist %}
      {% if post.external_source == blank %}
        {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
      {% else %}
        {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
      {% endif %}
      {% assign year = post.date | date: "%Y" %}
      {% assign tags = post.tags | join: "" %}
      {% assign categories = post.categories | join: "" %}

      <li data-categories="{{ post.categories | join: '|||' }}" data-tags="{{ post.tags | join: '|||' }}">
        <div class="post-card">
          {% if post.thumbnail %}
          <div class="row">
            <div class="col-sm-9">
          {% endif %}

          <!-- Category badge -->
          {% if categories != "" %}
          <div class="post-card-categories">
            {% for category in post.categories %}
              <span class="post-category-badge">
                <i class="fa-solid fa-tag fa-xs"></i> {{ category }}
              </span>
            {% endfor %}
          </div>
          {% endif %}

          <h3>
            {% if post.redirect == blank %}
              <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
            {% elsif post.redirect contains '://' %}
              <a class="post-title" href="{{ post.redirect }}" target="_blank">{{ post.title }}</a>
            {% else %}
              <a class="post-title" href="{{ post.redirect | relative_url }}">{{ post.title }}</a>
            {% endif %}
          </h3>
          <p class="post-card-description">{{ post.description }}</p>
          <div class="post-card-footer">
            <p class="post-meta">
              <i class="fa-regular fa-clock fa-xs"></i> {{ read_time }} min read &nbsp; &middot; &nbsp;
              <i class="fa-regular fa-calendar fa-xs"></i> {{ post.date | date: '%B %d, %Y' }}
            </p>
            {% if tags != "" %}
            <div class="post-card-tags">
              {% for tag in post.tags %}
                <span class="post-tag-inline">#{{ tag }}</span>
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
</div>

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  var categoryCards = document.querySelectorAll('.category-card[data-category]');
  var tagPills = document.querySelectorAll('.tag-pill[data-tag]');
  var postItems = document.querySelectorAll('.post-list li');
  var backBar = document.querySelector('.blog-back-bar');
  var backBtn = document.querySelector('.blog-back-btn');
  var categoriesSection = document.querySelector('.blog-categories-section');
  var tagsSection = document.querySelector('.blog-tags-section');

  function showAll() {
    postItems.forEach(function(li) { li.style.display = ''; });
    backBar.style.display = 'none';
    categoriesSection.style.display = '';
    tagsSection.style.display = '';
    categoryCards.forEach(function(c) { c.classList.remove('active'); });
    tagPills.forEach(function(c) { c.classList.remove('active'); });
  }

  // Category card click → filter posts
  categoryCards.forEach(function(card) {
    card.addEventListener('click', function() {
      var cat = this.getAttribute('data-category');
      categoryCards.forEach(function(c) { c.classList.remove('active'); });
      this.classList.add('active');
      tagPills.forEach(function(c) { c.classList.remove('active'); });

      postItems.forEach(function(li) {
        var cats = li.getAttribute('data-categories').split('|||');
        li.style.display = cats.indexOf(cat) !== -1 ? '' : 'none';
      });

      backBar.style.display = 'flex';
    });
  });

  // Tag pill click → filter posts
  tagPills.forEach(function(pill) {
    pill.addEventListener('click', function() {
      var tag = this.getAttribute('data-tag');
      tagPills.forEach(function(c) { c.classList.remove('active'); });
      this.classList.add('active');
      categoryCards.forEach(function(c) { c.classList.remove('active'); });

      postItems.forEach(function(li) {
        var tags = li.getAttribute('data-tags').split('|||');
        li.style.display = tags.indexOf(tag) !== -1 ? '' : 'none';
      });

      backBar.style.display = 'flex';
    });
  });

  // Back button
  backBtn.addEventListener('click', function() {
    showAll();
  });
});
</script>
