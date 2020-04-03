---
layout: image
title: Dog Filter Properties
permalink: /docs/image/DogFilterProperties/index.html
---

The [Difference of Gaussians (DoG)](https://en.wikipedia.org/wiki/Difference_of_Gaussians) filter enhances peaks by applying [Gaussian convolutions (blur)](https://en.wikipedia.org/wiki/Gaussian_blur) with different sigmas to two image copies. The final filtered image is calculated by taking the difference between the two Gaussian convolutions. Hence the name difference of gaussian. This algorithm works especially well for enhancing single fluorescent molecules imaged in TIRF experiments as seen in the images below. Additionally, if the image is very noisy, a median filter can be applied prior to applying a DoG filter. However, this does not appear to make a significant difference in the example image evaluated below.

This filter is conveniently available as an image op in ImageJ2. Several examples of applications of the DoG filter can be found in the jupyter notebooks provided with the [ImageJ tutorials](https://github.com/imagej/tutorials). The scripts used to generate the figures below can be found at the bottom and the example data is available in the [mars-tutorials](https://github.com/duderstadt-lab/mars-tutorials) repository.

<img align='center' src='{{site.baseurl}}/docs/image/img/DogFilterExampleImage.png' width='750' />

#### Peak enhancement

To access the peak enhancement as a function of DoG radius, three peaks with different intensities were systematically evaluated for a range of radii. The resulting images as displayed together with their x position profiles. The profiles are displayed in terms of detection threshold (N) where N is the number of standard deviations above the mean (mean + N * stdDev). For convenience the mean is set to zero in the profiles and multiples of the standard deviation are displayed on the y axis. Here mean and standard deviation were calculated for the entire example image above for each radius. Clearly a radius of 2.0 is the winner in terms of overall enhancement.

<img align='center' src='{{site.baseurl}}/docs/image/img/DogFilterPeakProfileComparison.png' width='450' />

<img align='center' src='{{site.baseurl}}/docs/image/img/DogFilterPeak1.png' width='450' />

<img align='center' src='{{site.baseurl}}/docs/image/img/DogFilterPeak2.png' width='450' />

<img align='center' src='{{site.baseurl}}/docs/image/img/DogFilterPeak3.png' width='450' />

#### Peak detection

Finally comparison of ground truth for a video vs. located peaks for different threshold values for one dog radius. First raw peaks count plot vs. ground truth.

<img align='center' src='{{site.baseurl}}/docs/image/img/xxx.png' width='450' />

#### Groovy scripts

DoG filter op script:

```groovy
#@ Img image
#@ double radius
#@OUTPUT Img output
#@ ImageJ ij

final double smaller = radius / Math.sqrt( 2 ) * 1.1
final double bigger = radius / Math.sqrt( 2 ) * 0.9

output = ij.op().run("dog", image, smaller, bigger)
```
Generate an output table of x profiles for a range of DoG radii given starting and ending x and y coordinates:

```groovy
#@ Img image
#@ ImageJ ij
#@ Integer Y
#@ Integer Xstart
#@ Integer Xend

#@OUTPUT MarsTable table

import de.mpg.biochem.mars.table.*
import org.scijava.table.*

table = new MarsTable("Dog Profiles")

def posCol = new DoubleColumn("Position")
def colRaw = new DoubleColumn("Raw")
def colRawThreshold = new DoubleColumn("Raw Threshold")

double RawMean = ij.op().stats().mean(image).getRealDouble()
double RawStdDev = ij.op().stats().stdDev(image).getRealDouble()

println("Raw ")
println("Mean: " + RawMean)
println("StdDev: " + RawStdDev)

rawra = image.randomAccess()
//Position , Dimension
//y
rawra.setPosition(Y,1)

for (def x=Xstart; x<=Xend; x++) {
	posCol.add(x as double)

	//Position , Dimension
	//x
	rawra.setPosition(x,0)

	colRaw.add(rawra.get().getRealDouble())
	colRawThreshold.add((rawra.get().getRealDouble() - RawMean) / RawStdDev)
}

table.add(posCol)
table.add(colRaw)
table.add(colRawThreshold)

//Now let's calculate it for different dog images...
//for (double radius = 1.0d; radius < 10.0d; radius++) {
for (double radius = 1.2d; radius < 3.0d; radius+= 0.2d) {

	double sigma1 = radius / Math.sqrt( 2 ) * 0.9d
	double sigma2 = radius / Math.sqrt( 2 ) * 1.1d

	def dog = ij.op().run("dog", image, sigma2, sigma1)
	def dogra = dog.randomAccess()
	dogra.setPosition(Y,1)

	//def colDog = new DoubleColumn("Dog " + radius)
	def colThreshold = new DoubleColumn("" + radius)

	double mean = ij.op().stats().mean(dog).getRealDouble()
	double stdDev = ij.op().stats().stdDev(dog).getRealDouble()

	println("Dog " + radius)
	println("Mean: " + mean)
	println("StdDev: " + stdDev)

	for (def x=Xstart; x<=Xend; x++) {
		//Position , Dimension
		//x
		dogra.setPosition(x,0)		
		//colDog.add(dogra.get().getRealDouble())
		colThreshold.add((dogra.get().getRealDouble() - mean) / stdDev)
	}
	//table.add(colDog)
	table.add(colThreshold)
}
```

Generate a strip of different images for different DoG radii given an input image with a single peak:

```groovy
#@ Img image
#@output Img profileSeries
#@ ImageJ ij

import net.imglib2.RandomAccessibleInterval
import net.imglib2.type.numeric.real.FloatType
import net.imglib2.interpolation.randomaccess.NLinearInterpolatorFactory
import net.imglib2.FinalInterval

w = image.dimension(0)
h = image.dimension(1)

profileSeries = ij.op().run("create.img", [w * 10, h, 1], new FloatType())
proRA = profileSeries.randomAccess()

//ij.op().create().img(FinalInterval.createMinSize(0, 0, w * 10, h), new FloatType())

int X0 = 0

//Now let's calculate it for different dog images...
//for (double radius = 1.0d; radius < 10.0d; radius++) {
for (double radius = 1.2d; radius < 3.0d; radius+= 0.2d) {

	double sigma1 = radius / Math.sqrt( 2 ) * 0.9d
	double sigma2 = radius / Math.sqrt( 2 ) * 1.1d

	def dog = ij.op().run("dog", image, sigma2, sigma1)
	def dogra = dog.randomAccess()

	for (x = 0; x < w; x++) {
		for (y = 0; y < h; y++) {
			//Position , Dimension
			//x
			dogra.setPosition(x, 0)
			//y
			dogra.setPosition(y, 1)

			proRA.setPosition(x + X0, 0)
			proRA.setPosition(y, 1)

			proRA.get().set(dogra.get())
		}
	}
	X0 += w
}
```
