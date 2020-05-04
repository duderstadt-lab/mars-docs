---
layout: example
title: Flow Magnetic Tweezers Pipeline
permalink: /examples/flow-Magnetic-Tweezers/index.html
---

This example demonstrates a complete workflow for tracking and processing the motion of beads tethered to individual DNA molecules. The example video provided was collected using a novel force spectroscopy technique called Flow Magnetic Teezers (Agarwal et al., unpublished). The technique enables multidimensional force manipulation of individual molecules. Here individual DNAs are stretched by a combination of lateral and vertical forces from flow and magnetic tweezers, respectively. During the experiment, DNA molecules are probed by changing the flow and supercoiled through the application of torque using magnet rotation. Finally, *E.coli* DNA gyrase is introduced into the flowcell. Gyrase relaxes postive supercoils in the DNA and introduces negative supercoils (ref - as footnote). These transformations lead to lengthening and shorting of the DNA observed as changes in the bead position as a function of time.

The first step in the workflow is tracking the beads as a function of time using the Mars [Peak Tracker](../../docs/image/PeakTracker). Once optimal peak finding and tracking settings have been found this command generates a MoleculeArchive containing the tracking results. The remainder of the example relies on a script that automates the process of feature recognition by calculating a series of single value parameters for the slopes and differences within and between regions of interest. In one step this script also tags molecules based on thresholds for each feature. In Mars tags allow for rapid and efficient reslicing of data. Subsets of molecules that exhibit unique combinations of behaviors can be rapidly called up and processed. Once the archive is tagged and parameterised, a final `.csv` file is generated with necessary data to make plots showing the velocity of coiling by DNA Gyrase vs the start time of events. Have fun Tweezing. <br>

Gyrase is a bacterial enzyme that maintains the topology of chromosomes and facilitates important processes like DNA replication and transcription. It does so by opening the G-segemnt (gate) and passing the T-segment (transfer) through it and religating the DNA G-segment. This results in a -2 change in linking number. In the cell, gyrase using energy from ATP to resolve positive supercoils and introduce negative supercoils as shown in the figure. <br><br>

<a style='text-decoration: none; color: orange;'>
  <img src='{{site.baseurl}}/examples/img/fmt/gyrase_cycle.png' style='width: 950px'>
  <div style='width: 950px; text-align: center;'>Cycle of DNA gyrase</div>

The single-molecule assay used here tethers DNA molecules to a slide surface and a super-paramagnetic bead. There is a constant flow resulting in a drag force on the molecule and also topological control of the DNA molecule as the bead is trapped in the magnetic field of the antiparallely mounted block magnets.<br>

During the assay, there are several checkpoints which characterize the bead and discussed in [Running analysis Pipeline: Selecting parameter boundaries and tags](#select_parameter_tag). Following these steps, the DNA is left positively coiled and gyrase is introduced: <br><br>

<a style='text-decoration: none; color: orange;'>
  <img src='{{site.baseurl}}/examples/img/fmt/sm_assay.png' style='width: 950px'>
  <div style='width: 950px; text-align: center;'>Gyrase reaction on Flow Magnetic Tweezers</div>


### 1. Importing the video in FIJI

Drag and drop the file `FMT_Example_Video.tif` or the folder `FMT_Example_Video` containing the video in FIJI and open it as virtual stack (HOW DO YOU DO THIS EXACTLY - WHICH MENU ETC..). This is a segment of the total field of view from a gyrase experiment. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image001.png' width='950' />

### 2. Opening tracker from Mars plugin: option -> MoleculeArchive Suite -> Image Processing -> Peak Tracker

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image002.png' width='950' />

### 3. Choosing boundaries and parameters for the peak finder and subsequent tracking

The program uses Difference of Gaussian filter ([DoG filter](../../docs/image/DogFilterProperties)) to find peak position with sub pixel precision. Once peaks are found, the program tracks them through the stacks making a trajectory of the bead movement in xy-plane.

After choosing appropriate settings for the DoG filter radius, Detection threshold and minimum distance between peaks, Preview checkbox displays the tracked peaks per frame. The Preview slice slider can we use to check the tracking of subsequent frames. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image003.png' width='950' />

Once appropriate setting are chosen, the tracking can be started. Console here displays the steps and times, status bar the progress of peak fitting and tracking. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image004.png' width='950' />

### 4. MARS GUI and archive

Once the tracking is done, an archive is made in memory. Console here displays the time taken. Here it took 23 minutes. It can vary depending on the system and the memory allotted to ImageJ. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image005.png' width='950' />

This file can also be saved as a store where all molecules are saved as separate files and loaded on as-needed basis. This format is very memory efficient and still compatible with multi-threading. All molecules here have unique IDs. The x and y tracking information is given by default. At this point **`Step1_Add_Regions.groovy`** can be executed. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image006.png' width='950' />

### 5. Adding regions of different activites to the archive

The groovy script adds different regions as displayed. Like reversals, Region of gyrase activity, slope of supercoiling steps etc.

```groovy

//MarsRegion(name, column, start, end, hex color, opacity (0-1))

metadata.putRegion(new MarsRegion("First Reversal RF", "slice", 100, 120, "#FFCA28", 0.2))
metadata.putRegion(new MarsRegion("First Reversal FF", "slice", 200, 220, "#42A5F5", 0.2))

metadata.putRegion(new MarsRegion("Gyrase Reaction", "slice", 4600, 6990, "#42A5F5", 0.2))

```

The Regions are used by the **`Step2_FMT_Pipeline_truncated.groovy`** to calculate different parameters and tag molecules. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image007.png' width='950' />

### 6. Running analysis Pipeline: Selecting parameter boundaries and tags

For the analysis pipeline stated previously, following figures explain the steps to characterize molecules in the archive. Following is a singly-tethered molecule showing a gyrase reaction. Notice that the trace is “flipped”, The coordinates for projected length here are given in microns. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image013.png' width='950' /> <br><br>

 Notice all the parameters and values, these were calculated with the analysis pipeline. Only prerequisite is that the regions should be added. The regions shown here are labelled as such with the add regions file. Additional to the Parameters, molecules are also classified using tags. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image014.png' width='950' /> <br><br>

So how does the program know that the molecule shown above is a singly-tethered, coilable and what force the molecule experiences? The inputs to analyse these questions are shown here. They will be discussed in detail through rest of the page. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image015.png' width='600' /> <br><br>

Different aspects of the molecules are analyzed. Shown below is the whole trace. With parameters on the right-hand side. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image016.png' width='950' /> <br><br>

#### Reversal at the beginning

The zoom is set to initial part of the trace called reversal (starting from left side: beginning of the trace). The first 238 frames here show a flow reversal. The regions are named first reversal RF (reverse flow) and first reversal FF (forward flow). An Inbuilt function called region difference calculator is used. As the name suggest, it calculates the difference between the stated region. Here the parameter was rev_begin_x (reversal at the beginning for x coordinate). <br><br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image019.png' width='600' /> <br><br>

```groovy

//Get Regions form the metadata

First_Reversal_RF = metadata.getRegion("First Reversal RF")
First_Reversal_FF = metadata.getRegion("First Reversal FF")

//First Reversal: Adding the Parameters rev_begin_x and rev_begin_y

//RegionDifferenceCalculatorCommand.calcRegionDifference(molecule, xColumn, yColumn, regionOne, regionTwo, parameterName)
RegionDifferenceCalculatorCommand.calcRegionDifference(molecule, "slice", "x", First_Reversal_RF, First_Reversal_FF, "rev_begin_x")
RegionDifferenceCalculatorCommand.calcRegionDifference(molecule, "slice", "y", First_Reversal_RF, First_Reversal_FF, "rev_begin_y")

//reversal start
if (molecule.getParameter("rev_begin_x") > reversalLowerBound &&\
  molecule.getParameter("rev_begin_x") < reversalUpperBound) {
  molecule.addTag("mobile_begin")
}

//reversal start neg
if (molecule.getParameter("rev_begin_x") > reversalLowerBoundNeg &&\
  molecule.getParameter("rev_begin_x") < reversalUpperBoundNeg) {
  molecule.addTag("mobile_begin_neg")
}

```

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image017.png' width='950' /> <br><br>


The reversal threshold separate mobile beads from stuck beads. Also, some multiply-tethered beads can be classified using reversals.

#### Magnet rotation at high force


Following the reversal, magnets are rotated at 2rot/sec and high flow rate (20 &mu;l/min - ca. 2pN) for 1200 frames to distinguish singly-tethered molecules. Two different parameters are calculated here. <br>

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image022.png' width='600' /> <br><br>

A singly-tethered bead is shown here. <br>

```groovy

//Get regions
coil20_Positive_Peak = metadata.getRegion("coil20 Positive Peak")
coil20_Negative_Peak = metadata.getRegion("coil20 Negative Peak")

//RegionDifferenceCalculatorCommand.calcRegionDifference(molecule, xColumn, yColumn, regionOne, regionTwo, parameterName)
//coil20
RegionDifferenceCalculatorCommand.calcRegionDifference(molecule, "slice", "x", coil20_Positive_Peak, coil20_Negative_Peak, "coil20")

//coilable20
if (molecule.getParameter("coil20") > coilable20LowerBound &&\
  molecule.getParameter("coil20") < coilable20UpperBound) {
    molecule.addTag("coilable20")
}

```

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image020.png' width='950' /> <br><br>

Here the magnets were rotated 150 turns in both positive and negative directions. Magrot20f is the region where the magnets are rotated. coil20 Negative Peak and coil20 Positive Peak are respective regions where sigma for DNA is highest for respective coilings. A single peak is observed here as DNA separates locally when negatively coiled, dissipating the torsion. This is not the case for bead that is tethered via multiple DNA molecules. To separate these molecules further, an additional region Slope_Neg_20 is calculated. <br>

```groovy

//Get regions
Slope_Neg_20 = metadata.getRegion("Slope_Neg_20")

//Calculate slopes
output = table.linearRegression("slice","x", Slope_Neg_20.getStart(), Slope_Neg_20.getEnd())
molecule.setParameter("Slope_Neg_20", output[2])

//doubleCoiling
if (molecule.getParameter("Slope_Neg_20") > doubleCoilingLowerBound &&\
  molecule.getParameter("Slope_Neg_20") < doubleCoilingUpperBound) {
    molecule.addTag("doubleCoiling")
}

```

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image021.png' width='950' /> <br><br>

The Slope_Neg_20 parameter is the slope of the region shown. Following thresholds are empirically chosen.

The tags here are doubleCoiling and coilable20 respectively.

#### Force calculation

Bead fluctuations lateral to flow direction are used to numerically ascertain force and length of the DNA molecule. As only the projected length is known, DNA is assumed to have Worm-like chain behavior.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image025.png' width='600' />

The <var>&delta;</var><var>y</var><sup>2</sup> is the the mean squared displacement of the bead excursions in the y direction. They are taken from the Force2p5 region.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image026.png' width='600' /> <br><br>

Other parameters such as temperature, persistanceLength, contourLength are given in the pipeline.

The other two values for slidingForceWindow and stdSlidingForce are used to check if the bead, in the chosen, get transiently stuck. It the bead gets stuck, it registers as higher force. The sliding windows iteratively calculates mean and standard deviation of each window. If a particular window deviates more than threshold MSD, the bead is tagged as stuckForce. This also makes sense as MSD of these fluctuations are supposed to be constant throughout the region.

```groovy

//Get regions
Force2p5 = metadata.getRegion("Force2p5")

//Force Calculation and tagging
logService.info("Calculating force...")
archive.getMoleculeUIDs().parallelStream().forEach{ UID ->
      Molecule molecule = archive.get(UID)
      MarsTable table = molecule.getDataTable()

	 double msdForce = table.msd("y_drift_corr", "slice", Force2p5.getStart(), Force2p5.getEnd())   //inserts different start and stop slice with each loop

	 msdForce = msdForce*conversionPixelToMicron*conversionPixelToMicron
	 double[] solution;
	 try {
	 	solution = MarsMath.calculateForceAndLength(persistenceLength, contourLength, temperature, msdForce);
	 } catch (Exception e) {
	 	return;
	 }

	 double force = solution[0]
	 double length = solution[1]
	 String force1 = "Force_PL35";			// make Force_2.5, Force_5 for different flow rates
	 String length1 = "Length_PL35";			// similarly for length

	 molecule.setParameter(force1, force);
	 molecule.setParameter(length1, length);

	 DoubleColumn msdColSlide = new DoubleColumn("MSDs")

	  double MaxMSD = 0
	  double MinMSD = 0

      for (int row = 0; row < table.getRowCount() - slidingForceWindow; row++) {
	      	if (table.get("slice",row) >= Force2p5.getStart() && table.get("slice",row) <= Force2p5.getEnd() && table.getRowCount() > row + slidingForceWindow) {
				double msdSlideForce = table.msd("y_drift_corr","slice",table.get("slice",row),table.get("slice",row + slidingForceWindow))

				if (msdSlideForce > MaxMSD)
					MaxMSD = msdSlideForce

				if (msdSlideForce < MinMSD)
					MinMSD = msdSlideForce

				msdColSlide.add(msdSlideForce)
	      	}
	  }

	  MarsTable tempTable = new MarsTable("MSDs table")
	  tempTable.add(msdColSlide)

	  double msdSTD = tempTable.std("MSDs")
	  double meanMSD = tempTable.mean("MSDs")

	  if (msdSTD*stdSlidingForce > Math.abs(MaxMSD - meanMSD)) {
	  	molecule.addTag("stuckForce")
	  } else if (msdSTD*stdSlidingForce > Math.abs(meanMSD - MinMSD)) {
	  	molecule.addTag("stuckForce")
	  }

      archive.put(molecule)
 }

```

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image024.png' width='950' />

#### Rotation at reaction flow rate

Before the gyrase reaction, the molecule is supercoiled at reaction flow rate. This part of the trace shows the molecule being coiled negatively at first and then positively. Ultimately, the tether is left at +60 turns for the gyrase reaction.

Similar to the coilable20 tag, coilable2p5 is a region difference calculation. It helps further refine finding optimal traces. The regions used for this tag are coil2p5 Peak and coil2p5 Background.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image033.png' width='600' />

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image028.png' width='950' />


Positive Coiling Slope and Negative Coiling Slope regions are used as coordinate conversion tool from pixels to cycles/sec. Here the magnet rotation is 1 rot/sec and frame acquisition rate is 4 frames per second. Hence turns_per_slice is 1 rot/4 frames = 0.25. turns_per_cycle is one cycle of gyrase as it is a type II topoisomerase.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image034.png' width='600' />

```groovy

//Get regions
Negative_Coiling_Slope = metadata.getRegion("Negative Coiling Slope")
Positive_Coiling_Slope = metadata.getRegion("Positive Coiling Slope")


//Calculate poscycles and negcycles on a molecules-by-molecule basis
logService.info("Calculating molecule-by-molecule cycle numbers...")
archive.getMoleculeUIDs().parallelStream().forEach({ UID ->
      Molecule molecule = archive.get(UID)
      MarsTable table = molecule.getDataTable()

	  double[] pos_coils_per_slice = table.linearRegression("slice", "x_drift_corr", Positive_Coiling_Slope.getStart(), Positive_Coiling_Slope.getEnd())

	  molecule.setParameter("pos_coil_slope", pos_coils_per_slice[2])

	  	if (!table.hasColumn("poscycles"))
			table.appendColumn("poscycles")

		for (int row = 0; row < table.getRowCount(); row++) {
			double conversion = (-1)*(turns_per_slice/pos_coils_per_slice[2])/turns_per_cycle
			table.setValue("poscycles", row, conversion*table.getValue("x_drift_corr", row))
		}

	  double[] neg_coils_per_slice = table.linearRegression("slice", "x_drift_corr", Negative_Coiling_Slope.getStart(), Negative_Coiling_Slope.getEnd())

	  molecule.setParameter("neg_coil_slope", neg_coils_per_slice[2])

	  	if (!table.hasColumn("negcycles"))
			table.appendColumn("negcycles")

		for (int row = 0; row < table.getRowCount(); row++) {
			double conversion = (1)*(turns_per_slice/neg_coils_per_slice[2])/turns_per_cycle
			table.setValue("negcycles", row, conversion*table.getValue("x_drift_corr", row))
		}

      archive.put(molecule)
})

```

#### Gyrase reaction

Final part of the trace after all the checkpoints is the gyrase reaction.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image031.png' width='950' />

In the gyrase reaction region, there are Before Enzyme and After Enzyme regions. As gyrase reactions are force dependent, for any force above ca. 0.5 pN, only positive coils are resolved (chi reaction). For lower forces, both positive coils are resolved and negative coils are introduced (alpha reactions). Following thresholds tag molecules either alpha or chimol. Also, the parameter activity_score is calculated in the Gyrase Reaction region.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image035.png' width='600' />

```groovy

//Calculating activity score
.forEach({ UID ->
    Molecule molecule = archive.get(UID)
    MarsTable table = molecule.getDataTable()

    if (table == null)
        return

	 double startTime = Gyrase_Reaction.getStart()
     double endTime = Gyrase_Reaction.getEnd()

     if (table.getValue("slice", table.getRowCount()-1) < Gyrase_Reaction.getEnd()) {
       endTime = table.getValue("slice", table.getRowCount()-1) - 10
     }

    double gMax = -1000000
    double gMin =1000000

    for (int row=0; row<table.getRowCount();row++) {
        double start = table.getValue("slice", row)
        double end = table.getValue("slice", row)

        if (start >= startTime && end <= endTime) {
            double cMax = table.getValue("poscyclesG", row)
            if (gMax < cMax)
                gMax = cMax

            double cMin = table.getValue("poscyclesG", row)
            if (gMin > cMin)
            	gMin = cMin
        }
    }

	molecule.setParameter("activity_score", gMax - gMin)

```

**Additional Thresholds shown here:**

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image037.png' width='600' />

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image036.png' width='600' />

```groovy

//From the reversal parameters calculated

//stuckRev
if (Math.abs(molecule.getParameter("rev_begin_x")) < stuckRevThreshold &&\
Math.abs(molecule.getParameter("rev_begin_y")) < stuckRevThreshold) {
  molecule.addTag("stuckRev")

}

//stuckMSD
if (molecule.hasTag("stuckRev") &&\
  molecule.getParameter("x_MSD") < stuckMSDThresholdx &&\
  molecule.getParameter("y_MSD") < stuckMSDThresholdy) {
    molecule.addTag("stuckMSD")
}

//shortTrajectory
int deadSlice = table.getValue("slice",table.getRowCount()-1)
if (deadSlice < minLength)
  molecule.addTag("shortTrajectory")

//Drift Calculator
  DriftCalculatorCommand.calcDrift(archive, "stuckMSD", "x", "y", "x_drift", "y_drift", false, "mean", "end")

//Drift Corrector
  DriftCorrectorCommand.correctDrift(archive, driftZeroRegionStart, driftZeroRegionEnd, "x_drift", "y_drift", "x", "y", "x_drift_corr", "y_drift_corr", false)


```

dirftZeroRegion is the number of frames during drift correction where coordinates are set to 0 for this reference region.
conversionPixeltoMicron is the pixels size in microns.



stuckRevThreshold uses the reversal parameters rev_begin_x and rev_begin_y to tag immobile beads. There should be no motion during flow reversal in either x or y coordinate. These are tagged stuckRev.

stuckMSDThresholdx and stuckMSDThresholdy parameters are calculated MSD for the whole trace for x and y respectively. Molecules within the threshold are tagged stuckMSD. There are than usd to calculate and correct drift.

minLength tags molecules shortTrajectory. This filters out all the the trajectories which have less frames than the threshold provided here by excluding this tag.

Once appropriate boundaries are chosen, the program can be run. Values for this particular example are set by default in the script.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image008.png' width='950' />

Once the program runs, the activity can be traced. Here it took 7 minutes to analyse.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image009.png' width='950' />


### 7. Final Data

The data shown is now drift corrected, different parameters calculated on the right are unique to this particular molecules. The molecules are tagged and the categories can be displayed by searching for the tag (top left) which also shows count of the tags. Calculated columns are shown at the bottom.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image010.png' width='950' />

At this point, **`Step3_generate_csv.groovy`**  can be executed. It extracts the coordinate, force and rate information form the analyzed data. Once the analysis pipeline is through, the tags singleTether (include: coilable20, exclude: doubleCoiling and shortTrajectory) and coilable2p5 are analysed with a sliding window which finds the maximum inclination for both positive and negative coiling from the poscycles and negcycles columns respectively.

Once the Step 3 groovy is done, a csv file is generated in the directory of the archive named **`Gyrase_Scatter.csv`**.

### 8. Plotting the dataset

For plotting, the csv can either be placed in the same folder as the python script Step4_scatter_plot.py or a path for this csv can be provided to any given IDE. Here Spyder is used.

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image011.png' width='950' />

<img align='center' src='{{site.baseurl}}/examples/img/fmt/image012.png' width='800' />
