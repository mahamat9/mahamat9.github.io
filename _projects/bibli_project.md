---
layout: page
title: Bibli — Electronic Library Application
description: University project — An object-oriented library management application with web scraping and automated report generation
#description: A Python application to create, populate and report on a digital library via web scraping — OOP university project
img: assets/img/bibli_app.png
importance: 10
category: academic
github: "https://github.com/mahamat9/Application-Bibli"
---

## Overview

**Duration:** November 2023 – January 2024  
**Co-author:** A. Gonin  
**Context:** Object-Oriented Programming university project

---

**Bibli** is an electronic library management application developed during a university project. It allows users to feed a digital library by scraping books from URLs, manage collections of EPUB and PDF files, and generate automated reports on the library's contents.

---

## Architecture

The application follows a clean **object-oriented architecture** built around three main pillars:

- **Book abstraction** — `BaseLivre` defines a common interface for all book types
- **Library abstraction** — `BaseBibli` handles collection management
- **Scraping layer** — `BibliScrap` populates the library from web sources

A diagram is shown above, and the visual architecture of the system is illustrated below:

<img src="assets/img/bibli_app.png"
     alt="Diagram of App"
     style="float: left; margin-right: 10px;" />

---

## Key Features

- **Multi-format support** — manages both EPUB and PDF books through dedicated subclasses
- **Web scraping** — automatically extracts book metadata from URLs using BeautifulSoup4
- **Report generation** — produces EPUB and PDF reports summarizing the library contents
- **Configuration management** — user-defined settings via `Bibli.conf` (library dir, reports dir, max books)
- **Command-line interface** — intuitive CLI for scraping and report generation
- **Extensible design** — abstract base classes make it easy to add new book formats or storage backends

---

## Usage

### Scrape books from a URL

```bash
./bibli.py <url> <depth>
```

**Example:**

```bash
./bibli.py https://example.com/books 2
```

This scrapes `example.com` up to depth 2, extracts book metadata, and adds new books to the library.

### Generate reports

```bash
./bibli.py rapports
```

Generates EPUB and PDF reports in the configured reports directory, summarizing the current library contents.

### Configuration

Edit `Bibli.conf` to customize:

```ini
[Bibliotheque]
repertoire = ./ma_bibliotheque
rapports_dir = ./rapports
max_livres = 1000
```

---

## Tech Stack

| Component    | Technology                   |
| ------------ | ---------------------------- |
| Language     | Python 3                     |
| Web Scraping | BeautifulSoup4               |
| EPUB Parsing | ebooklib                     |
| PDF Parsing  | PyPDF2                       |
| Reports      | EPUB generation + PDF export |
| Config       | Python config file (`.conf`) |

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> View on GitHub
  </a>
</div>
