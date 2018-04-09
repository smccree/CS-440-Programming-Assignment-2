# Web Page Report for CS440 Programming Assignment 1: Recognizing Hand Gestures Using Computer Vision

Name: Sonya McCree

Teammate: Yuxuan (Lorraine) Ji

Date: 2 April 2018

Note: I had trouble getting screenshots to show up in this report. All screenshots referenced are available in the screenshots folder.
Github repository link: https://github.com/smccree/CS-440-Programming-Assignment-2/edit/master/README.md

# Problem Definition
For this assignment, we were asked to create a program that uses a variety of computer vision techniques (using C++ and opencv)
to recognize hand gestures. The techniques we could have chosen to implement were:
1. Background differencing
2. Frame-to-Frame differencing
3. Template Matching
4. Motion Energy Templates
5. Skin-color detection
6. Horizontal and Vertical Projection
7. Size, Position, Orientation of "blobs"
8. Circularity of "blobs"
9. Tracking the position and orientation of moving objects

Using any combination of this techniques, we would be able to recognize 2 hand gestures: an open hand and a closed fist,
as well as an open-handed wave. From the start, we anticipated difficulties with differentiating background noise and similarly
shaped figures (such as our heads) from the object we were trying to classify. We also knew that it could be difficult to read
and interpret the data of a continuous stream of images because of system lag. By the end of our experiments with the techniques
listed above, however, we were able to come up with a relatively accurate system for recognizing fists, open hands and waves, using
technique 8 as our main classification method.

# Method and Implementation
Attempt 1: Template Matching

When beginning the assignment, we initially wanted to use Template Matching to classify our hand shapes. We modified a function we
obtained from OpenCV's tutorial on the subject and called it myTemplateMatching(). The function takes a source image (img)
and compares it to a template image (templ). We had two template images: templ1 (a fist) and templ2 (open hand) that we would choose
between for different tests. The template matching algorithm was moderately successful at locating the images in the source frame, but
was very inaccurate. Most times it would just latch onto the figure in the frame that most resembled the shape of the template, regardless
of whether we held up our hand or not. Because of these inconsistencies, we decided to switch our approach to focus on analyzing the 
circularity of the hand image in the frame and our template images.

Attempt 2: Circularity

After getting inaccurate results using the template matching method, we moved on to calculating the gestures' circularities in order
to classify the shapes. We read into the program two template images, same as before: one of a fist and one of an open hand. We got these images by taking screenshots of our fist/hand in front of the webcam after skin color detection and background differencing operations had been performed on the frame image. We created two functions: classifyHandShape() and computeCircularity(). Compute circularity uses the following formula to find the circularity of an image:

circularity = Emin / Emax

where Emin = ((a + c) / 2) - ((a - c)^2 / 2h) - (b^2 / 2h) and Emax = ((a + c) / 2) + ((a - c)^2 / 2h) + (b^2 / 2h)

a = M_2,0 b = 2 * (M_1,1) c = M_0,2 and h = sqrt((a-c)^2 + b^2)

According to the formula, the shape is more likely to be circular if the circularity is closer to 1, whereas it is more likely to be linear if the circularity is closer to 0. We found that the open hand template image we used had a higher circularity than the fist, so
we determined that the shape "blob" in the source image would be more likely to be a hand if its circularity was closer to 1, and more likely to be a fist if its circularity was closer to 0.

The way we programmed this was by using classifyHandShape. That function calls computeCircularity to get the circularities of the source, template 1 and template 2. We created two variables called diff_hand and diff_fist where we took the absolute value of the difference between the source image's circularity and the templates' circularity. If diff_fist was less than diff_hand, we classified the 'blob' as a fist; if diff_fist was greater than diff_hand, we classified the 'blob' as an open hand. We then stored the result in a global variable called isFist which we set to either true or false as a result of the classification. classifyHandShape prints a message on-screen that says either "This is a hand" or "This is a fist."

To classify our dynamic gesture--an open-handed wave--we modified the myMotionEnergy function. We created an if-else statement that reads the speed at which the 'movement blob' moves across the screen. If it moves at a rate of 30000 ms or greater and we had already determined that the shape in the source image is an open hand, we print a message on the myMotionEnergy screen that says "Wave" in script. The problem that we initially had with this approach was that any fast motion was being classified as a wave. However, using the isFist variable to determine the hand gesture before classifying the dynamic gesture solved this problem.

Using a mix of circularity computation, motion energy reading, template images skin color detection, among other techniques listed above, we were able to classify these gestures with moderate success.


# Experiments

We performed six initial tests to classify a static fist gesture using the circularity method outlined under "Attempt 2" above. For each experiment, we attempted to keep ourselves and other background items out of view so as not to skew our results or create erroneous classifications. We evaluated the results of our experiments by examining their overall accuracy--i.e., the number of correct classifications versus incorrect ones. We repeated the same testing process for static open-hand and dynamic wave. We also tested to make sure that the program will not classify a close-fisted wave as an open-handed wave. 

After our initial tests, we performed 20 rapid-fire tests for both fist and open hand recognition. For waving, we performed 24 additional rapid-fire tests. We did this to ensure a more a better data sample for our confusion matrices evaluating our results.

# Results

Task 1: Testing Recognition of a Closed Fist

![Trial 1 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_fist_source.PNG)      
![Trial 1 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_fist_circularity.PNG)

![Trial 2 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_fist_source.PNG)     
![Trial 2 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_fist_circularity.PNG)   

![Trial 3 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_fist_source.PNG)       
![Trial 3 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_fist_circularity.PNG)

![Trial 4 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_fist_source.PNG)      
![Trial 4 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_fist_circularity.PNG)

![Trial 5 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_fist_source.PNG)         
![Trial 5 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_fist_circularity.PNG)

![Trial 6 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_fist_source.PNG)   
![Trial 6 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_fist_circularity.PNG)



Task 2: Testing Recognition of an Open Hand

![Template Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/hand.PNG)


![Trial 1 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_hand_source.PNG)           ![Trial 1 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_hand_circularity.PNG)

![Trial 2 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_hand_source.PNG)           ![Trial 2 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_hand_circularity.PNG)        
![Trial 3 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_hand_source.PNG)           ![Trial 3 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_hand_circularity.PNG)

![Trial 4 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_hand_source.PNG)           
![Trial 4 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_hand_circularity.PNG)

![Trial 5 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_hand_sourch.PNG)           ![Trial 5 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_hand_circularity.PNG)

![Trial 6 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_hand_source.PNG)           ![Trial 6 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_hand_circularity.PNG)



Task 3: Testing Recognition of an Open-Handed Wave


![Trial 1 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_waving_source.PNG)         ![Trial 1 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_waving_display.PNG)

![Trial 2 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_waving_source.PNG)       
![Trial 2 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_waving_display.PNG)

![Trial 3 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_waving_source.PNG)         ![Trial 3 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_waving_display.PNG)

![Trial 4 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_waving_source.PNG)         ![Trial 4 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_waving_display.PNG)

![Trial 5 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_waving_source.PNG)         ![Trial 5 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_waving_display.PNG)

![Trial 6 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_waving_source.PNG)         ![Trial 6 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_waving_display.PNG)


![Confusion Matrix for Tasks 1 - 3](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/confusion%20matrix.png)

# Bonus: Template Matching Method Results

Because we initially tried to use template matching to classify hand gestures, I have included screenshots of the results from this
method. The only problem we had with using template matching is we were unable to figure out how to get the system to recognize when
the template we were searching for was not in the frame. Our modified version of the code available from OpenCV's tutorial on the subject simply found the 'best possible match' to the template we were searching for. However, it was still semi-accurate in our tests.

## Task 1: Testing Recognition of a Closed Fist


![Trial 1 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_fist_match.PNG)           ![Trial 1 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_fist_result.PNG)

![Trial 2 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_fist_match.PNG)           ![Trial 2 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_fist_result.PNG)        

![Trial 3 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_fist_match.PNG)           ![Trial 3 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_fist_result.PNG)

![Trial 4 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_fist_match.PNG)           ![Trial 4 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_fist_result.PNG)

![Trial 5 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_fist_match.PNG)           ![Trial 5 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_fist_result.PNG)

![Trial 6 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_fist_match.PNG)           ![Trial 6 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_fist_result.PNG)


Task 2: Testing Recognition of an Open Hand


![Trial 1 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_hand_match.PNG)           ![Trial 1 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T1_hand_result.PNG)

![Trial 2 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_hand_match.PNG)           ![Trial 2 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T2_hand_result.PNG)        

![Trial 3 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_hand_match.PNG)           ![Trial 3 Reuslt Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T3_hand_result.PNG)

![Trial 4 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_hand_match.PNG)           ![Trial 4 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T4_hand_result.PNG)

![Trial 5 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_hand_match.PNG)           ![Trial 5 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T5_hand_result.PNG)

![Trial 6 Source Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_hand_match.PNG)           ![Trial 6 Result Image](https://github.com/smccree/CS-440-Programming-Assignment-2/blob/master/screenshots/T6_hand_result.PNG)


# Discussion

As seen from the results of our tests, our circularity-computation-method worked with moderate accuracy. While it was generally successful, I would say the limitations were mainly in classifying open-handed gestures. Our method was more accurate in classifying fists, and in our tests for open-hands we had more instances of false-negative classifications. Also, since that part of our program was less accurate, we had difficulty with classifying waves. Sometimes we would be waving with an open hand and our program would not pick that up because it had classified our hand shape as a fist.

We expected template matching to be a better method of hand gesture recognition, but our method of implementation made circularity the more accurate method in our tests. We also expected the fist gesture template to have a higher circularity than our open hand gesture, but that was not the case. Also, we ran into the most problems when it came to classifying dynamic gestures. In the future, we would try to fine-tune that aspect of our program so that it is better at detecting wave-like motions in both directions. Also, we found that our program was better at classifying waves at higher speeds, so it was not as accurate at picking up slower wave motions.

As for potential future work, I believe that our template matching function has the most potential for improvement. At office hours, we learned that the reason why we were having problems with that method was because we fundamentally misunderstood the procedure for implementing it. We were attempting to match a single template image to an entire source image, which caused problems in both (1) differentiating open-hands from fists and (2) the system recognizing when there wasn't a hand in the image at all. However, we recently learned how to fix this problem. First, we should use the findBinaryLargeObjects() function from Lab 8 to find and cordon off the section of the source image that contains the hand gesture we are classifying. Then, we compare that cropped image with all our template images using the formula result = Sum(I_x,y - T_x,y). We then classify the hand gesture as the template with the smallest result value.

Despite this, we learned of our error well after we had switched focus toward computing circularity. I am confident that if we had the time to implement the template matching method correctly, we would have a more accurate system for recognizing hand gestures. In the future, I would like to adjust the program to reflect these changes.

# Conclusions
Computing circularity to classify hand gestures produced pretty accurate results for static gestures. It is yet to be seen whether our method is reproducable for hand gestures of all types. An interesting test would be to use a larger set of template images and see how our tests fare in response. Overall, I feel that circularity is a relatively simple but adept way of recognizing hand gestures. Also, its quick run-time makes it a relatively inexpensive operation to perform on the images. If we had more time to work on this project, I would have finished our template matching algorithm so that we could see which method yielded better results.

I am excited about the future of computer vision techniques in this area, especially for applications in American Sign Language. More advanced programs could potentially string together sentences and phrases signed in ASL, for use in either speech-to-type or similar technologies. As such, gesture recognition will continue to be a topic of interest for me in the future.


# Credits and Bibliography
Referenced "Template Matching" OpenCV Tutorial (2 April 2018) https://docs.opencv.org/2.4/doc/tutorials/imgproc/histograms/template_matching/template_matching.html
















