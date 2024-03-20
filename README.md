# Drawing-CoCreative-Agent

## Brief Description
A turn-taking system which assists a person in drawing. This means that the user draws a line and then the system reacts with a line of its own. I call this an episode. The user has full control of an episode. In other words, the user can draw one or more lines by using the settings in the bottom right box "Tools" and then let the agent do one or more moves by clicking any one of the seven buttons in the upper right box "Co-creative Agent". By doing this, there is more flexibility if a user wants to draw more and let the agent "follow" or vice versa.

## User Moves
When it is time for the user to draw something, he/she should press the left mouse button and drag the mouse toward the desired direction. The user can modify the way he/she draws by changing the brush size, the color, and the pen type. When something is not as expected then the user can also use the eraser or clear the canvas and start all over again. During drawing, these settings are collected in real-time and enriched with some metadata such as how many points a line has, the starting and ending points if the size of the canvas has changed or not, and many more. The reason behind that is to store and preprocess them in order to be used by the agent as well.

## Agent Moves
On the other hand, the agent is activated as soon as the user presses any of the following options: "Similar Pattern", "Replicate Drawing", "Horizontal Mirror", "Vertical Mirror", "Merge Drawing", "Random Pattern", and "Balance Canvas". Note that, these seven actions are crucial to form the three different layers of perceptual logic.. These options are grouped and described below:
- "Similar Pattern", "Random Pattern": Markov Chains models are utilized for generating patterns. I maintain two of them since the first one is related to the episode (and thus I reset it for every episode), whereas the second one is a global model which stores all the interactions through episodes. Both models use 2D points as states and retain a transition matrix to move from one state to another using relative paths. In other words, the pen positions are encoded as relative to the previous one, which made it possible to generalize over the canvas. In addition, I use 2nd and 3rd-order Markov Chain models to preserve more information about the paths and thus capture complex dependencies and increase the overall accuracy.
- "Replicate Drawing": The agent draws the exact drawing that the user did in the current episode. Simpler, the agent just produces a duplicate without considering whether the user used one or more lines in the episode.
- "Horizontal Mirror", "Vertical Mirror": This is similar to the above option "Replicate Drawing". The agent mirrors the drawing of the user in the current episode either horizontally or vertically.
- "Merge Drawing": In contrast to the above operations which belong to the Local perceptual logic because they modify individual lines, this operation is about Regional perceptual logic. This is because it groups lines into regions.
- "Balance Canvas": The last one is related to the Global perceptual logic. With this option, the agent can consider the relationship between groups to evaluate the overall composition. This is a special operation since it executes one of the above six operations and defines the position of the drawing to be as novel as possible by utilizing the Novelty Search algorithm.

## Positioning
The posision of each line is decided in a probabilistic way for the first six operations, whereas in the last one, it is determined by the Novelty Search algorithm. Regarding the probabilistic way, drawings may be placed in a position either within a radius of a previous drawing or in a symmetric place (for example if the user drew something in the left upper corner then the agent might draw something in the right upper corner, left bottom corner or even right bottom corner). On the other hand, the Novelty Search algorithm helps us define a novel place for a drawing. By doing so, the user does not care about crowded drawings in an area of the canvas but he/she uses the agent to identify the biggest blank area and draw something to maintain the balance. This is done in three steps:
1. Generate a number of random 2D points on the canvas (in particular, I generate 10 random points). 
2. Get the K-nearest neighbors (in my system K=5). In other words, find the K-nearest points that are either part of the user's lines or the agent's ones. 
3. Get the most novel point; meaning the point which is the furthest from its neighbors.


## Files
- co-creative_system.py: The painting system. Essentially, this is the brain of the whole repository since the most important things are handled here such us initializing the visual aspect of the system, defining episodes (when the agent is expected to draw something or it is the user's turn), storing metadata of user's drawings and many more.

- agent.py: Agent's eligible moves. This file hosts the implementation of all the options analyzed above. For example "Similar Pattern", "Replicate Drawing", etc.

- helper.py: This file has all the methods used by both files above. It also holds methods that used by either one of the files above to make the repository well-organized. 


## How to run 
python co-creative_system.py

## License
MIT License