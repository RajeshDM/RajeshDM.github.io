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
      <button class="filter-chip" data-filter="tag" data-value="{{ tag }}">
        {{ tag }}
      </button>
      {% endif %}
    {% endfor %}
  </div>
  {% endif %}

  <!-- Back button (hidden by default) -->
  <div class="blog-back-bar" style="display:none;">
    <button class="blog-back-btn">&larr; Show all categories</button>
  </div>

  <!-- Series / Categories -->
  {% if site.display_categories and site.display_categories.size > 0 %}
  <div class="blog-series-section">
    {% for category_name in site.display_categories %}
      {% assign cat_posts = site.posts | where_exp: "post", "post.categories contains category_name" %}
      {% if cat_posts.size > 0 %}
      <div class="series-block" data-category="{{ category_name }}">
        <div class="series-header" data-category="{{ category_name }}">
          <h2 class="series-title">{{ category_name }}</h2>
          <span class="series-count">{{ cat_posts.size }} post{% if cat_posts.size != 1 %}s{% endif %}</span>
        </div>
        <div class="series-posts">
          {% for post in cat_posts %}
            {% if post.external_source == blank %}
              {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
            {% else %}
              {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
            {% endif %}
            <a href="{{ post.url | relative_url }}" class="series-post-card"
               data-tags="{{ post.tags | join: '|||' }}">
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

</div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  var seriesBlocks = document.querySelectorAll('.series-block');
  var seriesHeaders = document.querySelectorAll('.series-header');
  var backBar = document.querySelector('.blog-back-bar');
  var backBtn = document.querySelector('.blog-back-btn');
  var filterChips = document.querySelectorAll('.filter-chip');
  var filterBar = document.querySelector('.blog-filter-bar');
  var activeTag = null;

  // Click a series header → show only that category
  seriesHeaders.forEach(function(header) {
    header.addEventListener('click', function() {
      var cat = this.getAttribute('data-category');
      seriesBlocks.forEach(function(block) {
        if (block.getAttribute('data-category') === cat) {
          block.style.display = '';
        } else {
          block.style.display = 'none';
        }
      });
      backBar.style.display = 'flex';
      // Clear tag filter when selecting category
      activeTag = null;
      filterChips.forEach(function(c) { c.classList.remove('active'); });
      showAllPostsInVisibleBlocks();
    });
  });

  // Back button → show all categories
  backBtn.addEventListener('click', function() {
    showAll();
  });

  function showAll() {
    seriesBlocks.forEach(function(block) {
      block.style.display = '';
    });
    backBar.style.display = 'none';
    activeTag = null;
    filterChips.forEach(function(c) { c.classList.remove('active'); });
    showAllPostsInVisibleBlocks();
  }

  function showAllPostsInVisibleBlocks() {
    document.querySelectorAll('.series-post-card').forEach(function(card) {
      card.style.display = '';
    });
  }

  // Tag filter chips
  filterChips.forEach(function(chip) {
    chip.addEventListener('click', function() {
      var tag = this.getAttribute('data-value');

      // Toggle: clicking same tag again clears filter
      if (activeTag === tag) {
        showAll();
        return;
      }

      activeTag = tag;
      filterChips.forEach(function(c) { c.classList.remove('active'); });
      this.classList.add('active');

      // Show all series blocks, but hide posts that don't match the tag
      seriesBlocks.forEach(function(block) {
        block.style.display = '';
        var cards = block.querySelectorAll('.series-post-card');
        var hasVisible = false;
        cards.forEach(function(card) {
          var tags = card.getAttribute('data-tags').split('|||');
          if (tags.indexOf(tag) !== -1) {
            card.style.display = '';
            hasVisible = true;
          } else {
            card.style.display = 'none';
          }
        });
        // Hide entire series block if no posts match
        if (!hasVisible) block.style.display = 'none';
      });

      backBar.style.display = 'flex';
    });
  });
});
</script>
