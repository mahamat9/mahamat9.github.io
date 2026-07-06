---
layout: page
title: Bibli — Electronic Library Application
description: Software engineering project implementing an object-oriented digital library system with scraping and automated reporting workflows.
#description: A Python application to create, populate and report on a digital library via web scraping — OOP university project
img: assets/img/bibli_app.png
importance: 11
category: academic
scientific_category: dev-big-data
github: "https://github.com/mahamat9/Application-Bibli"
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Duration:** November 2023 – January 2024  
**Authors:** <u>M. Mahamat</u>, A. Gonin

---

## Context

**Bibli** is an electronic library management application developed during a university project. It allows users to feed a digital library by scraping books from URLs, manage collections of EPUB and PDF files, and generate automated reports on the library's contents.

---

### Architecture

The application follows a clean **object-oriented architecture** built around three main pillars:

- **Book abstraction** : `BaseLivre` defines a common interface for all book types
- **Library abstraction** : `BaseBibli` handles collection management
- **Scraping layer** : `BibliScrap` populates the library from web sources

A diagram is shown above, and the visual architecture of the system is illustrated below:

<div class="row justify-content-center">
  <div class="col-sm-8">
    {% include figure.liquid path="assets/img/bibli_app.png"
       title="Diagram of App"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>

---

- **Multi-format support** : manages both EPUB and PDF books through dedicated subclasses
- **Web scraping** : automatically extracts book metadata from URLs using BeautifulSoup4
- **Report generation** : produces EPUB and PDF reports summarizing the library contents
- **Configuration management** : user-defined settings via `Bibli.conf` (library dir, reports dir, max books)
- **Command-line interface** : intuitive CLI for scraping and report generation
- **Extensible design** : abstract base classes make it easy to add new book formats or storage backends

---

#### Tech Stack

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin-bottom:2rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Component</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Technology</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Language</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Python 3</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Web Scraping</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>BeautifulSoup4</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">EPUB Parsing</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>ebooklib</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">PDF Parsing</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>PyPDF2</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Reports</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">EPUB generation + PDF export</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Config</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Python config file (<code>.conf</code>)</td>
    </tr>
  </tbody>
</table>

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>
