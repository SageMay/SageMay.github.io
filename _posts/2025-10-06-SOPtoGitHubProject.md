---
layout: post
title: "Using GitHub Projects for SOPs and Project Management"
author: "Sage Malingen"
categories: blog
tags: [blog]
image: flowers/sagebrushViolets.jpg
---
<!-- Teaching someone to run a molecular dynamics simulation
-->

When I learned to run a molecular dynamics simulation for the first time, my mentor sat down and walked me through the process end-to-end. At the time, I had only a rudimentary knowledge of using the command line and computational chemistry. That meant that I had to integrate a huge amount of context with a detailed sequence of commands. My mentor had also built custom helper scripts and templates that made starting a new project faster if you knew how to use them and where to look.

I took detailed notes as he walked me through the process and those notes became the basis for a more curated set of notes that I kept handy as I learned. The MD software, Amber, has a good set of tutorials for learners to use. And the manual is also excellent and details all capabilities of the software. But there was a gap -- a stripped down "How to" guide.

There were big draw backs to not having a "How to" guide or SOP in the group. First, it took a huge amount of time, patience  and generosity from my mentor -- those kinds of mentors are few and far between (thank you Matt!). Second, the person writing the notes (me) didn't have enough understanding to contextualize different parts of the process. This lead to me making errors in my early simulations that were easily avoidable with hindsight.

With new graduate students to onboard, we needed documentation to facilitate training and to reduce errors in accomplishing routine tasks.

The solution I pursued was using GitHub projects to create a "How to" guide for the Center for Translational Muscle Research's MD workflow and computational resources. Inherently, this workflow is customized to that group, and it is essential as the core adds new MD users to have accessible SOPs.

In addition to serving as a guide, I designed the project as a template so that it can be copied for new repositories where the project can be used for project management to help users ensure every step is properly completed. The repository will then contain the run scripts, along with the completed project steps and notes that the user is prompted to make as they accomplish their setup. In this way, a GitHub project can function for the computational scientist as a lab notebook.

Here you can see how the project is broken into 9 ordered tasks, each of which can be expanded to see the actions necessary to properly accomplish and document the task.

<img src="assets/img/GitHubProject/tasks.png" width="524" height="350" alt="Screen shote of a GitHub project template showing 9 tasks in the Todo column with headers indicating nature of the task." >

In creating this project template I was inspired by "Diataxis": <a href="url">https://diataxis.fr/</a>. I identified why I had struggled with the existing documentations: Amber's manual was not directed enough for addressing a goal and the tutorials often have information spread between multiple locations. The existing sources did not provide a curated "How to" guide, which is critical for repeatedly executing complicated procedures correctly. So that's what I aimed to make. My guide has pretty minimal explanation, but I aimed to provide explicit instructions so that even a novice on the command line could accomplish each step. And also so that experienced users could use the project management feature as a check list and a unified location for note-taking.

Here you can read the specific actions needed to accomplish the second task, including how to access and use a custom script to calculate ion quantity and an example of how the user should document the choices they make when solvating their system.

<img src="assets/img/GitHubProject/solvate.png" width="524" height="350" alt="Screen shote of a GitHub project template showing the actions needed to accomplish task 2, solvating the system." >


Will this particular project work for your specific MD workflow? Probably not. But I hope this will inspire others to document their work flows to make them more reproducible and trainable.
