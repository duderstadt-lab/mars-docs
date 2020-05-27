---
layout: kcp
title: Change Point Finder
permalink: /docs/kcp/ChangePointFinder/index.html
---

#### Introduction on the Kinetic Change Point Algorithm
Single-molecule imaging approaches provide datasets that reveal the kinetic behavior of individual molecules within the dataset. Detailed characterisation of rate changes and pausing is possible yielding valuable information on the kinetics of processes such as transcription, translation, and motor protein precession on an immobilized target. Due to the stochastic nature of these processes the obtained trace can usually be described best by a piece-wise linear motion.

<div style="text-align: center"><img src='{{site.baseurl}}/docs/kcp/img/img1.png' width="450"/></div>  
_Figure 1: Theoretical trace with identified change points (blue dots) and fitted linear regression lines (blue dashed lines)._

The kinetic change point (KCP) algorithm<sup>1</sup> used in the KCP analysis tools in **Mars** was specifically designed to detect these different linear regions in single-molecule traces effectively. In short, the algorithm detects the change points in the trace (fig. 1, blue dots) and fits a linear regression line between the points to yield the rates for every segment (fig. 1, blue regression lines). The algorithm was especially developed for motion of processive single molecules having Gaussian noise and assumes piece-wise linear motion. For a detailed mathematical explanation of the model and the decision strategy the reader is referred to the paper by Hill _et al._<sup>1</sup>.

#### Implementation of the Algorithm
The recursive binary search strategy for multiple change points identification algorithm (fig. 2) can be visualized in three distinct steps:
1. Initial sweep: Initially, the boundaries of the recorded trace are used to limit the change point search region. As soon as the first change point is identified, it is recorded and the search region is reduced accordingly. This process is repeated until the end of the trajectory is reached. This yields a first list of change points in the trajectory.
2. Refinement: The positions of the identified change points are optimized locally by re-finding the change point in a smaller region around its identified location.
3. Fitting: Fit a linear regression line between all change points found. As an output the coordinates of the change points as well as the regression fits (slope and intercept) are obtained.


<div style="text-align: center"><img src='{{site.baseurl}}/docs/kcp/img/img2.png' width="650"/></div>  
_Figure 2: Visualization of the algorithm to find multiple change points._

#### Inputs
* _MoleculeArchive_ - MoleculeArchive to find the kinetic change points for.
* _X Column_ - X column input (f.e. time).
* _Y Column_ - Y column input (axis along which displacement takes place).
* _Confidence value_ - Confidence interval, used to distinguish noise from real change points.
* _Global sigma_ - Specify the value of the error corresponding to the experimental noise to take into account.
* _Region source_ - Set the region source to Molecules or Metadata.
* _Calculate from background_ - Check in case the background error should be calculated from the specified background region.
* _Background region_ - Region to calculate a background error value (sigma) from.
* _Analyze region_ - Check box if only the region of interest has to be analyzed.
* _Region_ - Region of the tracks to analyse. Enables the selection of a region of interest.
* _Fit steps (zero slope)_ - Fit regression lines with a slope of 0, especially useful for fitting pauses.
* _Include_ - Choose to include 'all', 'tagged with' or 'untagged' molecules in the analysis.
* _Tags (comma separated list)_ - List of tags to base on which traces are included in the analysis.

<img src='{{site.baseurl}}/docs/kcp/img/img4.png' width='350' />

#### Output
* The output of the KCP analysis is the synthesis of a segments table as additional tab in the data section of the 'Molecules' tab in **Rover**. This table contains:
  * x1: X-coordinate of the change point, start point segment.
  * y1: Y-coordinate of the change point, start point segment.
  * x2: X-coordinate of the change point, end point segment.
  * y2: Y-coordinate of the change point, end point segment.
  * A: Slope of the linear line (rate).
  * sigma_A: Error on the slope calculation.
  * B: Intercept of the linear line.
  * simga_B: Error on the intercept calculation.


<img src='{{site.baseurl}}/docs/kcp/img/img5.png' width='650' />

#### How to run this Command from a groovy script

```Groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*;

//Make an instance of the Command you want to run
final KCPCommand kcpCalc = new KCPCommand();

//Populates @Parameters Services etc. using the current context which we get from the ImageJ Input
kcpCalc.setContext(ij.getContext());

//Set all the input parameters
kcpCalc.setArchive(archive);
kcpCalc.setXcolumn("y");
kcpCalc.setYcolumn("slice");
kcpCalc.setConfidenceLevel(0.99);
kcpCalc.setGlobalSigma(1.0);
kcpCalc.setRegionSource("Molecules");
kcpCalc.setCalculateBackgroundSigma(False);
kcpCalc.setBackgroundRegion(None);
kcpCalc.setAnalyzeRegion(False);
kcpCalc.setRegion(None);
kcpCalc.setFitSteps(False);
kcpCalc.setIncludeTags("Active");

//Run the Command
kcpCalc.run();

```


<sup>1</sup> Hill, F.R.; Van Ooijen, A.M. & Duderstadt K.E. (2018), "Detection of kinetic change points in piece-wise linear single molecule motion", _J. Chem. Phys._ **148**: 123317, [https://doi.org/10.1063/1.5009387](https://aip.scitation.org/doi/10.1063/1.5009387)
