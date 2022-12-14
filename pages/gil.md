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

We run the following simulations for a different number of initial cells in the range [30, 100, 500]. Each cell has partial observability of the 100x100 grid, and also predicts the partial state of the grid (3x3 input and output).

For each simulation, we perform a grid search over the following hyperparameters:
- Learning Rate = [0.1, 0.05, 0.01, 0.005, 0.001]
- Momentum = [0.9, 0.95, 0.97, 0.99]

### Results

1. 500 initial cells with 30 epochs
   - best hyperparameters: learning rate = 0.01, momentum = 0.99
   - avg. cell loss: 2017.8651169898762

2. 100 initial cells with 30 epochs
   - best hyperparameters: learning rate = 0.01, momentum = 0.99
   - avg. cell loss: 1817.7762443485703

3. 30 initial cells with 30 epochs
   - best hyperparameters: learning rate = 0.005, momentum = 0.97
   - avg. cell loss: 1502.6859429765955

### Discussion

Using a ResNet architecture, we ran into many issues with upsampling the 3x3 input to a 100x100 output prediction over the whole grid. The result was a very high and unpredictable loss which did not converge. We found that Pytorch’s given methods of upsampling for convolutional neural networks are not well suited for large changes in input sizes, and we needed to add over 20 cells of padding to get the desired output size. We felt this was one reason for the high loss, so we switched to a method of predicting only the cell’s neighbors. This meant that the cell’s input and output were both 3x3x9. 

The effect of this is shown in the difference between Figure 1 and Figures 2, 3. In the former, the loss spiked up and never converged. In the latter figures, the loss does spike, but often returns to a low baseline. Although the cause of these results are not exactly certain, we hypothesize the spiking behavior seen in Figures 2 and 3 is due to the cell network learning the position of its neighbors, and then experiencing high loss when neighboring cells move. The movement is partially determined by the output of a network but also partially stochastic noise. This can be difficult for cells to learn and may be one source of spiking losses.

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


