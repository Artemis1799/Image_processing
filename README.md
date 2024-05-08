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
![image](https://github.com/Artemis1799/Image_processing/assets/147591539/fd124629-3ed0-4dda-8be1-a44dc08b6e95)




### Part Two: Maximum Balls

To redraw the shape, we didn't want to simply run through the list and draw a white pixel when the value is 0 and a black pixel when the value is other than 0. We had to find another solution.

Looking at the squared Euclidean distance map and image representations, we noticed that, in reality, all shapes can be drawn with filled circles, whose radii are the square root of the pixel distance stored in the squared Euclidean distance map table. This is why we're going to use the maxiamale ball method.

This method consists in keeping only those balls that are maximal, i.e. that are not entirely contained within another ball. For if a ball is non-maximal, this means that another ball or set of balls can draw the same surface as it, and so, for the sake of optimization, we're going to keep only the maximal balls to display only the circles that are really useful for reconstructing the image.

To find out whether a ball X is maximal, we go through all the other balls in the array, and if the distance between the center of ball X and the center of the ball we've gone through, plus the radius of the smallest circle, is smaller, then it's included in another ball and therefore not maximal. If, after testing with all the balls, ball X is still not declared as "non-maximal", then it is maximal.

Example: tests with radii

However, running an entire course to check whether a single ball is maximal is not at all optimized, especially if there are many other balls to test. This is the skeleton of the image, the white lines seen on the squared Euclidean distance map.

<div style="display: flex; justify-content: space-between;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/fd124629-3ed0-4dda-8be1-a44dc08b6e95" style="height: 300px;">
  <img src="https://github.com/Artemis1799/Image_processing/assets/147591539/1e84682e-85bb-446a-b9ef-05c304e9cb39" style="height: 300px;">
</div>
![7](https://github.com/Artemis1799/Image_processing/assets/147591539/4703bc78-3e31-4312-a126-b69545bbd2a1)

This is why, in the optimized version, we start by processing circles where the points are on the median axis. This allows us to eliminate non-maximum balls much more quickly, and thus to make faster and faster runs.

We then save the maximum balls in a "Ball" list. "Ball" is a structure where the object has an x and y position of its center, as well as its radius.

If you'd like to know more about the squared Euclidean distance map and maximum balls, I've put together a thesis (by my teacher) which explains the subject in more detail: [Thesis Link](https://perso.liris.cnrs.fr/laure.tougne/theses_doctorants/these_Aurelie_leborgne.pdf)

### Part Three: Image Reconstruction

With this list of "Balls", we can now draw the circles and see that we've got the shape of the original image:

Now all we need to do is fill in the circles to obtain the complete shape:

As for the fidelity of the reproduction, it's 96%. We think that the missing 4% is due to the dpi (dot per inch), which is not the same between the original image and the image obtained after all the operations.

We've also added functions for drawing graphs to see the differences in calculation time, result, etc. between the brute force and optimized methods, and also whether noise had an impact on calculation time or result (noise is a mutation of the image: example image without noise and image with noise).

To create the graphics, we used the "Chart" graphic control.

## Web Part:

To show our results clearly, we decided to display them as an HTML page.

In the Web folder, you'll find the HTML file you need to open to view the page and our results.

However, we wanted to go further and go beyond our limits, so we made a second version of the web part, in JavaScript only. We've put all our efforts into creating an elegant, dynamic, and interactive site, and here's how to access it: [Website Link](https://ezwin.netlify.app)

If you'd like to know more about the design of the first version in HTML and the second version in JavaScript, I've made a video in which I explain the process and our choices in more detail: [Video Link](https://youtu.be/zJ3VeK5o50Q?si=sYNVVdltUfIWi16q)

![home2](https://github.com/Artemis1799/Image_processing/assets/147591539/25f2d361-0487-492e-a3ba-0e0945ad540b)

