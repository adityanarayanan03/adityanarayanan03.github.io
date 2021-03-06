---
title: "V5 Serial Plotter"
author_profile: false
---


### About The V5 Serial Plotter

Serial monitors and serial plotters are some of the most useful tools for debugging programs and obtaining data on program performance over time. For the VEX Cortex/VEX V5 system, a serial monitor is readily available within the coding environment in both PROS and VEXCode; however, both environments lack a serial monitor capable of graphically displaying data collected from the Cortex/Brain. Currently, the most widely used method of graphing data from the brain is to save said data to an SD card as a .csv file and plot using plotting tools such as Excel or Google Sheets. The V5 Serial Plotter aims to provide a more efficient process by creating a serial plotter with a graphical interface for creating easily customizable graphs with a permanent link to the Cortex/V5 Brain.

### Setting up the V5 Serial Plotter

1. Download the plotter. For plotter functionality, only the latest release folder is required, but standard GitHub allows only for zipping of a full repository. Compressing and Unzipping the full repository will work perfectly, but so will using tools such as DownGit to download specific folders. 

2. At this point, the file LatestRelease/plotter.exe should be run to run the V5 Serial Plotter. You can run this file through the command line using ```.\plotter.exe``` in the proper directory, running the file through file explorer, or create a shortcut for plotter.exe somewhere easily accessible, such as your Desktop (recommended method). While only the plotter.exe file must be run, **do not remove the plotter.exe file from the LatestRelease directory**. Plotter.exe depends on many packages found in LatestRelease and LatestRelease/lib.

### Setting up your VEX system for use with the Plotter

The plotter essentially scans the serial port from the Brain searching for outputted data of the correct form. Data is published to the serial link using the standard printf() method that will print to the PROS or VEXCode terminal.

1. At the point in your program at which you are ready for the serial plotter to begin collecting data, the string ```{START}``` should be sent to the terminal.
2. When you are ready to end collection, the string ```{STOP}``` should be sent. 
3. Each datum sent to the plotter should be sent on a new line, packed into the form ```{x_axis, y_1, y_2, etc.}```. Make sure to include the curly braces as these are the delimiters for data interpretation. The recommended way to do this is to create a function such as the one in Examples/PROS_Code/include/globalFunctions.h that will pack a vector of data into the proper form. A non modular way to achieve the same result is included below. 
4. An example of non-modular code that can create the intended results is:
```C++
printf('%s \n', '{START}');
while (i=1; i<=5; i++){
	printf('%s', '{');
	printf('%f', i);
	printf('%s', ',');
	printf('%f', i * 2.0);
	printf('%s', ',');
	printf('%f', i * 5.0);
	printf('%s \n', '}');
	delay(1000);
}
printf('%s \n', '{STOP}');
```
The result of the above code, if viewed in the PROS/VEXCode terminal, is
```
{START}
{1, 2, 5}
{2, 4, 10}
{3, 6, 15}
{4, 8, 20}
{5, 10, 25}
{STOP}
```
The above code and ouput will generate 
![Demo Image](https://github.com/adityanarayanan03/V5SerialPlotter/blob/GUI/Examples/ReadMeExample.pdf)

### Using the V5 Serial Plotter

1. Connect to the V5 Brain through the Brain's micro USB port.
2. Use either the PROS CLI or Device manager to find the name of the V5-User port (Ex. 'COM12') that represents the connection to the V5 Brain. 
3. Run the plotter.exe file through your preferred method. No other programs can be using the serial connection at the same time as the V5 Serial Plotter. Make sure any open PROS or VEXCode terminals are closed. 
4. Select the name of your COM port. 
5. Select your test stop-type. This can be either stop character based (default, no action required), or time-based (select duraction-based collection, and input a value in seconds for the duration of collection).
6. If you would like to save your plot automatically, select the save Plot check-box. The default path is the location of plotter.exe. To change the Save Path, select File --> Edit Save Path. The file will be named with the date and time it was created by default. As of now, you are unable to change this using the V5 Plotter itself, and you must rename files through your file explorer. This may be changed in a future update. 
7. Hit run to start the collection. The program will wait 30 seconds to receive the start character, giving the user ample time to run the proper program from the V5 brain and send the start character. 
8. When data collection ends, the plotter will display the created plot. 
