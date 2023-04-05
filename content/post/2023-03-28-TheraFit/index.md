---
author: Tristan Madden
categories: [JavaScript]
date: 2023-03-23
draft: true
summary: "An exploration of ways to match clients to therapists during an intake."
tags: [Google App Script]
thumbnail: "thumbnail.png"
title: "TheraFit"
toc: false
usePageBundles: true
---

{{< rawhtml >}}

<style>
.row {
  display: flex;
}
.column {
  flex: 33.33%;
  padding: 2em;
}
.centered {
  text-align: center;
}
</style>

<h2> Questionnaire Flow</h2>

<div class="row">
  <div class="column">
  <a href="https://docs.google.com/forms/d/e/1FAIpQLSfwhgFOYaW9p6bna0GfLjJyB5hT3oaPqq88mReXTcmcwZ_xRQ/viewform"><img src="client-questionnaire.jpeg"></a>
  </div>
  <div class="column">
    <a href="https://docs.google.com/forms/d/e/1FAIpQLSfdcfGF7PkEegHfd15YFOB-d-wc3NS0qSjtJ1qADvlDKns_wQ/viewform"><img src="therapist-questionnaire.jpeg"></a>
  </div>
</div>

<div class="row">
  <div class="column centered">
  ⬇️
  </div>
  <div class="column centered">
    ⬇️
  </div>
</div>

<div class="row">
  <div class="column">
  <a href="https://script.google.com/home/projects/1Zee293DSwmQTzhWdQb6cBUmsttWwCb5MwwQ0ZcPX6JzmLW4dE3nPJ1DQ/edit"><img src="client-script.jpeg"></a>
  </div>
  <div class="column">
    <a href="https://script.google.com/home/projects/1IdNZ7iL_2sL9VSDZSLYnzh_i6mjywzQjamofpUwi62NNmZHQazMkVDCs/edit"><img src="therapist-script.jpeg"></a>
  </div>
</div>

<div class="row">
  <div class="column centered">
  ⬇️
  </div>
  <div class="column centered">
    ⬇️
  </div>
</div>

<div class="row">
  <div class="column centered">
  <a href="https://docs.google.com/spreadsheets/d/1ACpGIUQ_EA42Ym_yDxNpb81DWHLXSTX1jHzq7cnNxdI/edit?resourcekey#gid=1443418222"><img src="questionnaire-responses.jpeg"></a>
  </div>
</div>

{{< /rawhtml >}}

<h2>Google Sheets API Stuff</h2>
<h3>Configure the OAuth consent screen and add test users</h3>
https://console.cloud.google.com/apis/credentials/consent

<h3>Create an OAuth client ID</h3>
https://console.cloud.google.com/apis/credentials

<h2>Creating the Anaconda Environment</h2>

```Python
conda create --name questionnaire python=3.8
conda activate questionnaire
pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib gradio pandas
```

<h2>Using the Program</h2>

```Shell
cd C:\Users\50567920\Documents\GitHub\Questionnaire
python3 run.py
```