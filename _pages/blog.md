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

  <!-- Header + filters -->
  <div class="blog-header">
    <p class="blog-header-desc">{{ site.blog_description }}</p>
  </div>

  <div class="blog-filter-bar">
    <button class="filter-chip active" data-filter="all">All</button>
    {% for category_name in site.display_categories %}
      {% assign cat_posts = site.posts | where_exp: "post", "post.categories contains category_name" %}
      {% if cat_posts.size > 0 %}
        <button class="filter-chip" data-filter="category" data-value="{{ category_name }}">{{ category_name }}</button>
      {% endif %}
    {% endfor %}
    <span class="filter-divider"></span>
    {% for tag in site.display_tags %}
      {% assign tag_posts = site.tags[tag] %}
      {% if tag_posts and tag_posts.size > 0 %}
        <button class="filter-chip filter-chip-tag" data-filter="tag" data-value="{{ tag }}">{{ tag }}</button>
      {% endif %}
    {% endfor %}
  </div>

  <!-- Active filter label -->
  <div class="blog-active-filter" style="display:none;">
    <span class="active-filter-label"></span>
    <button class="active-filter-clear">&times; Clear</button>
  </div>

  <!-- All posts as a single filterable list -->
  <div class="blog-posts">
    {% assign all_posts = site.posts | sort: "date" | reverse %}
    {% for post in all_posts %}
      {% if post.external_source == blank %}
        {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
      {% else %}
        {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
      {% endif %}

      <a href="{{ post.url | relative_url }}" class="blog-post-card"
         data-categories="{{ post.categories | join: '|||' }}"
         data-tags="{{ post.tags | join: '|||' }}">
        <div class="blog-post-card-inner">
          <div class="blog-post-meta">
            <time>{{ post.date | date: '%b %d, %Y' }}</time>
            <span class="meta-sep">&middot;</span>
            <span>{{ read_time }} min read</span>
            {% for category in post.categories %}
              <span class="meta-sep">&middot;</span>
              <span class="meta-category">{{ category }}</span>
            {% endfor %}
          </div>
          <h3 class="blog-post-title">{{ post.title }}</h3>
          <p class="blog-post-desc">{{ post.description }}</p>
          {% if post.tags.size > 0 %}
          <div class="blog-post-tags">
            {% for tag in post.tags %}
              <span class="inline-tag">#{{ tag }}</span>
            {% endfor %}
          </div>
          {% endif %}
        </div>
      </a>
    {% endfor %}
  </div>

  <!-- Empty state -->
  <div class="blog-empty-state" style="display:none;">
    No posts found for this filter.
  </div>

</div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const chips = document.querySelectorAll('.filter-chip');
  const cards = document.querySelectorAll('.blog-post-card');
  const activeFilterEl = document.querySelector('.blog-active-filter');
  const activeFilterLabel = document.querySelector('.active-filter-label');
  const clearBtn = document.querySelector('.active-filter-clear');
  const emptyState = document.querySelector('.blog-empty-state');

  function applyFilter(type, value) {
    let visibleCount = 0;
    cards.forEach(function(card) {
      let show = false;
      if (type === 'all') {
        show = true;
      } else if (type === 'category') {
        var cats = card.getAttribute('data-categories').split('|||');
        show = cats.indexOf(value) !== -1;
      } else if (type === 'tag') {
        var tags = card.getAttribute('data-tags').split('|||');
        show = tags.indexOf(value) !== -1;
      }
      card.style.display = show ? '' : 'none';
      if (show) visibleCount++;
    });

    // Update active states
    chips.forEach(function(c) { c.classList.remove('active'); });
    var activeChip = document.querySelector('.filter-chip[data-filter="' + type + '"][data-value="' + value + '"]');
    if (type === 'all') activeChip = document.querySelector('.filter-chip[data-filter="all"]');
    if (activeChip) activeChip.classList.add('active');

    // Show/hide filter label
    if (type === 'all') {
      activeFilterEl.style.display = 'none';
    } else {
      activeFilterLabel.textContent = (type === 'category' ? '' : '#') + value;
      activeFilterEl.style.display = 'flex';
    }

    // Empty state
    emptyState.style.display = visibleCount === 0 ? 'block' : 'none';
  }

  chips.forEach(function(chip) {
    chip.addEventListener('click', function() {
      applyFilter(this.getAttribute('data-filter'), this.getAttribute('data-value'));
    });
  });

  clearBtn.addEventListener('click', function() {
    applyFilter('all', '');
  });
});
</script>
