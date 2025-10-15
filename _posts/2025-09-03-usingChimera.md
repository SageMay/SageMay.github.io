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
2. Before creating a single image, create a color palate and prescribe it in a COM file opened every time you view the simulation.
2b. Don't change the color of prescribed ions.
3. Show depth using the Tools > Viewing controls > Effects settings.
4. Use the command line; document syntax lines you go back to. Try using an LLM if you need help.
5. Change your defaults and learn to visualize depth.
6. How to create a quick video.

An attractive aspect of molecular dynamics (MD) simulations is their visual nature. Unlike so much of molecular biology where visualization of Angstrom-nanometer scale cellular machinery is imaginative and  based primarily on careful experimentation, MD simulations enables visual observation.

This lays bare one of the limitations of molecular dynamics: as with any computational model, it is primarily a tool for hypothesis generation, albeit a pretty good one. Nonetheless, MD allows us to "observe" the behavior of molecules (proteins in particular) in a similar way that microscopes enable empirical observations. And, like much of microscopy, the conditions of a MD simulation are far removed from the conditions of life.

All of this to say, MD is prized for its visuals. And if you get to present these results, the visualization itself can shape how clearly scientific points are communicated and the degree of attention that other humans pay to them. Unfortunately, learning software to visualize these simulations can pose a big hurdle and is time-consuming in its own right.

For me this has been a source of frustration. When it comes to communicating science, I'm a firm believer in following some core principles and iterating quickly in response to feedback, but I usually don't want to spend a whole day fiddling with a figure if the result is already clear. My hope is that my notes on using UCSF Chimera will help others to visualize their results more clearly, quickly and consistently at the get go.

1. When you get that first simulation, go ahead and write down the residue numbers of key portions of the structure that you can anticipate visualizing. In my example, I'm working with a structure based on portions of the actin containing thin filament of muscle.
2. Make a "com" file to be opened whenever you open a simulation of that structure.
2.a. Choose some colors that look nice. You can modify them later, but it's annoying, because you're probably going to be generating lots of images along the way, and it's nice to have everything match.
2.b Don't change the colors of het atoms or default colors of ions. Also avoid colors that match those since you might want to highlight them later. For example, in this figure I have maintained the default lime green for the calcium ions, and I have avoided using blue and red colors of the same hue that are used to denote  N and O atoms.

3. Highlighting 3D structure in a 2D figure is key to representing the results of MD simulations. Here you can see the settings I used for depth cueing heavily emphasize the N lobe of TnC while allowing the IT lever arm to fade into the background as color is lost. The "start" and "end" values balance one another such that if start = .4, essentially the proximal 40% of the structure will be saturated and if end = .5 the distal 50% will be unsaturated with a gradient in between 40-50%.

Also, using a large value for "silhouette" emphasizes the black outline around the structure when saving an image. And keeping shadows on and adjusting the position of lightsource using the "lighting" tab can help to further emphasize the structure.

The downside of these settings, in particular shadows and silhouetting, is that it takes longer for your local machine to render these features. So if you're demonstrating live using Chimera and the computer is bogging down you can uncheck shadows and silhouettes to reduce the load to visualize the trajectory.

<img src="assets/img/chimeraImgs/depthCue.jpg" width="450" height="450" alt="Figure showing a screenshot of a Chimera session where Troponin is visualized (PDB: 1J1E with the addition of TnI's N terminal extension) and the settings for Viewing controls > Effects are shown." >

4. Use the command line to your advantage. Learning the syntax can help move faster using Chimera and can build to more complicated knowledge, like performing commands that re-perform a calculation for every frame of the simulation (the Per-Frame option in the MD movie viewer pane). This is an example of a per frame script I wrote to look at potential contacts between a subset of residues:

`findclash ':65,67,69,71,73,76!H :237-278!H' test self overlapCutoff -.4 hbondAllowance 0 intraMol false reveal true;
ksdssp;`

The first line looks for interactions by residues 65,67,69,71,73,76 and 237-278. For those savvy with troponin, those are the residues of the calcium coordinating loop at site II for cardiac troponin and of the N terminal extension of TnI, respectively. The coordinating residues are important because they functionally couple muscle activation with muscle contraction. The N terminal extension of TnI is a functionally critical portion of troponin, but because it is disordered we don't know much about what it does, either in the context of the whole sarcomere, or even in the troponin structure that is used for many wetlab experiments. I have prescribed settings for the `overlapCutoff` and `hbondAllowance` parameters to look for contacts, but you can also set these parameters to better identify clashes. The manual page for findclash explains this well: https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/midas/findclash.html I also set `intraMol` to false, since by default the `findclash` behavior would depict interactions between covalently bonded pairs in the structure. Finally, by setting `reveal` to true residues with atoms participating in a pseudobond will be automatically shown, along with the pseudobond itself, even though initially I am only showing the ribbon structure of NTnI. 

The second line `ksdssp` re-evaluates the protein's secondary structure for every frame, which is particularly helpful since there's a disordered region in this simulation.

There is a lot more out there; for instance see the UCSF's Chimera documentation: https://www.rbvi.ucsf.edu/chimerax/docs/user/commands/perframe.html
