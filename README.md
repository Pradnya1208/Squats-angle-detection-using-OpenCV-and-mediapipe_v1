
<div align="center">
  
[1]: https://github.com/Pradnya1208
[2]: https://www.linkedin.com/in/pradnya-patil-b049161ba/
[3]: https://public.tableau.com/app/profile/pradnya.patil3254#!/
[4]: https://twitter.com/Pradnya1208


[![github](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c292abd3f9cc647a7edc0061193f1523e9c05e1f/icons/git.svg)][1]
[![linkedin](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/9f5c4a255972275ced549ea6e34ef35019166944/icons/iconmonstr-linkedin-5.svg)][2]
[![tableau](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/e257c5d6cf02f13072429935b0828525c601414f/icons/icons8-tableau-software%20(1).svg)][3]
[![twitter](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c9f9c5dc4e24eff0143b3056708d24650cbccdde/icons/iconmonstr-twitter-5.svg)][4]

</div>

# <div align="center">Squat angle detection using OpenCv and Mediapipe</div>
<div align="center"> 
<img src= "https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/output/output2.gif?raw=true" >
</div>

## Overview:
Human pose estimation from video plays a critical role in various applications such as quantifying physical exercises, sign language recognition, and full-body gesture control. For example, it can form the basis for yoga, dance, and fitness applications. It can also enable the overlay of digital content and information on top of the physical world in augmented reality.

## ML Pipeline:

The solution utilizes a two-step detector-tracker ML pipeline. Using a detector, the pipeline first locates the person/pose region-of-interest (ROI) within the frame. The tracker subsequently predicts the pose landmarks and segmentation mask within the ROI using the ROI-cropped frame as input. Note that for video use cases the detector is invoked only as needed, i.e., for the very first frame and when the tracker could no longer identify body pose presence in the previous frame. For other frames the pipeline simply derives the ROI from the previous frame’s pose landmarks.
<div align="center">
<img src="https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/output/pose_tracking_detector_vitruvian_man.png?raw=true">
</div>

## Objectives:

- In this project we are going to detect the crucial angles in Squat position.
- The squat and deadlift are exercises prescribed by strength and conditioning professionals for the purpose of strengthening the legs, hips, back and torso musculature
- They are considered closed chain, compound lifts involving the integration of multiple joint systems and muscle groups.
- It has also been shown that long-term lifting with squats and deadlifts not only promotes an increase in bone mineral density in young populations, but it may also help maintain this adaptation well into the later stages of life 
- When performed correctly, injuries related to these exercises are uncommon, however poor technique or inappropriate prescription can lead to wide range of issues, especially in combination with heavy weights. Considering the complexity of these exercises and the variables related to performance, understanding the biomechanics is of great importance for achieving optimal muscular development as well as reducing training related injury. 
- Therefore, the purpose of this project is to detect the squat angles which will be helpful for the fitness instructors to provide a corrective advice where appropriate.

## Implementation:

**Libraries:**  `NumPy` `cv2` `Mediapipe`


## The Squat:
The squat starts with the descent phase as the hips, knees and ankles all flex.  A common cue is to descend until the thighs are parallel with the floor, and the hip joint is either parallel or below the knee joint.  Ascent is performed primarily through triple extension of the hips knees and ankles, until the subject returns to the starting position. 
<div align="center">
<img src= "https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/output/squat.jpg?raw=true" width="50%">
</div>

## Mechanics of the squat:
Analysis of the squat can be achieved by sub categorising the movement into three comprehensive domains; upper body, lower body and movement mechanics. The upper body emphasises the stability and posture of the head, neck and torso, the lower body asses the joint positions of hips, knees and ankles, finally the movement mechanics assess the timing and co-ordination of the exercise.<br>
When we look at the squat, there are typically two main areas we look at:
1. The knee joint<br>
2. The hip joint<br>

When the squat is paused in a certain position, we can then measure the angle of the joints.<br>
**The hip angle** is formed by the position of the back and the thigh. **The knee angle** is formed by the thigh and the position of the lower leg.

### Angle Calculation:
<div align="center">
<img src= "https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/output/dig.png?raw=true" width="50%">
</div>

```
def calculate_angle(a,b,c):
    a = np.array(a) # First
    b = np.array(b) # Mid
    c = np.array(c) # End
    
    radians = np.arctan2(c[1]-b[1], c[0]-b[0]) - np.arctan2(a[1]-b[1], a[0]-b[0])
    angle = np.abs(radians*180.0/np.pi)
    
    if angle >180.0:
        angle = 360-angle
        
    return angle
 ```
 For Calculating angles we need Co-ordinates of **shoulder**, **hip**, **knee**, and **ankle**, which can be obtained from landmark points.
- The landmark model in MediaPipe Pose predicts the location of 33 pose landmarks (see figure below).
![points](https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/output/points.PNG?raw=true)

#### Get coordinates:
```
landmarks = results.pose_landmarks.landmark
shoulder = [landmarks[mp_pose.PoseLandmark.RIGHT_SHOULDER.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_SHOULDER.value].y]
hip = [landmarks[mp_pose.PoseLandmark.RIGHT_HIP.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_HIP.value].y]
knee = [landmarks[mp_pose.PoseLandmark.RIGHT_KNEE.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_KNEE.value].y]
ankle = [landmarks[mp_pose.PoseLandmark.RIGHT_ANKLE.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_ANKLE.value].y]
```
**Knee-joint angle**:
```
angle_knee = calculate_angle(hip, knee, ankle)
knee_angle = 180-angle_knee
```
**Hip-joint angle**:
```
angle_hip = calculate_angle(shoulder, hip, knee)
hip_angle = 180-angle_hip
```

Checkout the [Notebook](https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/Squat%20pose%20estimation.ipynb) for complete implementation.<br>

<div align="center">
<img src= "https://github.com/Pradnya1208/Squats-angle-detection-using-OpenCV-and-mediapipe_v1/blob/main/output/output1.gif?raw=true">
</div>
            




## Lessons Learned
`Computer Vision`
`Pose Detection`
`Pose Estimation`
`Landmark angle calculation`
`Mediapipe`

## References:
- [Mediapipe](https://google.github.io/mediapipe/solutions/pose)
- [BioChemical Analysis of Squat](https://stuartbauld.wixsite.com/performance/single-post/2018/02/27/biomechanical-analysis-of-squat-and-deadlift-1)
- [The real science of the squat](https://squatuniversity.com/2016/04/20/the-real-science-of-the-squat/)

### Feedback

If you have any feedback, please reach out at pradnyapatil671@gmail.com



[1]: https://github.com/Pradnya1208
[2]: https://www.linkedin.com/in/pradnya-patil-b049161ba/
[3]: https://public.tableau.com/app/profile/pradnya.patil3254#!/
[4]: https://twitter.com/Pradnya1208


[![github](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c292abd3f9cc647a7edc0061193f1523e9c05e1f/icons/git.svg)][1]
[![linkedin](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/9f5c4a255972275ced549ea6e34ef35019166944/icons/iconmonstr-linkedin-5.svg)][2]
[![tableau](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/e257c5d6cf02f13072429935b0828525c601414f/icons/icons8-tableau-software%20(1).svg)][3]
[![twitter](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c9f9c5dc4e24eff0143b3056708d24650cbccdde/icons/iconmonstr-twitter-5.svg)][4]



                                                                                                                                  









