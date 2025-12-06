---
layout: project
title: Torque Wrench Design & Analysis
description: CAD, FEM Analysis, & Design Project
technologies: [Autodesk Fusion, Ansys, MATLAB]
image: /assets/images/torque-wrench/TW_Icon.png
---

<p>As a final project for my junior year materials class, I was tasked to design a 3/8" drive torque wrench given the following constraints:</p>

<p>1) Attain at least 1.0 mV/V output at the rated torque of 600 in-lbf sustained for 10<sup>6</sup> cycles at the strain gauge location of my choosing</p>
<p>2) Safety factor of X = 4 for yield or brittle failure</p>
<p>3) Safety factor of X = 2 for crack growth from an assumed crack of depth 0.04 inches</p>
<p>4) Fatigue stress safety factor of X = 1.5</p>
<p>5) Material must be a steel, aluminum, or titanium alloy</p>

Using MATLAB, I developed a quick script to assist in running the hand calculations I would need to do in order to reach the design requirements. The material I ultimately ended up choosing was Low Alloy Steel, AISI 4140, Oil-Quenched & Tempered at 205C. The properties of this material used for the analysis is listed below:

<p>E (Young's Modulus) = 30.2 Mpsi</p>
<p>v (Poissons Ratio) = 0.29</p>
<p>Yield strength = 214 ksi</p>
<p>K<sub>IC</sub> = 33 ksi-in<sup>1/2</sup></p>
<p>Fatigue strength for 10<sup>6</sup> cycles = 90 ksi</p>

The dimensions I chose for the torque wrench to fulfill the design requirements were:

<p>Length from drive center to end of handle = 18 in</p>
<p>Width = 0.45 in</p>
<p>Thickness = 0.55 in</p>
<p>Distance from center of drive to strain gauge = 0.5 in</p>

These dimensions, paired with the material properties, gave me a safety factor for yield of 6.62, for crack growth of 2.57, and for fatigue stress of 2.78.

Using Fusion 360, I created the CAD model:

<div class="scroll-gallery">
  <img src="{{ '/assets/images/torque-wrench/CAD_Dimensions2.png' | relative_url }}" alt="CAD Dimensions 2">
  <img src="{{ '/assets/images/torque-wrench/CAD_Dimensions1.png' | relative_url }}" alt="CAD Dimensions 1">
  <img src="{{ '/assets/images/torque-wrench/OverallCAD.png' | relative_url }}" alt="CAD 3">
  <img src="{{ '/assets/images/torque-wrench/TW_Rendered.png' | relative_url }}" alt="Rendered CAD">
</div>


And using Ansys, I loaded the the wrench with a force of 33.33 lbf at the end of the handle to simulate the 600 in-lbf torque and constrained the drive to not displace.

![Photo of boundary conditions and load applied to FEM model]({{ "/assets/images/torque-wrench/Loads_BC.png" | relative_url }})

Running the analysis at element size of 0.06 inch gave the following results:

Normal strain contours:

![Photo of normal strain contours on wrench]({{ "/assets/images/torque-wrench/Normal_Elastic_Strain.png" | relative_url }})

Max principle stress contours:

![Photo of principle stress contours on wrench]({{ "/assets/images/torque-wrench/Max_Principle_Stress.png" | relative_url }})

And max normal stress, load point deflection, and strain at the strain gauge location:

![Photo of normal stress on wrench]({{ "/assets/images/torque-wrench/Max_Norm_Stress.png" | relative_url }})

![Photo of load point deflection of wrench]({{ "/assets/images/torque-wrench/Total_Deformation.png" | relative_url }})

![Photo of strain gauge probe]({{ "/assets/images/torque-wrench/Strain_Probe_Location.png" | relative_url }})

At the location of the strain gauge probe, the strain gauge read ~1035 microstrain, which matched quite well with my hand calculation of 1041 microstrain. The maximum stress of 75 ksi was experienced by the wrench at the point of contact where the drive meets the handle itself. This was contradictory to my hand calculations of a max stress of about 32 ksi, but not unsurprising. Given the my hand calculations did not take into account the drive itself, and instead analyzed only the stress on the handle, we can see that if you shifted to the location on the handle, the stress was indeed 32 ksi. As for the deflection of the load point, FEM analysis gave 0.566 inch in the x direction, while my hand calculations gave 0.514 inch in the x. Overall, the results where quite close!

Note, however, that I did run the analysis previously with less nodes and an element size of 0.25, and the results of that contained a larger error in terms of how they compared to my hand calculations. I kept refining the mesh until I got to where I believed was a decent enough result and the error wasn't big enough to cause great concern that I may have screwed up in arithmetic, modeling, or elsewhere.

Finally, the reading of 1035 microstrain gives a torque wrench sensitivity of 1.035 mV/V (which fulfills the design requirement!) using a 1/2 bridge configuration. After leafing through some strain gauge catalogs, the ones I decided would fit nicely onto my design were these <a href="https://www.dwyeromega.com/en-us/linear-strain-gages/SGD-LINEAR1-AXIS/p/SGD-3-120-LY11">SGD precision linear strain gauges</a>. With a carrier dimension of 0.307 in x 0.150 in, they would have no problem fitting onto my 0.55 inch thick wrench handle.
