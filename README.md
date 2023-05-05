Download Link: https://assignmentchef.com/product/solved-cv1-lab-3-harris-corner-detector-and-optical-flow
<br>



<h1><a name="_Toc12198"></a>1             Harris Corner Detector</h1>

In this section, a derivation of the Harris Corner Detector [1] is presented.

Given a shift (∆<em>x,</em>∆<em>y</em>) at a point (<em>x,y</em>), the auto-correlation function is defined as:

<em>c</em>(∆<em>x,</em>∆<em>y</em>) =           <sup>X              </sup><em>w</em>(<em>x,y</em>)(<em>I</em>(<em>x </em>+ ∆<em>x,y </em>+ ∆<em>y</em>) − <em>I</em>(<em>x,y</em>))<sup>2 </sup><em>,                     </em>(1)

(<em>x,y</em>)∈<em>W</em>(<em>x,y</em>)

where <em>W</em>(<em>x,y</em>) is a window centered at point (<em>x,y</em>) and <em>w</em>(<em>x,y</em>) is a Gaussian function. For simplicity, from now on,           <sup>P          </sup>will be referred to as <sup>P</sup>.

(<em>x,y</em>)∈<em>W</em>(<em>x,y</em>)                                                                                <em>W</em>

Approximating the shifted function by the first-order Taylor expansion we get:

(2)

(3)

where <em>I<sub>x </sub></em>and <em>I<sub>y </sub></em>are partial derivatives of <em>I</em>(<em>x,y</em>). The first gradients can be approximated by:

(4)

(5)

Note that using the kernel (−1<em>,</em>1) to approximate the gradients is also correct. The auto-correlation function can now be written as:

(6)

(7)

(8)

where <em>Q</em>(<em>x,y</em>) is given by:

The <em>cornerness H</em>(<em>x,y</em>) is defined by the two eigenvalues of <em>Q</em>(<em>x,y</em>), e.g. <em>λ</em><sub>1 </sub>and <em>λ</em><sub>2</sub>:

<em>H    </em>=      <em>λ</em><sub>1</sub><em>λ</em><sub>2 </sub>− 0<em>.</em>04(<em>λ</em><sub>1 </sub>+ <em>λ</em><sub>2</sub>)<sup>2                                                                                                 </sup>(12)

=    <em>det</em>(<em>Q</em>) − 0<em>.</em>04(<em>trace</em>(<em>Q</em>))<sup>2                                                                                   </sup>(13)

=       (<em>AC </em>− <em>B</em><sup>2</sup>) − 0<em>.</em>04(<em>A </em>+ <em>C</em>)<sup>2</sup><em>.                                       </em>(14)

In this section, you are going to implement Equation 12 to calculate <em>H </em>and use it to detect the corners in an image.

<h3>Hint</h3>

For that purpose, you need to compute the elements of <strong>Q</strong>, i.e. <em>A</em>, <em>B </em>and <em>C</em>. To do that, you need to calculate <em>I<sub>x</sub></em>, which is the smoothed derivative of the image. That can be obtained by convolving the first order Gaussian derivative, <em>G<sub>d</sub></em>, with the image <em>I </em>along the x-direction. Then, <strong>A </strong>can be obtained by squaring <em>I<sub>x</sub></em>, and then convolving it with a Gaussian, <em>G</em>. Similarly, <strong>B </strong>and <strong>C </strong>can be obtained. For example, to get <strong>C</strong>, you need to convolve the image with <em>G<sub>d </sub></em>along the y-direction (to obtain <em>I<sub>y</sub></em>), raise it to the square, then convolve it with <em>G</em>.

<h3>Hint</h3>

The corner points are the local maxima of <strong>H</strong>. Therefore, you should check for every point in <em>H</em>, (1) if it is greater than all its neighbours (in an <em>n </em>× <em>n </em>window centered around this point) and (2) if it is greater than the userdefined threshold. If both conditions are met, then the point is labeled as a corner point.

<h3>Question – 1 (35-<em>pts</em>)</h3>

<ol>

 <li>Create a function to implement the Harris Corner Detector. Your function should return matrix <em>H</em>, the indices of rows of the detected corner points <strong>r</strong>, and the indices of columns of those points <strong>c</strong>, where the first corner is given by (<em>r</em>[0]<em>,c</em>[0]). Name your script as <strong>harris corner detector.py</strong>.</li>

 <li>Implement another function that plots three figures: The computed image derivatives <em>I<sub>x </sub></em>and <em>I<sub>y</sub></em>, and the original image with the corner points plotted on it. Show your results on example images <strong>toy/0001.jpg </strong>and <strong>doll/0200.jpeg </strong>in your report separately. Remember to experiment with different threshold values to see the impact on which corners are found.</li>

 <li>Is the algorithm rotation-invariant? How about your implementation? Rotate <strong>toy/0001.jpg </strong>image 45 and 90 degrees and run the Harris Corner Detector algorithm on the rotated images. Explain your answer and support it with your observations.</li>

</ol>

<em>Note: </em>You are allowed to use scipy.signal.convolve2d to perform convolution, and scipy.ndimage.gaussian filter to obtain your image derivatives. Include a demo function to run your code.

<h3>Question – 2 (10-<em>pts</em>)</h3>

Now you have seen the cornerness definition of Harris on Equation 12. Another relevant definition of cornerness is defined by Shi and Tomasi [2], after the original definition of Harris. Check their algorithm and answer the following questions:

<ol>

 <li>How do they define cornerness? Write down their definition using the notations of Equation 12.</li>

 <li>Does the Shi-Tomasi Corner Detector satisfy the following properties: translation invariance, rotation invariance, scale invariance? Explain your reasoning.</li>

 <li>In the following scenarios, what could be the relative cornerness values assigned by Shi and Tomasi? Explain your reasoning.

  <ul>

   <li>Both eigenvalues are near 0.</li>

   <li>One eigenvalue is big and the other is near zero.</li>

   <li>Both eigenvalues are big.</li>

  </ul></li>

</ol>

<h1><a name="_Toc12199"></a>2             Optical Flow – Lucas-Kanade Algorithm</h1>

Optical flow is the apparent motion of image pixels or regions from one frame to the next, which results from moving objects in the image or from camera motion. Underlying optical flow is typically an assumption of <em>brightness constancy</em>. That is the image values (brightness, color, etc) remain constant over time, though their 2D position in the image may change. Algorithms for estimating optical flow exploit this assumption in various ways to compute a velocity field that describes the horizontal and vertical motion of every pixel in the image. For a 2D+t dimensional case a voxel at location (<em>x,y,t</em>) with intensity <em>I</em>(<em>x,y,t</em>) will have moved by <em>δ<sub>x</sub></em>, <em>δ<sub>y </sub></em>and <em>δ<sub>t </sub></em>between the two image frames, and the following image constraint equation can be given:

<em>I</em>(<em>x,y,t</em>) = <em>I</em>(<em>x </em>+ <em>δ<sub>x</sub>,y </em>+ <em>δ<sub>y</sub>,t </em>+ <em>δ<sub>t</sub></em>)<em>.                                          </em>(15)

Assuming the movement to be small, the image constraint at I(x, y, t) can be extended using Taylor series, truncated to first-order terms:

(16)

Since we assume changes in the image can purely be attributed to movement, we will get:

= 0                                            (17)

or

(18)

where <em>V<sub>x </sub></em>and <em>V<sub>y </sub></em>are the <em>x </em>and <em>y </em>components of the velocity or optical flow of <em>I</em>(<em>x,y,t</em>). Further, <em>I<sub>x</sub></em>, <em>I<sub>y </sub></em>and <em>I<sub>t </sub></em>are the derivatives of the image at (<em>x,y,t</em>) in the corresponding directions, which defines the main equation of optical flow.

Optical flow is difficult to compute for two main reasons. First, in image regions that are roughly homogeneous, the optical flow is ambiguous, because the brightness constancy assumption is satisfied by many different motions. Second, in real scenes, the assumption is violated at motion boundaries and by miscellaneous lighting, non-rigid motions, shadows, transparency, reflections, etc. To address the former, all optical flow methods make some sort of assumption about the spatial variation of the optical flow that is used to resolve the ambiguity. Those are just assumptions about the world which are approximate and consequently may lead to errors in the flow estimates. The latter problem can be addressed by making much richer but more complicated assumptions about the changing image brightness or, more commonly, using robust statistical methods which can deal with ’violations’ of the brightness constancy assumption.

<strong>Lucas-Kanade Algorithm</strong>

We will be implementing the Lucas-Kanade method [3] for Optical Flow estimation. This method assumes that the optical flow is essentially constant in a local neighborhood of the pixel under consideration. Therefore, the main equation of the optical flow can be assumed to hold for all pixels within a window centered at the pixel under consideration. Let’s consider pixel <em>p</em>. Then, for all pixels around <em>p</em>, the local image flow vector (<em>V<sub>x</sub>,V<sub>y</sub></em>) must satisfy:

<em>I<sub>x</sub></em>(<em>q</em><sub>1</sub>)<em>V<sub>x </sub></em>+ <em>I<sub>y</sub></em>(<em>q</em><sub>1</sub>)<em>V<sub>y </sub></em>= −<em>I<sub>t</sub></em>(<em>q</em><sub>1</sub>)

<em>I<sub>x</sub></em>(<em>q</em><sub>2</sub>)<em>V<sub>x </sub></em>+ <em>I<sub>y</sub></em>(<em>q</em><sub>2</sub>)<em>V<sub>y </sub></em>= −<em>I<sub>t</sub></em>(<em>q</em><sub>2</sub>)

…                                                                 (19)

<em>I<sub>x</sub></em>(<em>q<sub>n</sub></em>)<em>V<sub>x </sub></em>+ <em>I<sub>y</sub></em>(<em>q<sub>n</sub></em>)<em>V<sub>y </sub></em>= −<em>I<sub>t</sub></em>(<em>q<sub>n</sub></em>)<em>,</em>

where <em>q</em><sub>1</sub>, <em>q</em><sub>2</sub>, …<em>q<sub>n </sub></em>are the pixels inside the window around <em>p</em>. <em>I<sub>x</sub></em>(<em>q<sub>i</sub></em>), <em>I<sub>y</sub></em>(<em>q<sub>i</sub></em>), <em>I<sub>t</sub></em>(<em>q<sub>i</sub></em>) are the partial derivatives of the image <em>I </em>with respect to position <em>x</em>, <em>y </em>and time <em>t</em>, evaluated at the point <em>q<sub>i </sub></em>and at the current time.

These equations can be written in matrix to form <em>Av </em>= <em>b</em>, where

<em> , </em>and<em> .                      </em>(20)

This system has more equations than unknowns and thus it is usually over-determined. The Lucas-Kanade method obtains a compromise solution by the weighted-leastsquares principle. Namely, it solves the 2 × 2 system as

<em>A<sup>T </sup>Av </em>= <em>A<sup>T </sup>b                                                        </em>(21)

or

<em>v </em>= (<em>A<sup>T </sup>A</em>)<sup>−1</sup><em>A<sup>T </sup>b.                                                     </em>(22)

<h3>Question – 1 (30-pts)</h3>

For this assignment, you will be given two pairs of images: <em>Car1.jpg</em>, <em>Car2.jpg</em>; and <em>Coke1.jpg</em>, <em>Coke2.jpg</em>. You should estimate the optical flow between these two pairs. That is, you will get optical flow for sphere images, and for synth images separately. Implement the Lucas-Kanade algorithm using the following steps. Name your script <strong>lucas kanade.py</strong>.

<ol>

 <li>Divide input images on non-overlapping regions, each region being 15× 15.</li>

 <li>For each region compute <em>A</em>, <em>A<sup>T </sup></em>and <em>b</em>. Then, estimate optical flow as given in Equation 22.</li>

 <li>When you have estimation for optical flow (<em>V<sub>x</sub>,V<sub>y</sub></em>) of each region, you should display the results. There is a <strong>matplotlib </strong>function quiver which plots a set of two-dimensional vectors as arrows on the screen. Try to figure out how to use this to show your optical flow results.</li>

</ol>

<em>Note: </em>You are allowed to use scipy.signal.convolve2d to perform convolution.

Include a demo function to run your code.

<h3>Hint</h3>

You can use regions that are 15 × 15 pixels that are non-overlapping. That is, if input images are 256 × 256, you should have an array of 17 × 17 optical flow vectors at the end of your procedure. As we consider 15 × 15 regions, your matrix <strong>A </strong>will have the following size 225 × 2, and the vector <strong>b </strong>will be 225 × 1.

<h3>Hint</h3>

Carefully read the documentation of <strong>matplotlib</strong>’s quiver. By default, the angles of the arrows are 45 degrees counter-clockwise from the horizontal axis. This means your arrows might point in the wrong direction! Also, play around with the arrow scaling.

<h3>Question – 2 (5-<em>pts</em>)</h3>

Now you have seen one of the optical flow estimation methods developed by Lucas and Kanade. There are several more methods in the literature. The Horn-Schunck method [4] is one of them. Check their method, compare it to Lucas-Kanade and answer the following questions:

<ol>

 <li>At what scale do the algorithms operate; i.e local or global? Explain your answer.</li>

 <li>How do the algorithms behave at flat regions?</li>

</ol>

<h1><a name="_Toc12200"></a>3             Feature Tracking</h1>

In this part of the assignment, you will implement a simple feature-tracking algorithm. The aim is to extract visual features, like corners, and track them over multiple frames.

<h3>Question – 1 (18-pts)</h3>

<ol>

 <li>Implement a simple feature-tracking algorithm by following below steps. Name your script <strong>py</strong>.

  <ul>

   <li>Locate feature points on the first frame by using the Harris Corner Detector, that you implemented in section 1.</li>

   <li>Track these points using the Lucas-Kanade algorithm for optical flow estimation, that you implemented in section 2.</li>

  </ul></li>

 <li>Prepare a video for each sample image sequences. These videos should visualize the initial feature points and the optical flow. Test your implementation and prepare visualization videos for <em>doll </em>and <em>toy </em></li>

</ol>

Include a demo function to run your code.

<h3>Question – 2 (2-pts)</h3>

Why do we need feature tracking even though we can detect features for each and every frame?

<strong>Tips: </strong>required Python packages: Pillow, Numpy, Scipy, Matplotlib, Opencv. To install Opencv in an anaconda environment, this command is highly recommended: “conda install -c menpo opencv”.

<h2>References</h2>

<ul>

 <li>G. Harris and M. Stephens, “A combined corner and edge detector.,” in <em>Alvey vision conference</em>, vol. 15, pp. 10–5244, Citeseer, 1988.</li>

 <li>Jianbo and C. Tomasi, “Good features to track,” in <em>IEEE Computer Society Conference on Computer Vision and Pattern Recognition</em>, pp. 593–600, 1994.</li>

 <li>D. Lucas and T. Kanade, “An iterative image registration technique with an application to stereo vision,” 1981.</li>

 <li>K. Horn and B. G. Schunck, “Determining optical flow,” in <em>Techniques and Applications of Image Understanding</em>, vol. 281, pp. 319–331, International Society for Optics and Photonics, 1981.</li>

</ul>