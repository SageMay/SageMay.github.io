---
layout: post
title: "Using GitHub Projects for SOPs and Project Management"
author: "Sage Malingen"
categories: blog
tags: [blog]
image: flowers/sagebrushViolets.png
---
<!-- Siloed information and out-of-date SOPs stall productivity. As a computational biologist, I value how SOPs streamline wet-lab work and make processes more reproducible, but I also prize adaptability and version control.

Using a GitHub project template, I created an SOP for running a molecular dynamics simulation given my group's computational infrastructure. The project template can be copied into the repository for a new research project at the outset and serves as a unified location to document key decisions and verify that critical steps are accomplished correctly. I also used this project for training, finding that it simplified onboarding for new learners.

For more context on how I thought about creating an effective project template, see my blog post: https://sagemay.github.io/SOPtoGitHubProject or the GitHub project template: https://github.com/SageMay?tab=projects.  
-->
The first time I learned to run a molecular dynamics simulation, my mentor sat down and walked me through the process end-to-end. At the time, I had only a rudimentary knowledge of using the command line and computational chemistry. That meant that I had to integrate a huge amount of context with a detailed sequence of commands. My mentor had also built custom helper scripts and templates that made starting a new project faster if you knew how to use them and where to look.

I took detailed notes as he walked me through the process, and those notes became the basis for a more curated set of notes that I kept handy as I learned. The MD software I use, Amber, has a good set of tutorials for learners. And Amber's manual details all the capabilities of the software. But there was a gap in resources: I needed a stripped-down "How to run an MD simulation" guide to reference.

There are big drawbacks to not having a "How to guide" or SOP for a technical process:
It takes a huge amount of time, patience, and generosity from my mentor to walk through the process end-to-end. And those kinds of mentors are few and far between (thank you, Matt!).
The person writing the notes doesn't have enough understanding to contextualize different parts of the process, leading to avoidable errors.
An SOP can serve as a checklist, helping ensure all aspects are accomplished as intended.
SOPs that are followed make science more reproducible and methods more easily shared.

With new graduate students to onboard, we needed documentation to facilitate training and to reduce errors in accomplishing routine tasks.

The solution I pursued was using GitHub projects to create a "How to run an MD simulation guide" for the Center for Translational Muscle Research's MD workflow and computational resources. Inherently, this workflow is customized to that group. It is essential to have accessible SOPs as the CTMR Computational group adds new MD users.

In addition to serving as a guide, I designed the project as a template so that it can be copied for new repositories. Then, the project can be used for project management to help users ensure every step is properly completed. The repository will then contain the run scripts, along with the completed project steps and notes that the user is prompted to make as they accomplish their setup. In this way, a GitHub project can function for the computational scientist as a lab notebook.

Here you can see how the project is broken into nine ordered tasks, each of which can be expanded to read the actions necessary to correctly accomplish and document the task.

<img src="assets/img/GitHubProject/tasks.png" width="524" height="350" alt="Screen shot of a GitHub project template showing 9 tasks in the Todo column with headers indicating nature of the task." >

In creating this project template, I was inspired by "Diataxis": <a href="url">https://diataxis.fr/</a>. I identified why I had struggled with the existing documentation since Amber's manual is not structured to achieve a goal, and the tutorials often have information spread between multiple locations, along with background information. The existing sources did not provide a curated "How-to guide", which is critical for repeatedly executing complicated procedures correctly. So that's what I aimed to create. My guide has pretty minimal explanation, but I aimed to provide explicit instructions so that even a novice on the command line could accomplish each step. But the information is curated such that experienced users can use the project management feature as a checklist and a unified location for note-taking.

Here you can read the specific actions needed to accomplish the second task, including how to access and use a custom script to calculate ion quantity and an example of how the user should document the choices they make when solvating their system.

<img src="assets/img/GitHubProject/solvate.png" width="524" height="350" alt="Screen shot of a GitHub project template showing the actions needed to accomplish task 2, solvating the system." >

This particular project is customized to the computers, templates, and helper scripts used by the CTMR, so it isn't immediately transferable. However, you are welcome to use it as a launch point for building guides for your workflows. I hope this helped inspire you to make your processes more reproducible and trainable.
