# Final Report

## Rubric

### Task FP.1 Match 3D Objects

The _matchBoundingBoxes_ method which takes as input both the previous and the current data frames and provides as output the ids of the matched regions of interest has been implemented. Matches are the ones with the highest number of keypoint correspondences. [Refer _camFusion_Student.cpp_ - Lines 273:329]

### Task FP.2 Compute Lidar-based TTC

The time-to-collision in second for all matched 3D objects using only Lidar measurements from the matched bounding boxes between current and previous frame have been computed. By extracting points withing the ego lane only, the code is able to deal with outlier Lidar points in a statistically robust way to avoid severe estimation errors. [Refer _camFusion_Student.cpp_ - Lines 219:271]

### Task FP.3 Associate Keypoint Correspondences with Bounding Boxes

Keypoint correspondences have been associated to the bounding boxes which enclose them. All matches which satisfy this condition have been added to the "kptMatches" vector in the respective bounding box. Also, outlier matches have been removed based on the euclidean distance between them in relation to all the matches in the bounding box. [Refer _camFusion_Student.cpp_ - Lines 134:165]

### Task FP.4 Compute Camera-based TTC

The time-to-collision in second for all matched 3D objects have been computed using only keypoint correspondences from the matched bounding boxes between current and previous frame. By using the medians, the code is able to deal with outlier correspondences in a statistically robust way to avoid severe estimation errors. [Refer _camFusion_Student.cpp_ - Lines 169:217]

### Task FP.5 Performance Evaluation 1

By inspecting the top view perspective of Lidar points and comparing with the TTC Lidar estimate, the TTC at following frames were observed to have been wrongly estimated. [Refer _EvaluationTask.xlsx_]

|S/N|Image No|TTC-Lidar (s)|X-min (m)|
|---|---|---|---|
|1|3|16.385|7.85|
|2|15|8.321|7.13|
|3|17|11.030|6.83|

In the cases of Image frames 3 and 17, the TTC suddenly increased while the x-axis distance decreases consistently, while for image frame 15, the TTC suddenly drops in a disproportionate way compared to the change in x-distance.

This is not unconnected to the outliers in Lidar points. The points in the ROI sometime cover more than the immediate back of the preceding car.

### Task FP.6 Performance Evaluation 2

This evaluation was done using the standard deviation metric. For each detector / descriptor combinations implemented, the TTC estimate on a frame-by-frame basis is evaluated and the best 3 are selected with lowest standard deviation of error from the TTC LIDAR estimate. [Refer _EvaluationTask.xlsx_]

#### The TOP3 by Standard Deviation of estimation error

1. AKAZE/FREAK - 1.17s
2. AKAZE/BRISK - 1.32s
3. AKAZE/ORB - 1.53s

#### TTC Camera Estimation Error

Particularly for the ORB Detector, there were some outrageous TTC estimates. Some examples are shown below.

|S/N|Combination|Image No|TTC Estimate (s)|
|---|---|---|---|
|1|ORB/BRISK|12|5416.2|
|2|ORB/BRIEF|9|2830000|
|3|ORB/ORB|12|-inf|
|4|ORB/FREAK|12, 17|-inf|

This is not unconnected to the sensitivity of the ORB detector.
