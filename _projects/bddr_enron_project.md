---
layout: page
title: Enron Email Investigation App
description: Web application for judicial data exploration — Django, PostgreSQL, Enron dataset
img: assets/img/bddr_illustration.png
importance: 9
category: academic
github: https://github.com/mahamat9/Base-de-donnees-et-Application-web-sur-donnees-Enron
---

## Overview

**Duration:** January – May 2024  
**Co-author:** M. Charbonneau  
**Stack:** Django · PostgreSQL · Python · Git

---

## Context

The **Enron scandal** (2001) led to the release of one of the largest real-world email datasets ever made public — over **500,000 emails** exchanged between ~150 employees of Enron Corporation, made available by the FERC during its investigation.

This project builds a **web application** allowing investigators — with no required technical background — to explore and query this dataset through structured forms and visualizations.

---

## Database Design

### Source data

| Source       | Format       | Content                 |
| ------------ | ------------ | ----------------------- |
| Email corpus | Maildir tree | ~500k raw `.txt` emails |
| Metadata     | XML file     | Employee attributes     |

### Relational schema

<div class="row justify-content-center">
  <div class="col-sm-10">
    {% include figure.liquid path="assets/img/bddr_illustration.png"
       title="Relational schema — Enron database"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Relational schema: Employee, Mailbox, Email, Recipient.
</div>

### Data Pipeline

The database is populated entirely via Python scripts:

- **XML parsing** — `xml.etree.ElementTree` → employee table
- **Maildir walking** — `email` standard library → raw email parsing
- **Bulk insertion** — `psycopg2` with batched `executemany`

---

## Django Application

Selected queries are listed below.

| Query                 | Description                                             |
| --------------------- | ------------------------------------------------------- |
| Employee lookup       | Retrieve all attributes of an employee by name / email  |
| Timeline analysis     | All emails exchanged over a given time interval         |
| Contact network       | List all employees who communicated with a given person |
| Top senders           | Rank employees by volume of sent emails                 |
| Keyword search        | Full-text search across subjects and bodies             |
| Thread reconstruction | Retrieve a full email thread from a message ID          |

---

## Tools & Stack

- \*_Backend_: Django, Python 3.11
- **Database**: PostgreSQL, psycopg2
- **Data parsing**: Python _email_, _xml.etree.ElementTree_
- **Frontend**: Django templates, HTML, CSS
- **Version control**: Git / GitHub

---

<div style="display:flex; gap:1rem; justify-content:center; margin: 2.5rem 0;">
  <a href="https://github.com/mahamat9/Projet_BDDR_Enron"
     class="btn btn-primary" role="button" target="_blank">
    More details on GitHub
  </a>
</div>
```
