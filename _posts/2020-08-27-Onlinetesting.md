---
published: true
title: Our experience with running online experiments 
categories: science
author: Dominik Krzeminski, Wojciech Zajkowski, Jiaxiang Zhang
description: "Our experience with running online experiments"
excerpt_separator: <!-- more -->
---

The COVID‐19 pandemic has impacted almost every aspect of our lives, including the way how we conduct experiments. This article shares our experience of running behavioural experiments online, with some hints on how to make this process smoother.

There are commercial products available for online testing (Testable, Inquisit, or Gorilla). However, if you opt for open-source solutions, there is no single tool to solve the whole problem. Thus, right after you come up with a design of an experiment you need to make a few choices:

## 1. Programming toolkit for your experiment
Online experiments typically run in a web-browser, and they need to be implemented in a programming language that such browser understands, like JavaScript (JS) with HTML and CSS components. Fortunately, there are several JS modules that provide ready to use components for online studies: 

[**_Lab.js_**](https://lab.js.org/) - probably the easiest to use for people not too experienced with web development. It has a nice web interface but still requires some programming. 

[**_PsychoJS_**](https://github.com/psychopy/psychojs/) – recommended for those already familiar with psychopy, as PsychoPy Builder can automatically convert an exising experiment into JS, albeit with limited functionalities. 

[**_jsPsych_**](https://www.jspsych.org/) – our current choice, as it is easier to use if you need to design an experiment from scratch. Its website provides some good tutorials with many predefined plugins to use, eg. random dot motion (RDK) task. Also, the community is quite active on the [discussion forum](https://github.com/jspsych/jsPsych/). 

## 2. Hosting platform for the experiment 
You can host the experiment wherever you want: on your university server, github pages, some other hosting provider, but our choice is [**_Pavlovia_**](https://pavlovia.org) as it provides nice ecosystem for the experiment code hosting and data saving that relies on git. After you share your experiment publicly, Pavlovia helps you saving the data on the server, such that you can download it with just one command line: `git pull`.

Here is tutorial on how to integrate jsPsych with pavlovia: [https://pavlovia.org/js-psych](https://pavlovia.org/js-psych)  

Pavlovia requires a paid license or buying storage credits. 

## 3. Recruitment platform

Finally, it comes the time to recruit your participants. For pilots, you may recruit from local participant panels in a normal way (e.g., SONA as used in many UK universities). One advantage of online experiments is that you can use existing services to reach a large participant portal. We evalauted two solutions: Amazon Mechanical Turk and Prolific). We eventually decided to use [Prolific](https://prolific.co) because of their easy-to-use [prescreening procedure](https://researcher-help.prolific.co/hc/en-gb/articles/360009221093-Can-I-prescreen-for-certain-demographics-or-target-certain-participants-Which-demographic-filters-do-you-offer-).

From Prolific you will get a unique code at the end of the experiment, which indicates that your participants successfully finished the task. It can look like this in jsPsych (at the end of your experiment): 

{% highlight javascript %}
var theend = { 
type: 'html-button-response', 
stimulus: "<p>This is the end of the experiment.<br/>" + 
"You can come back to Prolific now by clicking the link below:</p>", 
choices: ["<a href='https://app.prolific.co/submissions/complete?cc=<EXPERIMENT UNIQUE CODE>'>Press to finish</a>"]; 
}; 
timeline.push(theend); 
{% endhighlight %}

With all this you are basically ready to go! But there are some important details you need to be careful with. 

<!-- more -->

### (a) Local testing versus deployment 
Usually working on the code requires running it multiple times manually and then deploying to Pavlovia. As pavlovia has specific commands that must be added to the code (https://pavlovia.org/js-psych) that may not run locally, we suggest adding a logical variable `pav` that controls adding pavlovia specific commands, eg. 

{% highlight javascript %}
if (pav) { 
	var pavlovia_init = { 
	type: "pavlovia", 
	command: "init" 
}; 
timeline.push(pavlovia_init); 
{% endhighlight %}

You can switch it off when testing locally `(var pav = false)`, or turn on `(pav = true)` when pushing to the Pavlovia server. 


### (b) the issue of missing data files 
In our experience, there can be a considerable data loss (up to 10%). In particular, data from some participants doesn’t get saved. Currently, the cause of these issues remains a mystery to us. Thus, we recommend adding intermediate checkpoints in your experiment code, which will save extra data files on the server: 

{% highlight javascript %}
var pavlovia_finish = { 
type: "pavlovia", 
command: "finish", 
participantId: participant_id + '_checkpoint2' 
}; 
timeline.push(pavlovia_finish); 
timeline.push(wait1sec); 
{% endhighlight %}


### (c) Participant identification 
To identify your participants based on their ProlificID, you can simply modify the link to your experiment like this: 


{% highlight javascript %}
https://run.pavlovia.org/dokato/2drdkx/?PARTICIPANT_ID={{"{{%"}}PROLIFIC_PID%}}
{% endhighlight %}

And add to pavlovia code: 
{% highlight javascript %}
var participant_id = jsPsych.data.getURLVariable('PARTICIPANT_ID');
{% endhighlight %}

### (d) Recording additional data 
Sometimes you might be interested in recording for example the screen resolution and browser type of your participants. We can do it like this:

{% highlight javascript %} 
var browser = navigator.userAgent; 
var resolution = "W" + window.screen.availWidth + ",H" + window.screen.availHeight; 
jsPsych.data.addProperties({'browser' : browser, 'resolution' : resolution});
{% endhighlight %}

A couple of outstanding points that have proven to be crucial: 
* Make your task clear and instructions concise. Provide an interactive tutorial before starting the main part of the experiment with an automatic check to verify if participants really understood the task. Advanced elements enable looping through parts of the tutorial until participant reaches a desired performance level – indicator that they understood and engaged in task. 
* Introduce attention checks to your study. Participants join online experiments could be prone to distractions. 
* Run your experiment in small batches. Even the best designed experiment may contain bugs or cause some issues. It is easier to deal with these problems step by step. We suggest starting with small batches, recruiting up to 10 participants. After making sure the task works well and all the necessary data is collected, you can then recruit the rest of your participants in larger batches. 
* We encourage everyone to pre-register the experiment at OSF (https://osf.io). It’s quick and simple and vastly improves the robustness of your research. 

We hope that all the tips above will help you build better online experiments! 

Some extra reading: 

[https://link.springer.com/article/10.3758/s13428-020-01395-3](https://link.springer.com/article/10.3758/s13428-020-01395-3)  
[https://ocean.sagepub.com/blog/how-to-run-an-online-experiment](https://ocean.sagepub.com/blog/how-to-run-an-online-experiment)  
[https://www.jspsych.org](https://www.jspsych.org/)  
