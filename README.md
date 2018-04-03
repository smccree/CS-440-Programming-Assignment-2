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
























