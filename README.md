# CS-440-Programming-Assignment-2
Web Report and Code for CS 440 Programming Assignment 2: Recognizing Hand Gestures Using Computer Vision

Name: Sonya McCree
Teammate: Yuxuan (Lorraine) Ji
Date: 2 April 2018

# Problem Definition
For this assignment, we were asked to create a program that uses a variety of computer vision techniques (using C++ and opencv)
to recognize hand gestures. The techniques we could have chosen to implement were:
(1) Background differencing
(2) Frame-to-Frame differencing
(3) Template Matching
(4) Motion Energy Templates
(5) Skin-color detection
(6) Horizontal and Vertical Projection
(7) Size, Position, Orientation of "blobs"
(8) Circularity of "blobs"
(9) Tracking the position and orientation of moving objects

Using any combination of this techniques, we would be able to recognize 2 hand gestures: an open hand and a closed fist,
as well as an open-handed wave. From the start, we anticipated difficulties with differentiating background noise and similarly
shaped figures (such as our heads) from the object we were trying to classify. We also knew that it could be difficult to read
and interpret the data of a continuous stream of images because of system lag. By the end of our experiments with the techniques
listed above, however, we were able to come up with a relatively accurate system for recognizing fists, open hands and waves, using
technique 8 as our main classification method.

# Method and Implementation
Attempt 1: Template Matching

When beginning the assignment, we initially wanted to use Template Matching to classify our hand shapes. We modified a function we
obtained from OpenCV's tutorial on the subject and called it "myTemplateMatching(int, void*)." The function takes a source image (img)
and compares it to a template image (templ). We had two template images: templ1 (a fist) and templ2 (open hand) that we would choose
between for different tests. The template matching algorithm was moderately successful at locating the images in the source frame, but
was very inaccurate. Most times it would just latch onto the figure in the frame that most resembled the shape of the template, regardless
of whether we held up our hand or not. Because of these inconsistencies, we decided to switch our approach to focus on analyzing the 
circularity of the hand image in the frame and our template images.

Attempt 2: Circularity

After getting inaccurate results using the template matching method, we moved on to calculating the gestures' circularities in order
to classify the shapes. We read into the program two template images, same as before: one of a fist and one of an open hand. We got these images by taking screenshots of our fist/hand in front of the webcam after skin color detection and background differencing operations had been performed on the frame image. We created two functions: void classifyHandShape() and double computeCircularity(). Compute circularity uses the following formula to find the circularity of an image:

circularity = Emin / Emax

where Emin = ((a + c) / 2) - ((a - c)^2 / 2h) - (b^2 / 2h) and Emax = ((a + c) / 2) + ((a - c)^2 / 2h) + (b^2 / 2h)

a = M_2,0 b = 2 * (M_1,1) c = M_0,2 and h = sqrt((a-c)^2 + b^2)

According to the formula, the shape is more likely to be circular if the circularity is closer to 1, whereas it is more likely to be linear if the circularity is closer to 0. We found that the open hand template image we used had a higher circularity than the fist, so
we determined that the shape "blob" in the source image would be more likely to be a hand if its circularity was closer to 1, and more likely to be a fist if its circularity was closer to 0.

The way we programmed this was by using classifyHandShape. That function the calls computeCircularity to get the circularities of the source, template 1 and template 2. We created two variables called diff_hand and diff_fist where we took the absolute value of the difference between the source image's circularity and the templates' circularity. If diff_fist was less than diff_hand, we classified the 'blob' as a fist; if diff_fist was greater than diff_hand, we classified the 'blob' as an open hand. We then stored the result in a global variable called isFist which we set to either true or false as a result of the classification. classifyHandShape prints a message on-screen that says either "This is a hand" or "This is a fist."

To classify our dynamic gesture--an open-handed wave--we modified the myMotionEnergy function. We created an if-else statement that reads the speed at which the 'movement blob' moves across the screen. If it moves at a rate of 30000 ms or greater and we had already determined that the shape in the source image is an open hand, we print a message on the myMotionEnergy screen that says "Wave" in script. The problem that we initially had with this approach was that any fast motion was being classified as a wave. However, using the isFist variable to determine the hand gesture before classifying the dynamic gesture solved this problem.

Using a mix of circularity computation, motion energy reading, template images skin color detection, among other techniques listed above, we were able to classify these gestures with moderate success.
























