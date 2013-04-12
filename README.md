#FSRS LabVIEW

The license for the code contained in this repository is contained in the included `license.txt` file.

Contained in this repository are [LabVIEW](www.ni.com/labview/) VI's designed to run a femtosecond stimulated Raman instrument ([DOI:10.1039/c2cp23468h](http://dx.doi.org/10.1039/c2cp23468h) and [10.1021/jp400369b](http://dx.doi.org/10.1021/jp400369b)).

##FSRSv1.vi
This is the main "driver" program, it will call and display subVI's that perform certain tasks, such as the `daq_scan_sub3.vi` which will read in data from the lock-in amplifier (SR810). In addition, this program will run the main FSRS experiment, it will save a data file for each time delay and ground state acquisition, in each file there will be the calculated Raman gain spectrum (this is calculated on a shot-to-shot basis), the Raman pump on probe spectrum and the Raman pump off spectrum.

_**NOTE:** The `DataGrabber2.vi` and the `MoveNano.vi` will need to be rewritten for the particular camera and delay stage used in the instrument._

###daq_scan_sub3.vi
This program, which can be called from the main program (`FSRSv1.vi`) 
will read in data from the lock-in amplifier. This is useful to measure the instrument response function and transient absorption. The user can select a starting time, an initial time step, a doubling time and an ending time. The time steps are calculated by `DAQTimes.vi`.

_**NOTE:** the `SR810_Read.vi` will need to be rewritten for the particular lock-in amplifier used._

###DAQTimes.vi
Calculates semi-exponential times for the `daq_scan_sub3.vi`. The vi takes a start time, an initial time step, a "doubling time" and a final time. From these parameters it calculates a set of timepoints in which the time step is doubled for every number of steps taken resulting in a set of time points that is semi-exponential.

###DataGrabber2.vi
Tells the camera ([Princeton Instruments, PIXIS 100F](http://www.princetoninstruments.com/products/speccam/pixis/)) how many frames to take and then returns those frames as a data matrix. Due to LabVIEW's limitations on the sizes of matrices, the number of frames is limited to slightly less than 10,000. Actually, this VI tells the camera to take 10 more frames than the user specifies, which it then discards. This is necessary as the beam shutters take about 10 ms to open.

_**NOTE:** This VI is specific to the PIXIS 100F and will need to be replaced for a different camera._

###DataProcessor4.vi
Takes the output from `DataGrabber2.vi` and calculates the Raman gain, on a shot-to-shot basis, and averages these together. It also averages the probe spectra (both Raman pump on and off). These averages are output.

###DetectorXCSub2.vi
Records the dispersed cross correlation so that the chirp of the probe beam can be accurately characterized. We use the optical Kerr effect in the solvent in our sample cell to generate our cross correlation. This is essentially a FROG measurement of the probe, using the actinic pump as the gate. The program has a built-in fitting routine that will fit the time trace of each pixel to a gaussian (representing the IRF) and graphically displays the center, width and amplitude. Also shown are instructions as to whether or not more or less prism should be added to the probe path in the prism compressor.

###ExSubCloseFileImageMulti.vi
This VI corrects an error in `ExSubCloseFileImage.vi` provided by [R 
Cubed](http://www.rcubedsw.com/) the providers of the LabVIEW drivers 
for the PIXIS camera.

###GainCalculator.vi
Calculates the Raman gain based on two cursors placed on the displayed Raman gain curve.

###MoveNano.vi
Moves the delay stage (Melles Griot, Nanomotion II).

_**NOTE:** This VI is specific to the Nanomotion II and will need to be replaced for a different stage._

###TimeLeft.vi
Simple VI that takes as inputs two timers (which should be placed on 
either side of the process that you'd like to time) and the number of 
interations and the current iteration for the `for` loop. It then 
outputs a string variable that shows the amount of time left for the 
`for` loop to complete.

###Winspec.vi
Collects and displays Raman gain spectra and individual probe spectra continuously until the user presses the stop button. The shot-to-shot stability is measured showing a histogram of the means of each individual Raman gain spectra.

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/david-hoffman/FSRS-LabVIEW/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/21304a2785cadd7b2eac4cfb5d873d7d "githalytics.com")](http://githalytics.com/david-hoffman/FSRS-LabVIEW)
