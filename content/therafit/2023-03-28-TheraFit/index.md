---
author: Tristan Madden
categories: [Python]
date: 2023-03-23
lastmod: 2024-02-10
draft: false
featured: true
summary: "TheraFit is a personal project utilizing a Large Language Model to enhance the process of matching clients with therapists."
tags: [Python]
thumbnail: "thumbnail.png"
title: "TheraFit"
toc: true
usePageBundles: true
---

TheraFit is a personal project that leverages a Large Language Model to optimize the process of matching clients with therapists.

## Google Sheets API Integration

### Configure the OAuth Consent Screen and Add Test Users

[Configure OAuth Consent Screen](https://console.cloud.google.com/apis/credentials/consent)

### Create an OAuth Client ID

[Create OAuth Client ID](https://console.cloud.google.com/apis/credentials)

## Dummy Data

[Clinician Google Form](https://docs.google.com/forms/d/1Yme6uV67ytb9-2j_-qnwDC57ODVpimZjKXp-qvYOnOk/edit)

[Client Google Form](https://docs.google.com/forms/d/1mYu0uyOR8SXyCWs7v4lFeNH8q5FQCjjoAGbvMEJ_arI/edit)

[Questionnaire Spreadsheet](https://docs.google.com/spreadsheets/d/1ACpGIUQ_EA42Ym_yDxNpb81DWHLXSTX1jHzq7cnNxdI/edit#gid=1038865900)

## Setup Environment

### [Git Repository](https://github.com/Trimad/TheraFit)

```shell
git clone https://github.com/Trimad/TheraFit.git
cd TheraFit
python -m venv venv
venv\Scripts\activate
python.exe -m pip install --upgrade pip
pip install -r requirements.txt

```

## LLM Model Backend

I've retired my custom LLM backend I'd made for <a href="https://huggingface.co/stabilityai/StableBeluga-7B">stabilityai/StableBeluga-7B</a>. I am now using <a href="https://lmstudio.ai/">LM Studio</a>, as it's become polished enough to be a "set it and forget it" solution for running an LLM backend.

This is my favored model for TheraFit right now: <a href="https://huggingface.co/teknium/OpenHermes-2.5-Mistral-7B">teknium/OpenHermes-2.5-Mistral-7B</a>

## Usage

To use the application, ensure LM Studio is running. Then, activate the virtual environment and execute the Python script as follows:

```shell
venv\Scripts\activate
python TheraFit.py
```
If everything is set up correctly, the app will be accessible at this URL:
<a href="http://127.0.0.1:7860">http://127.0.0.1:7860</a>
