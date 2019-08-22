# HOUGH TRANSFORM


## INTRODUCTION

The Hough transform is a feature extraction technique used in image analysis, computer vision, and digital image processing. The purpose of the technique is to find imperfect instances of objects within a certain class of shapes by a ***voting procedure***. This voting procedure is carried out in a parameter space, from which object candidates are obtained as local maxima in a so-called accumulator space that is explicitly constructed by the algorithm for computing the Hough transform.

The classical Hough transform was concerned with the identification of lines in the image, but later the Hough transform has been extended to identifying positions of ***arbitrary shapes***, most ***commonly circles or ellipses***. The Hough transform as it is universally used today was invented by ***Richard Duda and Peter Hart in 1972***, who called it a ***"generalized Hough transform"*** after the related 1962 patent of Paul Hough.

Line Detection             |  Circle Detection
:-------------------------:|:-------------------------:
![](assets/README/1.jpg)   |  ![](assets/README/2.jpg)
<br>


## LINE DETECTION

### Representation of Lines in the Hough Space

Lines can be represented uniquely by two parameters. Often the form in Equation 1 is used with parameters a and b:

![](assets/README/3.png)

This form is, however, not able to represent vertical lines. Therefore, the Hough transform uses the form in Equation 2, which can be rewritten to Equation 3 to be similar to Equation 1. The parameters θ and r (here r and ρ are interchangable and I will use them so ) is the angle of the line and the distance from the line to the origin respectively.

![](assets/README/4.png)

All lines can be represented in this form when θ ∈ [0, 180] and r ∈ R (or θ ∈ [0, 360] and r ≥ 0). The Hough space for lines has therefore these two dimensions; θ and r, and a line is represented by a single point, corresponding to a unique set of parameters (θ0, r0). The line to-point mapping is illustrated in Figure 1.

![](assets/README/5.png)

So the the polar form of a line is represented as:

![](assets/README/8.jpg)



### Mapping of Points to Hough Space

An important concept for the Hough transform is the mapping of single points. The idea is, that a point is mapped to all lines, that can pass through that point. This yields a sine-like line in the Hough space. The principle is illustrated for a point p0 = (40, 50) in Figure 2.

![](assets/README/6.png)


## ALGORITHM

The algorithm for detecting straight lines can be divided into the following steps:

* Edge detection, e.g. using the Canny edge detector.

* Mapping of edge points to the Hough space and store in an accumulator.

* Interpretation of the accumulator to yield lines of infinite length. The interpretation is done by thresholding and possibly other constraints.

* Conversion of infinite lines to finite lines.

The finite lines can then be superimposed back on the original image. The Hough transform itself is performed in point 2, but all steps except edge detection is covered in this project.

### TRANSFORMATION TO HOUGH SPACE

The Hough transform takes a binary edge map as input and attempts to locate edges placed as straight lines. The idea of the Hough transform is, that ***every edge point in the edge map is transformed to all possible lines that could pass through that point***. Figure 2 illustrates this for a single point, and Figure 3 illustrates this for two points.

![](assets/README/7.png)

A typical edge map includes many points, but the principle for line detection is the same as illustrated in Figure 3 for two points. Each edge point is transformed to a line in the Hough space, and the areas where most Hough space lines intersect is interpreted as true lines in the edge map.

####  THE HOUGH SPACE ACCUMULATOR

When we say that a line in 2D space is parameterized by ρ and θ, it means that if we any pick a (ρ, θ), it corresponds to a line.

Imagine a 2D array where the x-axis has all possible θ values and the y-axis has all possible ρ values. Any bin in this 2D array corresponds to one line.

![](assets/README/9.png)

This 2D array is called an ***accumulator*** because we will use the bins of this array to collect evidence about which lines exist in the image. The top left cell corresponds to a (-R, 0) and the bottom right corresponds to (R, pi).

We will see in a moment that the value inside the bin (ρ, θ) will increase as more evidence is gathered about the presence of a line with parameters ρ and θ.

To determine the areas where most Hough space lines intersect, an ***accumulator covering the Hough space is used***. When an edge point is transformed, bins in the accumulator is incremented for all lines that could pass through that point. The resolution of the accumulator determines the precision with which lines can be detected.

The following steps are performed to detect lines in an image.

***Initialize Accumulator:*** The number of cells you choose to have is a design decision. Let’s say you chose a 10×10 accumulator. It means that \rho can take only 10 distinct values and the \theta can take 10 distinct values, and therefore you will be able to detect 100 different kinds of lines. The size of the accumulator will also depend on the resolution of the image. But if you are just starting, don’t worry about getting it perfectly right. Pick a number like 20×20 and see what results you get.


## DETECTION OF INFINITE LINES

For every edge pixel (x, y) in the above array, we vary the values of θ from 0 to pi and plug it in equation 2 to obtain a value for ρ.

In the Figure below we vary the θ for three pixels ( represented by the three colored curves ), and obtain the values for ρ using equation 2.

As you can see, these curves intersect at a point indicating that a line with parameters θ = 1 and ρ =  9.5 is passing through them.

![](assets/README/10.png)

Typically, we have hundreds of edge pixels and the accumulator is used to find the intersection of all the curves generated by the edge pixels.

Let’s see how this is done.



























