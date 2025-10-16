---
layout: post
title: "Visualizing Protein Structures Using Chimera"
author: "Sage Malingen"
categories: blog
tags: [blog]
image: columbine.jpg
---
<!-- Are you new to visualizing molecular dynamics (MD) simulations? Creating compelling visuals changes the game for how your work is perceived, but in my experience there's a steep learning curve. Check out my blog post for a few tips that helped make me make simulations clearer and quicker.
-->

TLDR:
1. Write down the key residue numbers right away -- it will save time later.
2. Before creating a single image, create a color palette and prescribe it in a COM file opened every time you view the simulation. (Remember, don't change the color of prescribed ions).
3. Show depth using the Tools > Viewing controls > Effects settings.
4. Use the command line; document syntax lines you go back to. Try using an LLM if you need help.
5. How to create a quick video.

An attractive aspect of molecular dynamics (MD) simulations is their visual nature. Unlike so much of molecular biology where visualization of Angstrom-nanometer scale cellular machinery is based primarily on careful experimentation and crystallographic structures, MD simulations enable visual observations.

This lays bare one of the limitations of molecular dynamics: as with any computational model, it is primarily a tool for hypothesis generation, albeit a pretty good one. Nonetheless, MD allows us to "observe" the behavior of molecules (proteins in particular) in a similar way that microscopes enable empirical observations. And, like much of microscopy, the conditions of a MD simulation are far removed from the conditions of life.

All of this to say, MD is prized for its visuals. And if you get to present these results, the visualization itself can shape how clearly scientific points are communicated and the degree of attention that other humans pay to them. Unfortunately, learning software to visualize these simulations can pose a big hurdle and is time-consuming in its own right.

When it comes to communicating science, I'm a firm believer in following some core principles and iterating quickly in response to feedback, but I usually don't want to spend a whole day fiddling with a figure if the result is already clear. My hope is that my notes on using UCSF Chimera will help others to visualize their results more clearly, quickly and consistently at the get go.


<h2> 1. Setting up for success </h2>
When you get that first simulation, go ahead and write down the residue numbers of key portions of the structure that you can anticipate visualizing. In my example, I'm working with a structure based on a portion of the actin-containing thin filament of muscle. In particular, the troponin complex from PDB 1J1E, which is a crystallographic structure where troponin was in the activated, calcium bound state. To this structure, I added an intrinsically disordered portion of troponin called NTnI. This region is not resolved in crystallographic structures, but it's critical for lusitropy in cardiac muscle.

<h2> 2. Making consistent images using a .com file </h2>
Make a ".com" file to be opened whenever you open a simulation of that structure that will color different portions of the structure. Setting this up early prevents having figures from when you first started the project that don't match the figures you made at the end. Here's an example of the `troponin.com` file I wrote:

`color #70a1ca :.A
color #c3ddf6 :.B
color #3b7e65 :.C`

The `:.A` syntax denotes a chain, but this could just as well be a numerical selection. For instance, the residues that are critical for coordinating the regulatory calcium could be selected as `:65,67,69,71,73,76`.

Some colors are used canonically for certain atom types; don't change the colors of het atoms or default colors of ions. Also, avoid colors that match those since you might want to highlight them later. For example, in the figure of troponin, I have maintained the default lime green for the calcium ions and I have avoided using blue and red colors of the same hue that are used to denote  N and O atoms.

<h2> 3. Depth cues to emphasize 3D structure </h2>
Highlighting 3D structure in a 2D figure is key to representing the results of MD simulations. Here you can see the settings I used for depth cueing heavily emphasize the N lobe of TnC while allowing the IT lever arm to fade into the background as color saturation is lost. The "start" and "end" values balance one another such that if start = .4, essentially the proximal 40% of the structure will be saturated and if end = .5 the distal 50% will be unsaturated with a gradient placed over the portion of the structure in the middle (40-50%).

Also, using a large value for "silhouette" emphasizes the black outline around the structure when saving an image. And keeping shadows on and adjusting the position of the light source using the "lighting" tab can help to further emphasize the structure.

The downside of these settings, in particular shadows and silhouetting, is that it takes longer for your local machine to render these features. So if you're demonstrating live using Chimera and the computer is bogging down you can uncheck shadows and silhouettes to reduce the load to visualize the trajectory.

<img src="assets/img/chimeraImgs/depthCue.png" width="450" height="450" alt="Figure showing a screenshot of a Chimera session where Troponin is visualized (PDB: 1J1E with the addition of TnI's N terminal extension) and the settings for Viewing controls > Effects are shown." >

<h2> 4. Using the command line </h2>
Use the command line to your advantage. Learning the syntax can help move faster using Chimera and can build to more complicated knowledge, like performing commands that re-perform a calculation for every frame of the simulation (the Per-Frame option in the MD movie viewer pane, which will be explained in 5). Here's an example that looks for pseudobonds among residues along the coordinating loop (residues 65-76). The coordinating residues are important because they functionally couple muscle activation with muscle contraction by tightly binding to calcium ions released by the sarcoplasmic reticulum, and that tight binding is in part facilitated by a network of bonds among the coordinating loop residues. When calcium binds, a hydrophobic region on the opposite side of TnC is disposed to open up and can result in the a portion of TnI binding that allows the rest of the thin filament to bind with myosin.

`findclash ':65-76!H' test self overlapCutoff -.4 hbondAllowance 0 intraMol true`

This line can be run in Chimera's command line (if yours pane doesn't show it, use Favorites>Command Line to open it).
Here, I have prescribed settings for `findclash` to look for contacts by adjusting the values of `overlapCutoff` and `hbondAllowance`, but you can also set these parameters to better identify clashes. The manual page for findclash explains this well: https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/midas/findclash.html

<img src="assets/img/chimeraImgs/commandLine.png" width="450" height="450" alt="Figure showing a screenshot of a Chimera session where Troponin is visualized (PDB: 1J1E with the addition of TnI's N terminal extension) and the command line is used to highlight pseudobonds between residues along the regulatory calcium coordinating loop." >

<h2> 5. Writing a per-frame script </h2>
Building on knowledge of how to use the command line, multiple commands can be performed repeatedly over the course of a trajectory. To do this, from the MD movie panel open Per frame>Define script and enter the code into the window. This is an example of a per frame script I wrote to look at potential contacts between a subset of residues:

`findclash ':65,67,69,71,73,76!H :237-278!H' test self overlapCutoff -.4 hbondAllowance 0 intraMol false reveal true;
ksdssp;`

The first line looks for interactions by residues 65,67,69,71,73,76 and 237-278. Those are the residues of the calcium coordinating loop and of the N terminal extension of TnI, respectively. The N terminal extension of TnI is a functionally important portion of troponin, but because it is disordered we don't know much about what it does, either in the context of the whole sarcomere, or even in the troponin structure that is used for many wet lab experiments. So we can use this command to figure out if it's likely to be interacting with the coordinating loop in this simulation.

Again, I prescribed settings for the `overlapCutoff` and `hbondAllowance` parameters to look for contacts: https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/midas/findclash.html. I also set `intraMol` to false, since by default the `findclash` behavior would depict interactions between covalently bonded pairs in the structure, but in this case, I want to see what's going on between two disconnected regions. Finally, by setting `reveal` to true residues with atoms participating in a pseudobond will be automatically shown, along with the pseudobond itself, even though initially I am only showing the ribbon structure of NTnI.

The second line `ksdssp` re-evaluates the protein's secondary structure for every frame, which is particularly helpful since there's a disordered region in this simulation.

There is a lot more out there; for instance see the UCSF's Chimera documentation: https://www.rbvi.ucsf.edu/chimerax/docs/user/commands/perframe.html

Finally, a per frame script is great when you're analyzing your results, but it's also important to be able to record a movie for a power point presentation.

<h2> 6. Record a movie </h2>
Recording movies can be complicated and gorgeous if you want to put in the time. But if you're just getting started, try going to the MD movie pane and selecting File>Record movie. You can adjust your step size and frame rate accordingly, and also the beginning and end of the simulation. As long as you have set up the viewing window on the portion of the structure you wish to highlight and used Actions>Hold selection steady for the key region, this can quickly produce a decent movie. In this example, you can observe several positively charged Lysines and Arginines on NTnI that seem disposed to interact with the negatively charged calcium coordinating site II.

<video width="600" height="600" controls loop="" muted="" autoplay="">  
  <source type="video/mp4" src="https://github.com/SageMay/SageMay.github.io/blob/main/assets/img/chimeraImgs/test.mp4">
</video>
