# Work Summary
This is the work summary of week Nov. 18 ~ Nov. 22.
## 1.  Undetectable Pedestrians
We introduced a new process of filtering out pedestrians that has low outcome of posture detection. These pedestrians will be ignored by the classifier to leverage performance on the detectable individuals. Currently, we have 2 undetectable pedestrians:
- Back (Discussed in previous meetings ): The pedestrian is showing its back to the camera.
	- Criteria: The backside ratio is smaller than -0.2 and confidence of the landmarks of the two shoulder is higher than 0.3.
	- Backside ratio = $\frac{l\_shoulder\_x-r\_shoulder\_x}{width\_bbox}$
- Out of frame (New): Most of the landmarks of this pedestrian is out of frame.
	- Criteria: More than 5 points of the scores of the first 13 landmarks is smaller than 0.3.
<a href="https://sm.ms/image/9HF7QYDRow4IWbv" target="_blank"><img src="https://s2.loli.net/2024/11/23/9HF7QYDRow4IWbv.png" ></a>

We use the binary digits of an integer to denote such filtering states for great extendability.

|                                      | Base-10 Integer | Binary Integer |
| ------------------------------------ | --------------- | -------------- |
| `OK_CLASSIFY`                        | 0               | `0000, 0000`   |
| `OUT_OF_FRAME`                       | 1               | `0000, 0001`   |
| `BACK_SIDE`                          | 2               | `0000, 0010`   |
| Possible future<br>filtering state 1 | 4               | `0000, 0100`   |
| Possible future<br>filtering state 2 | 8               | `0000, 1000`   |
By `AND`-ing the classification state with the filtering state, we could efficiently determine whether to classify one person or not: Perform classification if and only if the `AND` result is 0.

P.S. `OUT_OF_FRAME` has higher priority over `BACK_SIDE`. That means, if both of them is fulfilled, by the current mechanism, only `OUT_OF_FRAME` will be reported. This is done to save performance from the algorithm giving un-useful determinations.

## 2. YOLOv11
### 2.1 Model Training (In Progress)
Python package of `ultralytics` and `roboflow` is used. Roboflow's online workspace is used to manage training data to train a pre-trained YOLOv11 model to detect specifically for phones. (Training is done locally, we added a `step03_yolo_phone_detection` directory in the `yolo_dev` branch of `posture_mmpose`).

Data are currently from Google and Baidu. Original data has about 200 pictures. Around 600 individual training data is obtained after various image augmentations.

The performance of the model is still under testing.
### 2.2 Interface Exposure
We expose the interface for the YOLOv11 model by cropping the sub-pictures of a person's hand and feed them into the model as inputs. 

The center of the sub-frame is determined by:
- The landmark of ones wrists, and
- the width of the bounding box:
- $l\_center = normalize(lm\_l\_wrist-lm\_l\_elbow)\times \frac{bbox\_w}{5}$
- The center is obtained by offsetting the landmark of the wrist by a unit vector with the same direction of the pedestrian's arm with the length of a fixed factor determined by the bounding box.

The width & height of the sub-frame is determined by the width & height of the bounding box.
- $hand\_w = \frac{bbox\_w}{5}$
- $hand\_h=\frac{bbox\_h}{5}$

<a href="https://sm.ms/image/po1nJ9VwHvrbS3q" target="_blank" ><img src="https://s2.loli.net/2024/11/23/po1nJ9VwHvrbS3q.png" style="width: 400px"></a>
