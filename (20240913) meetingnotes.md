# Information
- **Time:** 13:20
- **Attendees:** Bob Zhang (Supervisor), Mai Jiajun, Huang Yanzhen

# Meeting Agenda
## Config for MMPose
- We discovered the exact config for MMPose to run on cuda.
- The required environments and config steps is listed in README.md of repository `posture_mmpose`.
## Performance of MMPose
- Lower process time per frame compared to Mediapipe.
- Comes with built-in object detection.
- The original `posture` will be archived as a backup.
## Recall from the first meeting: Scores of angles
- Geometric average of scores of three points of the angle
- $angle\_score = (score_1\times score_2\times score_3)^{\dfrac{1}{3}}$
- Backup calculation plan: Harmonic Mean (Harder for computers to compute, lower accuracy).
## Neural Network:
- Input: Each person flattened.
	- Each Person:
		- $9$ key angles, along with the angle score
		- Flattened into a $18$-d vector
	- Input matrix is of shape $(num\_test\times 18)$
	- Normalize Value:
		- Normalized angle value into $[0,1]$ by dividing $180$.
		- Normalized entire input matrix using Z-value approach, i.e., a normal distribution with $\mu=0$ and $\sigma=1$.
- Output: 2 classes
- Structure:
	- 4 fully-connected layers
	- use ReLU as activation function
- Training Parameters:
	- input size = $18$
	- hidden size = $100$
	- learning rate = $0.01$
	- number of epochs = $1000$
- Current Issue:
	- Loss don't come below $0.5$ since around the $160^{th}$ epoch.
## Web Streaming
- Added Pause function