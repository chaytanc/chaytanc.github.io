---
nav_exclude: true
layout: page
title: Game of Intelligent Life
category: "AI & Research"
description: "A deep learning, convolutional neural network based cellular automata for exploring emergent properties of CNN agents."
permalink: /projects/gil
---
## Table of contents
{: .no_toc .text-delta }
1. TOC
{:toc}

{% assign me = site.staffers | where: 'role', 'gil' %}
{% for staffer in me %}
{{ staffer }}
{% endfor %}


<video src="https://user-images.githubusercontent.com/103079871/207569452-f2d9feaf-f182-42e9-9252-6d154c65c680.mp4" controls="controls" style="max-width: 730px;"></video>


### Abstract

Cellular automata captivate researchers due to the emergent, complex individualized behavior that simple global rules of interaction enact. Recent advances in the field have combined cellular automata with convolutional neural networks to achieve self-regenerating images. This new branch of cellular automata is called neural cellular automata [1]. The goal of this project is to use the idea of neural cellular automata to grow prediction machines. We place many different convolutional neural networks in a grid. Each conv net cell outputs a prediction of what the next state will be, and minimizes predictive error. Cells received either their neighbors colors and fitness as input. Each cell’s fitness score described how accurate its predictions were. Cells could also move to explore their environment.  

### Problem Introduction

Overall, we explore the possibility of emergent behaviors in a multi-agent setting with a simple goal of predicting the next state of the game. By giving each cell agency with a convolutional neural network, we were able to explore more complex multi-agent behavior, giving each agent a short term memory in the form of convolutional parameters, and the ability to explore vs exploit their environment through their output movements. The goal was to explore the types of emerging behavior from groups of CNN agents with a selective pressure toward better predictions of the next state.  

### Question
In an environment where pixel agents are given mechanisms to self-replicate, compete, communicate, and predict, are these channels enough for the emergence of centralized control from distinct agents?

### Related Work

This work was heavily inspired by the paper “Growing Neural Cellular Automata” [1](#references), as well as by the original Game of Life by John Conway [2](#references). The ResNet architecture was also borrowed and modified from this tutorial [3](#references). Finally, the idea of a fitness value for the cells comes from the field of genetic algorithms.  
The guiding question and following philosophical implications are deeply connected to the works of Deleuze and Simondon among other philosophers and physicists.  

### Assumptions
 - Macroscopic wholes can emerge from discretized atomic agents
 - Intelligent, accurate predictions of the world emerge from resource scarcity, thus a system requiring accurate predictions to grow imposes resource constraints correlated to those which life imposes on cell division and growth.
 - We can model primordial selves with self replicating simplified agents given arbitrary, nonstationary bounds on themselves in the form of a pixel.
 - The foundational goal of life is to self replicate, and that self replication does not imply intelligence, but greater intelligence can often improve self replication.
 - Through coordination of self replicated beings, the self can expand.


### Methodology

1. Convolutional neural networks are randomly generated on a 100x100 grid.
   - A cell_grid object keeps track of the object positions.
   - A grid object keeps track of the vectorized form of the cells at the positions.
   - An intermediate cell grid object keeps track of the possibly overlapping movements of cells.
2. We begin the first frame by running forward on all conv nets, passing each one its 3x3 neighboring cells.
3. The outputs are the predicted colors and fitnesses of 3x3 neighbors in the next frame, and a movement represented by five channels.
   - The movement property of cells is updated.
4. The movements of the cells are computed from the output movement channels. This updates the intermediate cell grid.
5. If cells overlap, their fitnesses are used to determine which one takes precedence, resolving the intermediate cell grid.
6. The cell_grid and grid objects are updated based on the resolved intermediate cell grid to get the new frame.
7. The output predicted colors and fitnesses are compared to the actual colors and fitnesses of the new frame.
8. The conv nets are updated using backpropagation.
9. The colors of each conv net in the grid are updated to reflect the changes in network parameters.
10. The fitnesses of each conv net in the grid are updated to reflect the loss received.

### Evaluation

The results of this experiment are exploratory; there was no desired outcome, but rather a desire to understand the effects of various design choices on the simulated behavior. To understand the effects, we evaluated the loss of the convolutional neural networks using mean squared error between a cell’s predicted next frame of the game and the actual next frame of the game.

We test the following two methods of prediction:
- Each cell has partial observability of the 100x100 grid, and also predicts the partial state of the grid (3x3 input and 3x3 output).
- Each cell has partial observability of the 100x100 grid, and predicts the full state of the grid (3x3 input and 100x100 output).

We run the following simulations for a different number of initial cells in the range [30, 100, 500] for partial observability and plot the cell losses per epoch for randomly picked cells on the grid, as well as compute the average cell loss as a measure of the overall prediction strength of the conv net for different initial starting states. 

For each simulation, we perform a grid search over the following hyperparameters:
- Learning Rate = [0.1, 0.05, 0.01, 0.005, 0.001]
- Momentum = [0.9, 0.95, 0.97, 0.99]

### Results

For partial observability with partial state prediction, we plot the losses of 3 cells chosen from the grid:

Experiment 1 - 500 initial cells with 30 epochs
   - best hyperparameters: learning rate = 0.01, momentum = 0.99
   - avg. cell loss: 2017.8651169898762

Figure 1.1:

![wishywashy500](https://user-images.githubusercontent.com/103079871/207540941-f0d02746-e875-4d1a-81dc-b7ba9544afa4.png)

Figure 1.2:

![maybe500](https://user-images.githubusercontent.com/103079871/207541260-c5bf49c0-5817-478c-8831-35684deddf0c.png)

Figure 1.3:

![spiky500](https://user-images.githubusercontent.com/103079871/207541316-aab5714f-4c90-4607-909f-8e82342e9d78.png)

Experiment 2 - 100 initial cells with 30 epochs
   - best hyperparameters: learning rate = 0.01, momentum = 0.99
   - avg. cell loss: 1817.7762443485703

Figure 2.1:

![survivor100](https://user-images.githubusercontent.com/103079871/207541461-1d228757-963c-406d-b9dc-6d854102dd39.png)

Figure 2.2:

![superspiky100](https://user-images.githubusercontent.com/103079871/207541558-5c66ae52-661c-4508-b3a1-89261859b1ca.png)

Figure 2.3:

![spiky100](https://user-images.githubusercontent.com/103079871/207541587-4ff85a96-2364-46fb-ae88-00785d2c23b1.png)

3. 30 initial cells with 30 epochs
   - best hyperparameters: learning rate = 0.005, momentum = 0.97
   - avg. cell loss: 1502.6859429765955

Figure 3.1:

![strange30](https://user-images.githubusercontent.com/103079871/207541623-577a6a2c-1de5-4ef9-a7d5-57ce226f0190.png)

Figure 3.2:

![safe30](https://user-images.githubusercontent.com/103079871/207541645-1fcbd00c-3630-4c3a-af2e-c27562c87dc7.png)

Figure 3.3:

![safeagain30](https://user-images.githubusercontent.com/103079871/207541670-39467ad7-e497-419a-9175-a7560754be7f.png)

### Discussion

Using a ResNet architecture, we ran into many issues with upsampling the 3x3 input to a 100x100 output prediction over the whole grid. The result was a very high and unpredictable loss which did not converge. We found that Pytorch’s given methods of upsampling for convolutional neural networks are not well suited for large changes in input sizes, and we needed to add over 20 cells of padding to get the desired output size. We felt this was one reason for the high loss, so we switched to a method of predicting only the cell’s neighbors. This meant that the cell’s input and output were both 3x3x9. 

We found that in the former situation where we tried to predict the full state of the grid given partial observability, the loss spiked up and never converged. In the latter experiments 1-3, the loss does spike, but often returns to a low baseline (seen in figures 1.1-1.3, 2.2, 2.3) and may sometimes converge to a lower value (seen in figures 2.1, 3.1-3.3). Although the cause of these results are not exactly certain, we hypothesize that the spiking behavior is due to the cell network learning the position of its neighbors, and then experiencing high loss when neighboring cells move. And for other cells, we hypothesize that the converging behavior occurs when cells move around the grid and encounter few/no neighbors, resulting in more accurate predictions as new cells would not suddenly appear in their field of vision. This may also be supported by the fact that the average cell loss is lower when we start the game with less initial cells (shown in experiments 1-3), as the CNN has a harder time predicting the next state of the game when cells are constantly moving in/out of the 3x3 neighbor grid. We theorize that the loss values are high due to the movement being partially determined by the output of a network and partially determined by stochastic noise (where each cell has a 0.1 probability of taking a random action). This can be difficult for cells to learn and may be one source of high/spiking losses.

The next step may be to train the CNN on the full state/a larger partial state instead of the 3x3 partial state neighbors. By giving the CNN more information, each cell may be better able to predict the next state of the game. Preliminary investigations of this have shown that this significantly increases training time and computation cost, so running this on powerful devices (GPUs/TPUs) is recommended. 

Beyond that, we propose the following future implementations:
- Weight sharing or mixing between cells
   - Instead of each cell having to learn individually, it can instead 'absorb' the learned parameters of other cells to improve itself. This may lead to      faster convergence and a lower average loss per cell.
- Random reproduction of highest fitness cells to allow for clusters of high fitness cells
   - Similar to weight sharing: allows cells with the highest predictive power to clone themselves, leading to faster convergence and lower average cell        loss.
- Better recurrent CNN implementation
- Random death of lowest fitness cells
   - Removes cells with diverging loss from the game, leading to a more biologically accurate model.
- Channels through which convolutional networks can send scalar signals to other networks and in a sense communicate
   - Allows for the model to emulate speech/language between cells for more interesting effects.
- Neighbor’s predictions of a cell’s fitness impacting the fitness of the cell in question
   - Allows for the model to emulate a 'horde mentality' between the cells for more interesting effects.

### Possible Use Cases

- Self assembling images (similar to [1](#references))
- Modeling biological systems – bacteria colonies, multicellular organism formation
- Modeling physical systems at a particle level
- Economics, and other dynamic multi-agent systems with partial observability



### Demo and Code
[Source Code](https://github.com/chaytanc/game-of-intelligent-life/tree/main)
<iframe src="https://giphy.com/embed/UtGpYehPfGjCkVjyv2" width="480" height="383" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/UtGpYehPfGjCkVjyv2">demo 1</a></p>
<iframe src="https://giphy.com/embed/h1DAaQ4c4RsWDSXPO2" width="480" height="383" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/h1DAaQ4c4RsWDSXPO2">demo 2</a></p>
<iframe src="https://giphy.com/embed/7ezz5x4QCypeJ29c6c" width="480" height="383" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/7ezz5x4QCypeJ29c6c">old buggy (but cool!) demo</a></p>

### Philosophical Implications
*by Chaytan*  

First, the code is lacking several key features I originally intended:  
 - The fitness of the cell is not determined by neighboring cell predictions
 - Cells cannot currently reproduce themselves
 - Due to computational infeasibility, cells are not given the network of their neighbors  

However, although the final result is not yet a useful model of multi-agent interaction and the emergence and shaping 
borders of self that I imagined it might become, design choices and observations of the process of creating agency 
clarify several implications of the inspiring text, Simondon’s Genesis of the Individual.  
 1. Atomic cells, without a provided mechanism for reproduction, do not show the capacity for recapitulating similarities in their local environment
 2. Without communication, cells do not interact or form group dynamics, nor can they assemble larger selves through autopoiesis
 3. Without the ability to change their own internals, cells are trapped within algebraically deterministic environment as opposed to the rich, differential equations in which we live  

Both 1) and 2) seem self-evident, and would not require a simulation to discover. Yet their implications are profound, and certainly worth internalizing.  
1)  
The process of individuation that we recognize does not exist without considering a means of reproduction. 
Corroborating this observation is Gilles Deleuze’s Desert Islands [\[5\]](#references): 
“The animal whose mode of reproduction remains unknown to us has not yet taken its place.” Thus, with respect to the original question, 
regarding the emergence of centralized control, although the conditions for which it arises are yet unknown, we know one precondition is reproduction; self-replication.  
2)  
Although this too seems quite a tautology, perhaps we should meditate on the implications more because Simondon and Deleuze’s 
concept of the preindividual and the plane of immanence respectively seem at odds with the statement. The statement posits that 
communication requires boundaries and spaces in between. Our selves make themselves of smaller biological cells which are themselves 
individuating selves. The whole self is then forms of communication between them. Our milieu, itself thus having boundaries and smaller 
selves, is not “zero intensity” as described by Deleuze’s plane of immanence. Our individuation comes from colluding smaller individuations. 
And before those, perhaps too there were more reproductions of individuation. The concept of the preindividual is perhaps fallacious. Or can 
we really become from a pure gradient? Must we assume a fundamental individual or can individuals form from smooth fields of energy? I 
suggest that resolving this dispute merits delving into the phenomenological philosophy of physics.  

We currently use roughly three contradictory frameworks to describe objects in space which we apply according to the scale of the object
relative to ourselves. In brief, general relativity (GR) for big, Newtonian for “can wrap-your-head-around-it”, and quantum 
for unimaginably small sizes. The struggle is between quantum and relativistic scales, whereas Newtonian we can consider a 
dumbed down subset of general relativity’s equations. One problem is this: GR says mass bends spacetime; quantum says 
particles have mass; quantum also says particles exist in superpositions of multiple places at once; GR does not concede 
that all those places are bending spacetime at once, in fact it’s really confused by the whole idea of multiplicities. So 
what does this mean for our friends Simondon and Deleuze? It means that a rigorous dive into the processes of describing 
the mechanics of individuated objects is at odds with our best explanations for their individuations, which arise from 
quantum field theory (QFT) [\[9\]](#references). It means that Simondon and Deleuze, whose ideas of milieux and planes of immanence neatly 
correspond to QFT, would fail to describe the actual motion and movement of larger scales of objects, which we observe 
as warping spacetime *at a single point*, not at a superposition of states.  

Quantum field theory frames particles as disturbances of underlying fields, much in the spirit of Deleuze’s plane of 
immanence or Simondon’s milieu. But this framework does not apply in physics to the scales at which Simondon and Deleuze 
apply their ideas. The idea of probabilistic embodiments of many forms at once has not been reconciled with ideas of 
general relativity. So the presumption that we individuate from the individuated, rather than a soupy milieu akin to 
fields in quantum field theory is perhaps the more founded assumption. But really, if physics’ sojourn into the 
philosophical and pragmatic explanations of individuation teaches us anything, it is perhaps that our explanations of 
particle based quantum superpositions versus those presupposing individuated matter such as general relativity However, 
I would be remiss if I did not point out that particles are frequently ‘made’ or individuated by scientists at particle 
accelerators like CERN by hypothetically exciting the underlying fields enough for particles like the Higgs boson to emerge. 
So +10 points Simondon. But describing how the gravity of such particles causes them to behave? -10 points Simondon.  

To do Simondon and Deleuze a little more justice, their theories do not perfectly map to quantum field theory. Quantum 
field theory relies on discretely quantized excitations of underlying fields to account for the emergence of particles 
rather than the Deleuzian picture of arbitrary energetic-value excitations of a plane of immanence resulting in emergence. 
Could such an equivalent be found in physics? It would not go far in explaining the discretized packets of energy and mass 
we observe in ‘fundamental’ particles like electrons and photons, so may not be of much use to quantum mechanics, though 
it may be of more use to GR which likes to describe energy in such arbitrary quantities. Nevertheless, although I have 
not been shy of writing about phenomena I do not fully understand thus far, I will have to refrain from trying to further 
answer that question in order to cling to remaining shreds of correctness.  

So do we need a conciliatory philosophy or physics? Well, as for the physics front, we have a few options. But Simondon 
may have a few words to exchange with these physicists. One option is to limit the granularity of spacetime. Beyond the 
a certain granularity, gravity no longer applies. This view is touted by physicist Craig Hogan [\[6\]](#references). The most prominent theory, string 
theory, ideates that matter is formed from finitely small loops [\[6\]](#references). A similar trend in unification theories is the presupposition 
of the substances of individuated (quantized) matter, be it indivisible boundaries of finiteness or finitely small strings, 
and guess who has something to say about that?  

“The world and the living being are already contained in the tropistic unity... as a polarity of a gradient that locates 
the individuated being in an indefinite dyad at whose median point it can be found, and upon which it bases its further exfoliation. 
Perception, and later Science itself, continue to resolve this problematic, not only with the invention of spatiotemporal 
frameworks but also with the constitution of the notion of an object, which then becomes the ‘source’ of the original 
gradients and organizes them among themselves as if they were an actual world.” Harsh words from the philosopher [\[4\]](#references).  

In trying to avoid using individuations as the ‘source of the original gradients’, Simondon uses a relativistic 
approach of explaining what is, yet rather ended up with a quantum field theory-esque probabilistic explanation which 
cannot apply on scales beyond the unobservable because life as we most often observe it is deterministic. So tough luck 
for physics and philosophy, because our best explanations are at odds, and so are their reconciliations. (Well, string 
theory is consistent, but predictions like supersymmetric particles have yet to be observed [\[8\]](#references)). Moreover, if these 
reconciliations via presumed finitudes are not a satisfactory response to the mysterious machinations of the universe, 
you may, treacherously, have to divert the question to branches of metaphysics. Yikes.  

Brief interlude on the two body problem in GR: no closed form solutions are known to exist for Einstein’s GR equations
when there are two bodies of mass in the system [\[7\]](#references). That is because (in my humble, non-physicist opinion) the question 
doesn’t exist. GR is predicated on the whole of spacetime describing the motion of other areas within it. There aren’t 
even two bodies in the equation, there is only the whole system. Drawing arbitrary lines around things within the system 
and calling them objects breaks the assumptions which define the equations, namely that all of spacetime curves and bends 
according to distributions of mass which are themselves distributed by the curvature of spacetime. You can solve for the 
effect of one point of mass because you can treat that one ‘object’ as being the whole of spacetime. When you introduce 
an ‘other object’ then you can no longer address the whole system at once, for what would you define as other after the 
first object has been accounted for? Perhaps our tendency of arbitrarily drawing lines around things and labeling them 
as objects has met fundamental limitations.  

So what is left of our journey into the physical descriptions of emerging individuation? What does my little cellular
model of individuation have anything to do with all this anyway? There is more to be discussed in the implications of 
the philosophical and physical formulations of time, but that is for another project. For now, we have a relativistic 
perspective, which observes the continuity of our macroscopic world and presumes some fundamental matter that can 
continuously interact and exchange energy and mass. What this might be or how it comes about is yet inexplicable. 
And we have strong predictions of individuated matter that come from continuous background fields of energy, but that 
cannot explain the deterministic and macroscopic behavior of matter when these individuations form larger wholes. And 
we have Simondon and Deleuze, whose ideas, unpresupposing of the individual, tend more towards the latter formulation, 
and we have my own little simulation, presupposing some form of individuated atomic that can form larger wholes, tending 
towards Einstein’s camp, since the more useful arena in which I would like to apply the ideas are individuation is that 
of the macroscopic world.  

As we can see, the seemingly tautological observation naturally emerging from a simple attempt to model individuation 
is actually founded on deeply contested centuries of observation, modeling, theorizing, and debating far from settled 
in either philosophical or phenomenological physical realms.  

3)  
This last observation is true by the structure of differential equations, which are the foundations of the equations 
describing our universe, regardless of whether you choose the relativistic or quantum equations to do so, as well as 
being the equations which underpin descriptions of biological interactions of cells and proteins. Through and through, 
models of the world must be able to take actions which in turn affect that which took the action. The process of making 
this simulation has revealed the foundational opposition which computing takes to this kind of process. Bits are to be 
flipped by something else, and the process of flipping them is not to further interact and reverberate within the system. 
It must start and stop without further effects upon itself. Programs are to be run without the results of their 
computations ever changing the structures that produced the computation. Yet, as is clear by the deficits of the model 
produced as well as mathematical descriptions of the universe, these simplified models do not describe individuation.  

And now I am far out of my depth, but nevertheless I am a big proponent of talking out of your ass in a genuine attempt 
to understand and continue a dialogue as mistakes are uncovered through shoddy discourse. So there it is – a theoretical, 
philosophical, and practical examination into ideas of the process of individuation.  

### References

[\[1\] Growing Neural Cellular Automata](https://distill.pub/2020/growing-ca/) Mordvintsev, Alexander, et al. “Growing Neural Cellular Automata.” Distill, 27 Aug. 2020, https://distill.pub/2020/growing-ca/.  
[\[2\] Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) “Conway's Game of Life.” Wikipedia, Wikimedia Foundation, 15 Dec. 2022, https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life.   
[\[3\] ResNet From Scratch in Pytorch](https://blog.paperspace.com/writing-resnet-from-scratch-in-pytorch/)   
[\[4\] Genesis of the Individual](https://www.scribd.com/document/228161394/Gilbert-Simondon-Genesis-of-the-Individual.) Simondon, Gilbert. “Gilbert Simondon - Genesis of the Individual.” Scribd, Scribd, https://www.scribd.com/document/228161394/Gilbert-Simondon-Genesis-of-the-Individual.  
\[5\] Desert Islands Deleuze, Gilles, et al. Desert Islands: And Other Texts 1953-1974. Semiotext(e), 2004.  
[\[6\] Relativity v Quantum Mechanics](https://www.theguardian.com/news/2015/nov/04/relativity-quantum-mechanics-universe-physicists) Powell, Corey S. “Relativity v Quantum Mechanics – the Battle for the Universe.” The Guardian, 4 Nov. 2015, https://www.theguardian.com/news/2015/nov/04/relativity-quantum-mechanics-universe-physicists.  
[\[7\] General Relativity Two Body Problem](https://en.wikipedia.org/wiki/Two-body_problem_in_general_relativity) “Two-Body Problem in General Relativity.” Wikipedia, Wikimedia Foundation, 29 Oct. 2022, https://en.wikipedia.org/wiki/Two-body_problem_in_general_relativity.  
[\[8\] String Theory](https://www.forbes.com/sites/startswithabang/2020/02/26/why-string-theory-is-both-a-dream-and-a-nightmare/?sh=38683fba3b1d) Siegel, Ethan. “Why String Theory Is Both a Dream and a Nightmare.” Forbes, Forbes Magazine, 26 Feb. 2020, https://www.forbes.com/sites/startswithabang/2020/02/26/why-string-theory-is-both-a-dream-and-a-nightmare/?sh=38683fba3b1d.  
[\[9\] General Relativity](https://en.wikipedia.org/wiki/General_relativity) “General Relativity.” Wikipedia, Wikimedia Foundation, 14 Dec. 2022, https://en.wikipedia.org/wiki/General_relativity.  
