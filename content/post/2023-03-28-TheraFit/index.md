---
author: Tristan Madden
categories: [nodejs]
date: 2023-03-23
lastmod: 2023-08-03
draft: true
summary: "An exploration of ways to match clients to therapists during an intake."
tags: [Google App Script]
thumbnail: "thumbnail.png"
title: "TheraFit"
toc: true
usePageBundles: true
---



## Llama 2 Implementation

### GitHub Repository

I'm using the oobabooga text-generation-webui for LoRA training and the API for inference.

https://github.com/oobabooga/text-generation-webui

#### webui.py flags
```python
CMD_FLAGS = '--api --load-in-4bit --model TheBloke_Llama-2-13B-Chat-fp16'
```
#### model weights
https://huggingface.co/TheBloke/Llama-2-13B-Chat-fp16
## Training Data Schema

```json
[

	{
		"role": "patient",
		"name": "Byrthdeigh Parteigh",
		"What brings you in for therapy?": "I have been feeling a constant level of anxiety for the past few months.",
		"Have you sought therapy before for similar concerns?": "No, this is my first time seeking therapy.",
		"Are you currently experiencing any major life changes or stressors?": "I recently started a new job, and I've been finding it difficult to adjust.",
		"How would you describe your sleep patterns recently?": "I have difficulty sleeping most nights. I find it hard to turn my thoughts off.",
		"What are some of your coping strategies when you feel anxious?": "I try to distract myself with work or exercise, but it's getting harder to manage.",
		"What are you hoping to achieve through therapy?": "I hope to learn better coping strategies and understand why I'm feeling this way.",
		"How is your relationship with your family?": "It's generally good, but they don't really understand what I'm going through.",
		"Have you experienced any traumatic events that impact your mental health?": "I had a car accident a few years back, and it still affects me sometimes.",
		"Do you have any thoughts of suicide or self-harm?": "Occasionally, I have such thoughts but never really planned on it.",
		"What's your social life like?": "I have a few close friends, but I've been withdrawing from them lately.",
		"Do you have any physical health issues that affect your mental health?": "I've been diagnosed with hypothyroidism, and I think it affects my mood.",
		"What activities used to bring you joy but now do not?": "I used to love painting, but lately, I can't find the energy or motivation to do it.",
		"How would you describe your overall mood on a typical day?": "Mostly anxious with a mix of sadness. Some days are better than others.",
		"Do you consume alcohol or any drugs? If so, how often?": "I occasionally drink, maybe a glass of wine a week.",
		"Are there any fears or worries that are currently bothering you the most?": "I'm worried I might lose my job if I can't get my anxiety under control.",
		"notes": "I would prefer a therapist who is experienced in cognitive behavioral therapy. Also, a non-judgmental approach is crucial for me."
	},

	{
		"role": "therapist",
		"name": "Michelle Madden",
		"credentials": "Certified in Cognitive Behavioral Therapy",
		"What is your therapeutic approach?": "I primarily use a cognitive behavioral approach, but I'm also trained in solution-focused brief therapy and mindfulness techniques.",
		"What types of disorders do you specialize in treating?": "I specialize in treating anxiety disorders, depressive disorders, and adjustment disorders.",
		"What is your approach towards homework assignments for patients?": "I believe homework assignments can be very beneficial for reinforcing therapeutic concepts. However, I understand each patient is different, so I try to adjust the workload according to their comfort and pace.",
		"How do you handle crises or emergencies?": "I encourage patients to call emergency services or hotlines during a crisis. While I'm not available 24/7, I check my messages regularly and try to respond as soon as I can.",
		"Do you incorporate mindfulness practices in your therapy?": "Yes, I believe mindfulness practices can be beneficial for many patients and often incorporate them into therapy when appropriate.",
		"How do you approach a case of high anxiety?": "I use a combination of cognitive restructuring, relaxation techniques, and exposure therapy while treating patients with high anxiety.",
		"What is your policy on communication outside of sessions?": "I can be reached through email for non-emergencies. However, I encourage discussing significant issues during our sessions.",
		"How do you handle patients with suicidal thoughts?": "I take any expression of suicidal thoughts very seriously. I work with the patient to develop a safety plan and connect them with necessary crisis resources.",
		"What is your approach towards medication?": "While I'm not able to prescribe medication, I understand its role in mental health treatment. I collaborate with other healthcare providers when medication may be beneficial.",
		"How do you ensure a non-judgmental environment in therapy?": "I consistently express empathy, validate patient feelings, and respect patient choices to ensure a non-judgmental environment.",
		"What are your views on self-care for patients?": "I believe self-care is crucial in maintaining mental health. I often work with patients to develop effective self-care routines.",
		"How do you measure progress in therapy?": "Progress is measured through ongoing dialogue about the patient's feelings and changes in their thoughts and behaviors, as well as periodic formal evaluations.",
		"What is your approach to confidentiality in therapy?": "I adhere strictly to professional ethical guidelines and laws to protect patient confidentiality, except in cases of imminent danger to self or others, or legal requirements.",
		"What kind of follow-up do you do after therapy concludes?": "I offer follow-up sessions to review progress and address any new concerns. The frequency is tailored to each patient's needs.",
		"How do you handle difficult or resistant patients?": "I try to understand the root of their resistance and work collaboratively with the patient to address these issues, providing empathy and patience throughout the process.",
		"notes": "I prefer not to work with patients diagnosed with severe personality disorders as it is outside my area of expertise."
	}

]
```

## GSuite Implementation
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

<h3> Questionnaire Flow</h3>

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