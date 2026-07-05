---
layout: page
title: Projects
permalink: /projects/
description: A collection of my main academic and personal projects.
nav: true
nav_order: 3
display_categories: [academic, personal] # add professional cat. in future
horizontal: false
---

<!-- pages/projects.md -->
<div class="projects">
<div class="project-filters" aria-label="Scientific project filters">
  <span class="project-filters-label">Filter:</span>
  <button type="button" class="project-filter-btn is-active" data-filter="all">All</button>
  <button type="button" class="project-filter-btn" data-filter="math">Mathematics</button>
  <button type="button" class="project-filter-btn" data-filter="statistics">Statistics</button>
  <button type="button" class="project-filter-btn" data-filter="applied-ml-dl">Applied ML & DL</button>
  <button type="button" class="project-filter-btn" data-filter="dev-big-data">Dev / Big Data</button>
  <button type="button" class="project-filter-btn" data-filter="challenge">Challenge</button>
  <button type="button" class="project-filter-btn" data-filter="other">Other</button>
</div>
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <section class="project-category-section">
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container project-category-grid">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3 project-category-grid">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  </section>
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    const filterButtons = document.querySelectorAll('.project-filter-btn');
    const cards = document.querySelectorAll('.project-card-item');
    const categorySections = document.querySelectorAll('.project-category-section');

    const applyFilter = (filter) => {
      cards.forEach((card) => {
        const category = card.dataset.scientificCategory || 'other';
        card.style.display = filter === 'all' || category === filter ? '' : 'none';
      });

      categorySections.forEach((section) => {
        const visibleCards = Array.from(section.querySelectorAll('.project-card-item')).filter(
          (card) => card.style.display !== 'none'
        ).length;
        section.style.display = visibleCards > 0 ? '' : 'none';
      });
    };

    filterButtons.forEach((btn) => {
      btn.addEventListener('click', () => {
        filterButtons.forEach((item) => item.classList.remove('is-active'));
        btn.classList.add('is-active');
        applyFilter(btn.dataset.filter || 'all');
      });
    });

    applyFilter('all');
  });
</script>
