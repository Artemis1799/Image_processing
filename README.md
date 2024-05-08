This project is divided into two categories: the first is the algorithmic part, and the second is the web part, where we'll display the results and conclusions of the first part.


## Algorithmic Part:

The basic objective was for the computer to be able to process an image and recreate it. In other words, by giving it an image, it should be able to remake it using the data retrieved when it was shown the first image, but not only that. We want to be able to perform operations on the image, such as obtaining the image skeleton, storing the image data in a text file and many others.

To simplify our task, we're going to use black and white images, although this also works with grayscale images.

### Part One: The Squared Euclidean Distance Transform (SEDT)

First, we need to find a way for the computer to record the image data. Since we're working with black and white images, the data is simple: either a pixel is white, or it's black. If you tell me that all we have to do is put in a two-dimensional array at the pixel location, 0 if it's a black pixel, and 1 if it's a white pixel, you'd be right. However, we wanted to do more complex things later on that wouldn't have been possible with a simple array like this.

We also needed to record the distance between a pixel on the shape and a pixel on the background. Example: imagine an image with a black circle, we agree that the closer a pixel is to the edge, the smaller its distance from the image background (in white), and that if we take the pixel at the center of the circle, we have the pixel with the greatest distance from the background. We store all this information in a two-dimensional array that has the same length and width as the image, with 0 if the pixel is white (a background pixel) and X if it's a black pixel (a shape pixel) (X corresponds to the distance between the shape pixel and the background pixel).

To implement this algorithm, we'll use the squared Euclidean distance transform. The Euclidean distance squared map is a representation of an image in which each pixel is replaced by the Euclidean distance squared between that pixel and the nearest background pixel. This point can be the center of the image or a specific point. The squared Euclidean distance between two points (x1, y1) and (x2, y2) in two-dimensional space is calculated as follows:

Euclidean distance squared = (x2 - x1)^2 +(y2 - y1)^2

By replacing each pixel in the image with this value, we obtain a map where higher values represent areas farthest from the nearest background pixel, while lower values represent closer areas.

<div style="display: flex; justify-content: space-around;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/f72a727b-1a4d-4839-96d7-bc6f71840c71" style="height: 300px; margin-right: 20px;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/83cf186e-1332-422b-8226-7dbb08968acb" style="height: 300px;">
</div>


In our project, in order to observe differences in complexity and completion time, we created two functions to calculate the map of Euclidean distances to the square, one brute force and one optimized.

Once we had our Euclidean distances-to-squares chart, we set up a function to visualize the chart. Background pixels remain white, and shape pixels are displayed according to their distance; the further a pixel is from the background, the more its color will tend toward white, and the closer the pixel is, the more its color will tend toward black.

Example:

<div style="display: flex; justify-content: space-around;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/9b9fae70-5c28-4460-b5c3-3df117451340" style="height: 400px;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/048279a9-d2f4-451d-99e2-91920ed77fdb" style="height: 400px;">
</div>


### Part Two: Maximum Balls

To redraw the shape, we didn't want to simply run through the list and draw a white pixel when the value is 0 and a black pixel when the value is other than 0. We had to find another solution.

Looking at the squared Euclidean distance map and image representations, we noticed that, in reality, all shapes can be drawn with filled circles, whose radii are the square root of the pixel distance stored in the squared Euclidean distance map table. This is why we're going to use the maxiamale ball method.

This method consists in keeping only those balls that are maximal, i.e. that are not entirely contained within another ball. For if a ball is non-maximal, this means that another ball or set of balls can draw the same surface as it, and so, for the sake of optimization, we're going to keep only the maximal balls to display only the circles that are really useful for reconstructing the image.

To find out whether an X-ball is maximum, we review all the other balls in the table, and if the distance between the centre of the X-ball and the centre of the ball we have reviewed, plus the radius of the smallest circle, is smaller than the radius of the largest circle, then it is included in another ball and is therefore not maximum. If, after testing all the balls, ball X is still not declared "non-maximal", then it is maximal.

Example:
<div style="display: flex; justify-content: space-between;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/0c434272-dafb-44cc-98d6-7dd38b992f4f" style="height: 500px;">
</div>

This example shows the 3 situations. The two top situations are the situations where ball X is not included in another ball (so can be a maximum ball). And the bottom situation is the case where ball X is included in another ball (so cannot be a maximum ball).


However, it is not at all optimised to carry out a complete run to check whether a single ball is maximal, especially if there are many other balls to test. We then noticed that most of the centres of the maximum balls lie on the median axis, which is the skeleton of the image, the white lines visible on the squared Euclidean distance map.

<div style="display: flex; justify-content: space-between;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/5798b7f3-399d-4302-96a0-379629d67554" style="height: 380px;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/d0712006-a098-444c-81c7-569da97c3dce" style="height: 380px;">
</div>


This is why, in the optimized version, we start by processing circles where the points are on the median axis. This allows us to eliminate non-maximum balls much more quickly, and thus to make faster and faster runs.

We then save the maximum balls in a "Ball" list. "Ball" is a structure where the object has an x and y position of its center, as well as its radius.


One of the reasons we decided to store the image data with maximum balls rather than a simple two-dimensional array with pixel values is that with maximum balls it's simpler and faster. In fact, it's easy to write the contents of an image in a text file: you make a list of all the maximum balls. Write the coordinates of the centre of the ball and the radius.

Example:

<pre>
[337, 588, 1044]
[509, 665, 1025]
[405, 331, 1025]
[342, 585, 1018]
[402, 329, 1018]
[500, 658, 970]
[256, 238, 932]
...
</pre>

We could simply write a script that reads each line of the text file and draws the balls. That's what we're going to do in part 3.

If you'd like to know more about the squared Euclidean distance map and maximum balls, I've put together a thesis (by my teacher) which explains the subject in more detail: [Thesis Link](https://perso.liris.cnrs.fr/laure.tougne/theses_doctorants/these_Aurelie_leborgne.pdf)


### Part Three: Image Reconstruction

With this list of "Balls", we can now draw the circles and see that we've got the shape of the original image:

Now all we need to do is fill in the circles to obtain the complete shape:
<div style="display: flex; justify-content: space-between;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/353d8d8e-b39b-4916-8a97-7a5c3e65adc4" style="height: 450px;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/75b177f1-196b-476b-a8d7-4378d88829f3" style="height: 450px;">
</div>

As for the fidelity of the reproduction, it's 96%. We think that the missing 4% is due to the dpi (dot per inch), which is not the same between the original image and the image obtained after all the operations.

We've also added functions for drawing graphs to see the differences in calculation time, result, etc. between the brute force and optimized methods, and also whether noise had an impact on calculation time or result. Noise is a mutation of the image. 

Example of an image with a noise level of 0 (no noise) and an image with a noise level of 4 (big mutation):
<div style="display: flex; justify-content: space-between;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/8dc0b544-340a-4a3d-9235-eb0aa281e3a9" style="height: 350px;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/410b586b-9ed2-43bb-b528-509d0a7dc894" style="height: 350px;">
</div>


To create the graphics, we used the "Chart" graphic control.

## Web Part:

To show our results clearly, we decided to display them as an HTML page.

In the Web folder, you'll find the HTML file you need to open to view the page and our results.

However, we wanted to go further and go beyond our limits, so we made a second version of the web part, in JavaScript only. We've put all our efforts into creating an elegant, dynamic, and interactive site, and here's how to access it: [Website Link](https://ezwin.netlify.app)

If you'd like to know more about the design of the first version in HTML and the second version in JavaScript, I've made a video in which I explain the process and our choices in more detail: [Video Link](https://youtu.be/zJ3VeK5o50Q?si=sYNVVdltUfIWi16q)

![home2](https://github.com/Artemis1799/Image_processing/assets/147591539/25f2d361-0487-492e-a3ba-0e0945ad540b)

