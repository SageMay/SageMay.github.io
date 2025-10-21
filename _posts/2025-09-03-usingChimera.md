---
layout: post
title: "Visualizing Protein Structures Using Chimera"
author: "Sage Malingen"
categories: blog
tags: [blog]
image: flowers/columbine.jpg
---
<!-- Are you new to visualizing molecular dynamics (MD) simulations? Creating compelling visuals changes the game for how your work is perceived, but in my experience there's a steep learning curve. Check out my blog post for a few tips that helped make me use Chimera to make clearer visuals more quickly. https://sagemay.github.io/usingChimera

In the post I cover:

1. Writing a .com file to specify colors of key structural features consistently.
2. How to use depth cueing
3. Using the command line and writing per frame scripts
4. Making your first videos
-->

TLDR:
1. Write down the key residue numbers right away and use them to make a COM file to consistently color your structure. Remember, don't change the color of prescribed atoms.
2. Show depth using the Tools > Viewing controls > Effects settings.
4. Use the command line and document syntax lines you go back to.
5. How to create a quick video.

Molecular dynamics (MD) simulations are prized for their visuals. And if you get to present these results, the visualization itself can shape how clearly scientific points are communicated and the degree of attention that other humans pay to them. Unfortunately, learning software to visualize these simulations is time-consuming in its own right. When it comes to communicating science, I'm a firm believer in following some core principles and iterating quickly in response to feedback. But for a first draft, I don't want to spend a whole day fiddling with a figure if the result is already clear. My hope is that my notes on using UCSF Chimera (1) will help others to visualize their results more clearly, quickly and consistently at the get go for a good first draft.

<h2> 1. Setting up for success </h2>
When you get that first simulation, go ahead and write down the residue numbers of key portions of the structure that you can anticipate visualizing. In my example, I'm working with a structure based on a portion of the actin-containing thin filament of muscle. In particular, the troponin complex from PDB 1J1E (2), which is a crystallographic structure where troponin was in the activated, calcium bound state. To this structure, I added an intrinsically disordered portion of troponin called NTnI. This region is not resolved in crystallographic structures, but it's critical for lusitropy in cardiac muscle.

In this post, some key residues are residues that coordinate the regulatory calcium at site II (65,67,69,71,73 & 76), the residues that form the N terminal extension of TnI (237-278) and the three chains that compose the troponin complex (TnC, TnT and TnI).

<h2> 2. Making consistent images using a .com file </h2>
Make a ".com" file to be opened whenever you open a simulation of that structure that will color different portions of the structure. Setting this up early prevents having figures from when you first started the project that don't match the figures you made at the end. Here's an example of the `troponin.com` file I wrote:

`color #70a1ca :.A`<br>
`color #c3ddf6 :.B`<br>
`color #3b7e65 :.C`<br>

The `:.A` syntax denotes a chain, but this could just as well be a numerical selection. For instance, the residues that are critical for coordinating the regulatory calcium could be selected as `:65,67,69,71,73,76`. After loading the MD simulation, use File > Open and select the .com file to change colors from the default to those that you specified for your structure.

Some colors are used canonically for certain atom types; don't change the colors of het atoms or default colors of ions. Also, avoid colors that match those since you might want to highlight them later. For example, in the figure of troponin, I have maintained the default lime green for the calcium ions and I have avoided using blue and red colors of the same hue that are used to denote  N and O atoms.

<h2> 3. Use depth cues to emphasize 3D structure </h2>
Highlighting 3D structure in a 2D figure is key to representing the results of MD simulations. Here you can see the settings I used for depth cueing heavily emphasize the N lobe of TnC while allowing the IT lever arm to fade into the background as color saturation is lost. The "start" and "end" values balance one another such that if start = .4, essentially the proximal 40% of the structure will be saturated and if end = .5 the distal 50% will be unsaturated with a gradient placed over the portion of the structure in the middle (40-50%).

Also, using a large value for "silhouette" emphasizes the black outline around the structure when saving an image. And keeping shadows on and adjusting the position of the light source using the "lighting" tab can help to further emphasize the structure.

The downside of these settings, in particular shadows and silhouetting, is that it takes longer for your local machine to render these features. So if you're demonstrating live using Chimera and the computer is bogging down you can uncheck shadows and silhouettes to reduce the load to visualize the trajectory.

<img src="assets/img/chimeraImgs/depthCue.png" width="450" height="400" alt="Figure showing a screenshot of a Chimera session where Troponin is visualized (PDB: 1J1E with the addition of TnI's N terminal extension) and the settings for Viewing controls > Effects are shown." >

<h2> 4. Using the command line </h2>
Use the command line to your advantage. Learning the syntax can help move faster using Chimera and can build to more complicated knowledge, like performing commands that re-perform a calculation for every frame of the simulation (the Per-Frame option in the MD movie viewer pane, which will be explained in 5). Here's an example that looks for pseudobonds among residues along the coordinating loop (residues 65-76). The coordinating residues are important because they functionally couple muscle activation with muscle contraction by tightly binding to calcium ions released by the sarcoplasmic reticulum. Strong coordination is in part facilitated by a network of bonds among the coordinating loop residues. Effective coordination means that when calcium binds a hydrophobic region on the opposite side of TnC is disposed to open up and can result in the a portion of TnI binding that allows the rest of the thin filament to bind with myosin.

`findclash ':65-76!H' test self overlapCutoff -.4 hbondAllowance 0 intraMol true`

This line can be run in Chimera's command line (if your pane doesn't show the command line you can use Favorites > Command Line to open it).
Here, I have prescribed settings for `findclash` to look for contacts by adjusting the values of `overlapCutoff` and `hbondAllowance`, but you can also set these parameters to better identify clashes. The manual page for `findclash` explains this well: <a href="url">https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/midas/findclash.html</a>

<img src="assets/img/chimeraImgs/commandLine.png" width="450" height="400" alt="Figure showing a screenshot of a Chimera session where Troponin is visualized (PDB: 1J1E with the addition of TnI's N terminal extension) and the command line is used to highlight pseudobonds between residues along the regulatory calcium coordinating loop." >

<h2> 5. Writing a per-frame script </h2>
Building on knowledge of how to use the command line, multiple commands can be performed repeatedly over the course of a trajectory. To do this, from the MD movie panel open Per frame>Define script and enter the code into the window. This is an example of a per frame script I wrote to look at potential contacts between two subsets of residues:

`findclash ':65,67,69,71,73,76!H :237-278!H' test self overlapCutoff -.4 hbondAllowance 0 intraMol false reveal true;`<br>
`ksdssp;`

The first line looks for interactions by residues 65,67,69,71,73,76 and 237-278. Those are the residues of the calcium coordinating loop and of the N terminal extension of TnI, respectively. The N terminal extension of TnI is a functionally important portion of troponin, but because it is disordered it's challenging to know where it localizes, both in the context of the whole sarcomere, or even in the troponin structures that are used for many wet lab experiments. So we can use this command to figure out if it's likely to be interacting with the coordinating loop in this simulation.

Again, I prescribed settings for the `overlapCutoff` and `hbondAllowance` parameters to look for contacts: <a href="url">https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/midas/findclash.html</a>. I also set `intraMol` to false, since by default the `findclash` behavior would depict interactions between covalently bonded pairs in the structure, but in this case, I want to see what's going on between two disconnected regions. Finally, by setting `reveal` to true residues with atoms participating in a pseudobond will be automatically shown, along with the pseudobond itself, even though initially I am only showing the ribbon structure of NTnI.

The second line `ksdssp` re-evaluates the protein's secondary structure for every frame, which is particularly helpful since there's a disordered region in this simulation.

There is a lot more out there. For instance, you might want to perform a calculation every 10 frames rather than every 1 frame to lighten the computational load. For more, see UCSF's Chimera documentation: <a href="url">https://www.rbvi.ucsf.edu/chimerax/docs/user/commands/perframe.html</a>

Finally, a per frame script is great when you're analyzing your results, but it's also important to be able to record a movie.

<h2> 6. Record a movie </h2>
You've probably seen gorgeous MD movies that take a lot of skill to make, and watching them it can feel insurmountable to get started. If you're just beginning, try going to the MD movie pane and selecting File > Record movie. You can adjust your step size and frame rate accordingly, and also the beginning and end of the simulation. As long as you have set up the viewing window on the portion of the structure you wish to highlight and used Actions > Hold Selection Steady for the key region this can quickly produce a decent movie that communicates the key scientific idea. In this example, you can observe several positively charged Lysines and Arginines on NTnI that seem disposed to interact with the negatively charged calcium coordinating site II. I used the perframe script from 5, which illustrates pseudobonds with yellow.

<iframe width="560" height="315" src="https://www.youtube.com/embed/_0Wo_yHRS_s?si=u0v4znlpRxI0sPMc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Molecular graphics and analyses performed with UCSF Chimera, developed by the Resource for Biocomputing, Visualization, and Informatics at the University of California, San Francisco, with support from NIH P41-GM103311.

1. UCSF Chimera--a visualization system for exploratory research and analysis. Pettersen EF, Goddard TD, Huang CC, Couch GS, Greenblatt DM, Meng EC, Ferrin TE. J Comput Chem. 2004 Oct;25(13):1605-12.
2. Takeda, S., Yamashita, A., Maeda, K., & Maéda, Y. (2003). Structure of the core domain of human cardiac troponin in the Ca2+-saturated form. Nature, 424(6944), 35-41.
