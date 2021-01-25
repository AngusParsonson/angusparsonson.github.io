---
title: "Dartboard (in image) Detection Algorithm - Computer Vision"
excerpt: "In collaboration with a peer, we created a piece of software to detect dartboards in an image. The techniques used included Sobel and Canny edge detection, Viola Jones object detection and our own custom implementations of the Circle Hough transform, the regular Hough transform and a weighted mean clustering algorithm. Completed using C++. <br/><img src='/images/projects/dartboard_detector/detected.jpg' width=400>" 
collection: projects
---

<div style="text-align: justify">
In collaboration with a peer, we created a piece of software to detect dartboards in an image. The techniques used included Sobel and Canny edge detection, Viola Jones object detection and our own custom implementations of the Circle Hough transform, the regular Hough transform and a weighted mean clustering algorithm. Completed using C++.
</div>

## Algorithm Overview
![image](/images/projects/dartboard_detector/algorithm.png)

- The whole algorithm consits of a multitude of detection steps, in the following description these will be refurred to as the *detectors*.
- The extra Canny step (line thinning and hysteresis thresholding to promote lines running in the same direction) was added to give a more refined input to our detectors which resulted in more accurate Hough spaces. 
- The first detector is a combination of Viola-Jones and circle detection. After the detectors make their predictions, IOUs are calculated between the two sets of boards and thresholded. This detector gave the highest F1-score. If the IOU passed the threshold, the circle board is passed into the next stage.
- The rejects are then separated into two streams, Viola-Jones and Circles.
- Line detection and thresholding is performed on the Viola-Jones boards, with successful boards making it to the next step. This detector had low F1-score so the threshold was set high.
- The Circle stream was first clustered using our own weighted mean clustering algorithm as many similar circles were often detected, then the same line detection was performed but with a lower threshold. Our clustering algorithm worked by looping through rectangles and calculating their IOU with the other rectangles until a match (IOU above a certain threshold was obtained). We then took the average rectangle of the two in question, stored this in the second rectangle vector position and increased the weight attribute by the previous weight of the two rectangles used to calculate the new rectangle. This method clustered similar circles together with averages, but insured areas with high circle density weren’t given the same prevalence as the areas with low circle density.
- The successful boards were also clustered at the end using our weighted mean algorithm but with a lower threshold IOU. This meant that if our last boards were overlapping by even a small amount it was considered to be the same board (this saved us a lot of false positives whilst not decreasing our TPR).

<img src='/images/projects/dartboard_detector/demo.jpg' width=700>

## Idea

We had access to three basic detection methods: Viola-Jones, Line detection and Circle detection. These were capable of very high TPR scores by themselves, however they fell short in terms of F1. In response to this we combined these detectors to increase F1 but this then led to a decrease in TPR. By joining the results of the three combination detectors we were able to achieve significant improvements over the basic methods.

Detectors with higher F1 scores and lower TPR scores, such as the circle detector, could afford to have higher thresholds than those with lower F1 scores, and were ultimately the most useful.

The line detector was understandably the least accurate as lines could be registered through most images. In response to this we decided not to run this detector on its own but instead to only run it on boards which other detectors had predicted. This meant our line detector acted more as a validity check than a true detector.

Our line detector took the edge direction into account which meant that the hough space was only incremented in the relevant places. This meant that noise levels throughout were reduced and we could lower thresholds, meaning increased TPR with the same F1.

Thresholds were calculated depending on the area of the image being processed. This was necessary as images with more pixels would generally have higher Hough scores. A linear constant was still added to every threshold calculation as smaller images tended to become overexposed when adjusting.

By tweaking Viola-Jones and Canny parameters, we were able to hand the detectors more useful inputs.

### Scoring Methods

In order to test the accuracy of our detection algorithm, three scoring methods were implemented:
- Intersection Over Union (IOU): finds the ratio of how much two areas overlap.
- True Positive Rate (TPR): finds the percentage of ground truths which were detected.
- F1-Score: a measure of how accurate the detector is when it predicts an object. It accounts for false positives and is calculated using the following formula:

![F1 Equation](https://latex.codecogs.com/gif.latex?\frac{2&space;*&space;precision&space;*&space;recall}{precision&space;&plus;&space;recall})

In order to calculate the TPR for an attempt, we must be able to assess if detections sufficiently match with the ground truths. Our approach to this was to match each result to the corresponding ground truth which gives the highest IOU. 
Care had to be taken as one result may match another’s ground truth, but it could only be paired with one. Deciding how to pair the results was difficult.

TPR does not always give a meaningful result because a 100% score can always be achieved by simply detecting results at every possible area in the image, meaning every ground truth will have a match. F1-Score can be used to make up for this deficiency by scoring the rate of successful predictions.

## Training a New Cascade

To detect dart boards instead of faces a new Viola-Jones cascade was needed. To create the samples needed to train this cascade, opencv\_createsamples fed a target image, which cloned the image with slight variance. Next, opencv\_traincascade was fed the training data and after three stages produced the required cascade.

![Cascade Training](/images/projects/dartboard_detector/cascade.png)

