---
layout: about
title: About
permalink: /
#subtitle: <a href='#'>Affiliations</a>. Address. Contacts. Motto. Etc.
subtitle:
  Applied AI
  #"MSc Applied Mathematics — Data Science · Statistical learning, signal processing, deep learning, and interpretable methods for societal challenges"
  #Medical deep learning, image processing, generative AI

profile:
  align: right
  image: pic_prof.png
  image_circular: false # crops the image to make it circular
  #more_info: >
  #<p>Brest, France</p>
  #<p>Open to relocation</p>
  #<p>Seeking a <b>PhD or applied research position</b>, without location constraints.</p>
  #<p><a href="/files/CV.pdf">Full CV (PDF)</a></p>

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

<style>
  h2, h3, h4  { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }

  .post .profile {
    width: min(34%, 300px);
    margin: 0.4rem 0 1.2rem 2rem;
    text-align: center;
  }

  .post .profile img {
    width: 100%;
    max-width: 260px;
    border-radius: 22px;
    box-shadow: 0 14px 34px rgba(0, 0, 0, 0.16);
    border: 3px solid var(--global-bg-color);
  }

  .post .profile .more-info {
    font-family: inherit;
    font-size: 0.9rem;
    line-height: 1.4;
    opacity: 0.8;
    margin-top: 0.8rem;
  }

  .about-lead {
    font-size: 1.06rem;
    line-height: 1.65;
  }
  
  .cv-buttons {
    display: flex;
    justify-content: center;
    gap: 0.8rem;
    flex-wrap: wrap;
    margin: 1.6rem 0 2.2rem 0;
  }

  .cv-buttons .btn {
    font-size: 0.9rem;
    letter-spacing: 0.02em;
  }

  .section-rule {
    border: none;
    border-top: 1px solid var(--global-divider-color);
    margin: 2.4rem 0 1.8rem 0;
  }

  h4 {
    margin-top: 0;
    margin-bottom: 0.9rem;
    font-size: 1.05rem;
    letter-spacing: 0.03em;
    text-transform: uppercase;
    opacity: 0.7;
  }

  hr {
    margin: 2.2rem 0;
    opacity: 0.35;
  }

  @media (max-width: 768px) {
    .post .profile {
      float: none !important;
      width: 100%;
      margin: 0 auto 1.1rem auto;
    }

    .post .profile img {
      max-width: 190px;
    }
  }
</style>

<div class="about-lead" markdown="1">

I work on statistical learning, with a preference for methods whose behaviour can be explained rather than only measured. My background is in applied mathematics (MSc, University of Angers), preceded by a BSc in Mathematics and one year of health science studies (PluriPASS), which is where the interest in biomedical data started.

</div>

<div class="cv-buttons">
  <a href="/assets/pdf/CV_Mahamat_FR.pdf" class="btn btn-primary" role="button" target="_blank">
    <i class="fas fa-file-pdf"></i>&nbsp; CV (Français)
  </a>
  <a href="/assets/pdf/CV_Mahamat_EN.pdf" class="btn btn-outline-primary" role="button" target="_blank">
    <i class="fas fa-file-pdf"></i>&nbsp; CV (English)
  </a>
</div>

<hr class="section-rule">

#### Recent work

At **IMT Atlantique**, I worked on unsupervised detection of bioacoustic events and source separation. The methodological direction, algebraic and probabilistic methods (NMF, change-point detection) rather than deep architectures, was set with my supervisors; I implemented and benchmarked it against deep learning baselines. This choice was driven by computational efficiency and inherent interpretability, at a time when research was shifting massively toward deep learning. This work was presented at the **SERENADE 2026** workshop, and a journal paper is currently being drafted.

Before that, I worked on my [master's thesis](/projects/diffusion/) as part of a team. I was focusing on the theoretical foundations of diffusion models (the stochastic differential equations (SDE) formulation, time reversal and the connection to score matching) and conducting numerical experiments to validate the equivalence between the reverse-time process and the training of a generative diffusion model.

<hr class="section-rule">

#### What I want to work on next

Having spent time on the interpretable end of the spectrum, I want to apply the same standard of evidence to deep learning on image data, where my earlier work in computer vision for biology first drew me in.

The interest is not in applying deep models as black boxes, but in the question of when and why they work and at what cost.

**Medical and biological imaging** is the natural setting: it is where I started ([blood cell segmentation, apple detection](/projects/deep-image-processing/)), and where reliability is a constraint rather than a nice-to-have.

<hr class="section-rule">

#### Availability

I am looking for a **PhD position or an applied research role**, with no geographical constraint.

What I bring concretely: a mathematics background solid enough to read a method's derivation rather than only its API; experience running a full research cycle (literature, implementation, benchmarking against published baselines, write-up, conference submission); and a habit of reporting what did not work alongside what did.

Feel free to [get in touch](mailto:mmahamatnour99@gmail.com), I am happy to discuss open positions or collaborations.