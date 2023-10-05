---
date: "2022-10-24"
sections:
- block: hero
  content:
    cta_alt:
      label: Ask a question
      url: https://discord.gg/z8wNYzb
    image:
      filename: hero-academic.png
    title: My website
  design:
    background:
      gradient_end: '#1976d2'
      gradient_start: '#004ba0'
      text_color_light: true
- block: about.biography
  content:
    title: Biography
    username: admin
  id: about
- block: features
  content:
    items:
    - description: 'I have 5 years of experience using R'
      icon: r-project
      icon_pack: fab
      name: R
    - description: 'I have 1 year of experience using Overleaf'
      icon: overleaf
      icon_pack: ai
      name: Overleaf
    - description: 'I have 1 year of experience using Sql'
      icon: sql
      icon_pack: custom
      name: Sql
    - description: 'I have 1 year of experience using Python'
      icon: python
      icon_pack: custom
      name: Python
    title: Skills
  id: skills
- block: experience
  content:
    date_format: Jan 2006
    items:
    - company: PIMS/Awesense
      company_logo: org-gc
      company_url: https://www.pims.math.ca/
      link: uploads/pims-awesense.pdf
      date_end: "2022-07-01"
      date_start: "2022-07-27"
      description: |2-
          Responsibilities include:

          * Analysing
          * Modelling
          * Deploying
      description: |2-
          Responsibilities include:

          * Analysing
          * Modelling
          * Deploying
      location: Ottawa
      title: PIMS research project
    - company: University X
      company_logo: org-x
      company_url: ""
      date_end: "2020-12-31"
      date_start: "2016-01-01"
      description: Taught electronic engineering and researched semiconductor physics.
      location: California
      title: Professor of Semiconductor Physics
    title: Experience
  design:
    columns: "2"
  id: experience
- block: accomplishments
  content:
    date_format: Jan 2006
    items:
    - certificate_url: https://www.coursera.org
      date_end: ""
      date_start: "2021-01-25"
      description: ""
      organization: Coursera
      organization_url: https://www.coursera.org
      title: Neural Networks and Deep Learning
      url: ""
    - certificate_url: https://www.edx.org
      date_end: ""
      date_start: "2021-01-01"
      description: Formulated informed blockchain models, hypotheses, and use cases.
      organization: edX
      organization_url: https://www.edx.org
      title: Blockchain Fundamentals
      url: https://www.edx.org/professional-certificate/uc-berkeleyx-blockchain-fundamentals
    - certificate_url: https://www.datacamp.com
      date_end: "2020-12-21"
      date_start: "2020-07-01"
      description: ""
      organization: DataCamp
      organization_url: https://www.datacamp.com
      title: Object-Oriented Programming in R
      url: ""
    subtitle: null
    title: Accomplish&shy;ments
  design:
    columns: "2"
- block: collection
  content:
    count: 5
    filters:
      author: ""
      category: ""
      exclude_featured: false
      exclude_future: false
      exclude_past: false
      folders:
      - post
      publication_type: ""
      tag: ""
    offset: 0
    order: desc
    subtitle: ""
    text: ""
    title: Recent Posts
  design:
    columns: "2"
    view: compact
  id: posts
- block: portfolio
  content:
    buttons:
    - name: All
      tag: '*'
    - name: Deep Learning
      tag: Deep Learning
    - name: Other
      tag: Demo
    default_button_index: 0
    filters:
      folders:
      - project
    title: Blogs-Projects
  design:
    columns: "1"
    flip_alt_rows: false
    view: showcase
  id: blogs
- block: markdown
  content:
    subtitle: ""
    text: '{{< gallery album="demo" >}}'
    title: Gallery
  design:
    columns: "1"
- block: collection
  content:
    filters:
      featured_only: true
      folders:
      - publication
    title: Publications
  design:
    columns: "2"
    view: card
  id: publications
- block: collection
  content:
    filters:
      exclude_featured: true
      folders:
      - publication
    text: |-
      {{% callout note %}}
      Quickly discover relevant content by [filtering publications](./publication/).
      {{% /callout %}}
    title: Recent Publications
  design:
    columns: "2"
    view: citation
- block: collection
  content:
    filters:
      folders:
      - event
    title: Recent & Upcoming Talks
  design:
    columns: "2"
    view: compact
  id: talks
- block: tag_cloud
  content:
    title: Popular Topics
  design:
    columns: "2"
- block: contact
  content:
    autolink: true
    contact_links:
    email: yciss079@uottawa.ca
    form:
      formspree:
        id: null
      netlify:
        captcha: false
      provider: netlify
    subtitle: null
    text:
    title: Contact
  design:
    columns: "2"
  id: contact
title: null
type: landing
---
