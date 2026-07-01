---
layout: page
title: Enron Email Investigation App
description: Relational Database and Web application for judicial data exploration
img: assets/img/bddr_illustration.png
importance: 10
category: academic
github: https://github.com/mahamat9/Base-de-donnees-et-Application-web-sur-donnees-Enron
---

<style>
  h2 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Duration:** January – May 2024  
**Authors:** <u>M. Mahamat</u>, M. Charbonneau  

---

## Context

The **Enron scandal** (2001) triggered the release of one of the largest real-world email datasets ever made public over **500,000 messages** exchanged between ~150 employees, disclosed by the FERC during its federal investigation.

This project turns that raw corpus into a **queryable web application**: a structured PostgreSQL database, populated entirely by Python parsing scripts, exposed through a Django interface designed for investigators with no technical background.

Three questions drove the design:
- Who communicated with whom, and how often?
- What was said — and when?
- Can a non-technical user reconstruct a thread from a single message ID?

> The full schema, pipeline code, and query logic are available on GitHub.

---

## Database Design

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Source</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Format</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Content</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Email corpus</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Maildir tree</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">~500 k raw <code>.txt</code> emails</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Metadata</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">XML file</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Employee attributes</td>
    </tr>
  </tbody>
</table>

### Relational schema

<div class="row justify-content-center">
  <div class="col-sm-10">
    {% include figure.liquid path="assets/img/bddr_illustration.png"
       title="Relational schema — Enron database"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>

### Ingestion Pipeline

The database is populated entirely via Python scripts and no manual import:

- **XML parsing** : `xml.etree.ElementTree` → employee table
- **Maildir walking** : `email` stdlib → header & body extraction
- **Bulk insertion** : `psycopg2` with batched `executemany`

---

## Django Application

Six investigative queries are exposed through structured forms:

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Query</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Employee lookup</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">All attributes of an employee by name or address</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Timeline analysis</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">All emails exchanged over a given time interval</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Contact network</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Everyone who communicated with a given person</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Top senders</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Employees ranked by volume of sent emails</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Keyword search</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Full-text search across subjects and bodies</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Thread reconstruction</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Full email thread retrieved from a single message ID</td>
    </tr>
  </tbody>
</table>

---

## Stack

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Layer</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Technology</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Backend</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Django · Python 3.11</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Database</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">PostgreSQL · psycopg2</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Parsing</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>email</code> · <code>xml.etree.ElementTree</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Frontend</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Django templates · HTML · CSS</td>
    </tr>
  </tbody>
</table>

---

<div style="display:flex; gap:1rem; justify-content:center; margin: 2.5rem 0;">
  <a href="https://github.com/mahamat9/Projet_BDDR_Enron"
     class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>