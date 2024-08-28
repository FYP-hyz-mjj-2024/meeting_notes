# Discussion Summary
## Current Work Demonstration
- Demonstrated the working framework of the project, including a YOLO-based multiple-people locator merged with a Mediapipe posture detection system.
- Demonstrated the annotation of the image, i.e., to use mediapipe's posture detection model to convert an image of a single person into a vector of key angles.
## Things to Improve
- **Add Restriction on YOLO Detection.** 
	- The YOLO model would attempt to recognize things that's out of our scope of targets (person+phone). Try to force it to only recognize person and phone to save performance.
- **Improve Eye-Related Detection.** 
	- The current model needs to improve the recognition of pedestrian's eyes, in order to detect whether they are looking at their phones or not. Detection criterion could include:
		- Lowering Head
		- Angle of key landmarks on their eye
- **Combine key angles and confidence.**
- **More Exploration on Classification Model**
	- Currently only using Random Forest for demo. Try other methods like SVR/CNN/....
	- Cross-check parameter tuning for a better classification model.
# Agenda of Next Meeting
 - Keep the current framework. Demonstrate an improved self-trained classification model.
 - Discuss about how the code could be optimized so as to boost performance.
 - More exploration on the combination of methods.
# Literature Survey
- Discussed about the source of literature survey, including *code with paper* and *google scholar*.
- Explore similar works on GitHub.