---
nav_exclude: true
layout: page
title: Game of Intelligent Life
permalink: /gil
---
## Table of contents
{: .no_toc .text-delta }
1. TOC
{:toc}

{% assign me = site.staffers | where: 'role', 'gil' %}
{% for staffer in me %}
{{ staffer }}
{% endfor %}


### Abstract

Cellular automata captivate researchers due to the emergent, complex individualized behavior that simple global rules of interaction enact. Recent advances in the field have combined cellular automata with convolutional neural networks to achieve self-regenerating images. This new branch of cellular automata is called neural cellular automata [1]. The goal of this project is to use the idea of neural cellular automata to grow prediction machines. We place many different convolutional neural networks in a grid. Each conv net cell outputs a prediction of what the next state will be, and minimizes predictive error. Cells received either their neighbors colors and fitness as input. Each cell’s fitness score described how accurate its predictions were. Cells could also move to explore their environment.  

### Problem Introduction

Overall, we explore the possibility of emergent behaviors in a multi-agent setting with a simple goal of predicting the next state of the game. By giving each cell agency with a convolutional neural network, we were able to explore more complex multi-agent behavior, giving each agent a short term memory in the form of convolutional parameters, and the ability to explore vs exploit their environment through their output movements. The goal was to explore the types of emerging behavior from groups of CNN agents with a selective pressure toward better predictions of the next state.  

### Related Work

This work was heavily inspired by the paper “Growing Neural Cellular Automata” [1](#references), as well as by the original Game of Life by John Conway [2](#references). The ResNet architecture was also borrowed and modified from this tutorial [3](#references). Finally, the idea of a fitness value for the cells comes from the field of genetic algorithms.  

### Methodology

1. Convolutional neural networks are randomly generated on a 100x100 grid 
   - A cell_grid object keeps track of the object positions 
   - A grid object keeps track of the vectorized form of the cells at the positions 
   - An intermediate cell grid object keeps track of the possibly overlapping movements of cells
2. We begin the first frame by running forward on all conv nets, passing each one its 3x3 neighboring cells
3. The outputs are the predicted colors and fitnesses of 3x3 neighbors in the next frame, and a movement represented by five channels
   - The movement property of cells is updated
4. The movements of the cells are computed from the output movement channels. This updates the intermediate cell grid
5. If cells overlap, their fitnesses are used to determine which one takes precedence, resolving the intermediate cell grid
6. The cell_grid and grid objects are updated based on the resolved intermediate cell grid to get the new frame
7. The output predicted colors and fitnesses are compared the actual colors and fitnesses of the new frame
8. The conv nets are updated using backpropagation
9. The colors of each conv net in the grid are updated to reflect the changes in network parameters
10. The fitnesses of each conv net in the grid are updated to reflect the loss received

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

We found that in the former, the loss spiked up and never converged. In the latter figures from experiments 1-3, the loss does spike, but often returns to a low baseline (seen in figures 1.1-1.3, 2.2, 2.3) and may sometimes converge to a lower value (seen in figures 2.1, 3.1-3.3). Although the cause of these results are not exactly certain, we hypothesize the spiking behavior seen in Figures 2 and 3 is due to the cell network learning the position of its neighbors, and then experiencing high loss when neighboring cells move. The movement is partially determined by the output of a network but also partially stochastic noise. This can be difficult for cells to learn and may be one source of spiking losses.

- Channels through which convolutional networks can send scalar signals to other networks and in a sense communicate
- Neighbor’s predictions of a cell’s fitness impacting the fitness of the cell in question
- Random reproduction of highest fitness cells to allow for clusters of high fitness cells
- Random death of lowest fitness cells
- Weight sharing or mixing between cells
- Better recurrent CNN implementation

**Possible Use Cases** 
- Self assembling images (similar to [1](#references))
- Modeling biological systems – bacteria colonies, multicellular organism formation
- Modeling physical systems at a particle level
- Economics, and other dynamic multi-agent systems with partial observability



### Demo and Code
[Source Code](https://github.com/chaytanc/game-of-intelligent-life/tree/main)
<iframe src="https://giphy.com/embed/XjZHMeJsmxZ2SKQrod" width="480" height="383" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/XjZHMeJsmxZ2SKQrod">demo 1</a></p>
<iframe src="https://giphy.com/embed/h1DAaQ4c4RsWDSXPO2" width="480" height="383" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/h1DAaQ4c4RsWDSXPO2">demo 2</a></p>
<iframe src="https://giphy.com/embed/7ezz5x4QCypeJ29c6c" width="480" height="383" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/7ezz5x4QCypeJ29c6c">old buggy (but cool!) demo</a></p>

### References

[\[1\] Growing Neural Cellular Automata](https://distill.pub/2020/growing-ca/)  
[\[2\] Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)  
[\[3\] ResNet From Scratch in Pytorch](https://blog.paperspace.com/writing-resnet-from-scratch-in-pytorch/)  


