# Information
- **Time:** 14:15-14:40
- **Attendees:** Bob Zhang (Supervisor), Mai Jiajun, Huang Yanzhen 
# Discussion Summary
## Code Optimization
- Stress Testing: New York Times Square, performance issue
- Current Test Result: Mediapipe takes up too much time
- Interface the `pred_instances` from `mmpose` package with the project classifier.
## Similar Work on GitHub
- **MMPose:** 
	- A well-encapsulated project on GitHub.
	- Use top-down predictor to predict key points.
	- ~130 key points for us to choose.
- **OpenPose:**
	- Advertised to be "faster".
	- Harder to use, very few key points. 
	- Need to study via videos.
## Migration
- Currently, Mediapipe takes up too much time per frame running only on CPU since Mediapipe does not support CUDA.
- Gradually migrate to CUDA-supportive projects, like MMPose, OpenPose, etc.
- Current Issue: MMPose does not work on CUDA12.5 with python 3.8. Need a computer/workstation with lower CUDA version to test if this issue still exists.
## Push Forward: WebSocket Streaming
- Python + Next.js
- Push video frames with base64 encoding to WebSocket.
- Frontend receives base64 and recover video frame. (WIP)
## Others
- Be aware of the mutual-similarity issue.
# Agenda of Next Meeting
- Migration of opensource pose detection projects with properly arranged interfaces.