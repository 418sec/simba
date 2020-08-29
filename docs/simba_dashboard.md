# Interactive Data Visualizations in SimBA

## Overview

Once analyses have been performed in SimBA, users may have the need to visualize the results of the classifiers and easy, interactive, paths towards exporting the parts of the datasets of interest into third-party statistical and graphing applications and scripts. For this SimBA has a built-in interactive graphical dashboard written in [Plotly](https://plotly.com/) and [Dash](https://github.com/plotly/dash) that allows users to inspect **huge** (or not so huge) data-sets, and create their own new data-sets (through drag-and-drop, mouse-clicks, zoom-functions and more) without havin to write any costum code.  In this tutrial, we outline and explain the different functions within the SimBA Plotly Dashboard, and how we can utilize the dashboard for analyzing and exporting a larger data-sets (attack classifications in 438 videos) to third-party applications.  The SimBA dashboard was written by [Sophia Hwang](https://github.com/sophihwang26) and [Aasiya Islam](https://github.com/aasiya-islam).

<p align="center">
<img src="https://github.com/sgoldenlab/simba/blob/master/images/Dash_logo.png" />
</p>



### PART 1: Generating a SimBA Dashboard file and opening the Dashboard

1. To open the SimBA Dashboard, we first need to create a single *collated* dashboard dataset file in SimBA. This single dashboard file is ahighly compressed [HDF dataframe container](https://en.wikipedia.org/wiki/Hierarchical_Data_Format) that contains all your data that we want to be able to play around with within our Plotly dashboard. 

>Note: This dataframe container also provides an efficient way of sharing data, and can be opened in SimBA at any location without any other project files being required.

2. To generate this file, begin by loading your project in SimBA. In the main SimBA console window, click on `File` and `Load project`. In the **[Load Project]** window, click on `Browse File` and select the `project_config.ini` that belongs to your project, and then click `Load Project`. 
 
3. Navigate to the `Visualizations` tab, and you should see the following menu on the right hand side, titled `Plotly / Dash`. 

![](https://github.com/sgoldenlab/simba/blob/master/images/Dash_1.JPG "Plotly Graph Features")

4. The first part of this menu contains 5 tick-box menus. We will use these tick-box menus to specify *what* data we want to have contained within our dashboard file (e.g., what data we want to be able to be able to handle interactively). In this tutorial we will go ahead and tick `Sklearn results`, `Time bin analyses`, `Probabilities`.

* ```Sklearn results```: 

* ```Time bin analyses```:

* ```Probabilities```:

* ```Severity analyses```:

* ```Entire Machine Classification dataframes```:

Once we have ticked the tick-boxes for the data we want to include, go ahead and click on `Save SimBA/Plotly dataset`. This will generate a single, highly-compressed, `H5` dataframe container, which will be located in the `project_folder/logs` directory. The filename, of this file, will be date-time stamped, and be named something like this: `SimBA_dash_file_20200829090616.h5`.

>Note: This is a highly compressed file. In this example tutorial, the 438  have been compressed into a very-much sharable 32MB that contain all the data indicated by the tick boxes selected in the SimBA GUI 'Plotly /Dash` submenu:

![](https://github.com/sgoldenlab/simba/blob/master/images/Dash_2.JPG "Plotly Graph Features")

Depending on the number of videos that the user has within the project, this step may take some time. You can follwo the progress in the Main SimBA terminal window. 

### PART 2: Opening the SimBA Dashboard file

1. To open and inspect a `SimBA/Plotly dataset` h5 file, we will use the two menus circled in blue below:

![](https://github.com/sgoldenlab/simba/blob/master/images/Dash_4.JPG "Plotly Graph Features")

* ```SimBA Dashboard file (H5)```: In this menu, click on 'Browse File` and select the dataframe H5 dataframe container you wish to use within the Dashboard interface. 
>**Important**: The selected `SimBA Dashboard file` .H5 file does not have to have been generated within the currently opened project. The selected `SimBA Dashboard file` .H5 can have been generated within the SimBA interface on any anywhere, within any project (regardless of pose-estimation tool, tracked body-parts, and the number and specific classifier used). 

* ```SimBA Groups file (CSV)```: **(OPTIONAL)** If we want to plot and compare group-level metrics, then we need to tell SimBA and [Plotly](https://plotly.com/) which videos belong to which group. The most straightforward way of doing this is to create our own CSV file, where each column represents each group, and each row represents each video belonging to that group, and feed the information in this CSV file into the Dashboard. For the current tutorial example, with 438 videos, I have created a CSV file example that can be downloaded [HERE](https://github.com/sgoldenlab/simba/blob/master/misc/SimBA_Dash_tutorial_Group_information.csv). This file contains **two columns** representing the two groups (males v. females), with one row for each video in each group. For this example, we have 337 videos in the **male** group, and 101 videos in the **female** group. 

![](https://github.com/sgoldenlab/simba/blob/master/images/Dash_5.JPG "Plotly Graph Features")

Once you have selected your files `SimBA Dashboard file` (.. and the optional SimBA Groups file), go ahead and click on the `Open SimBA / Plotly dataset` button. A little time will pass for the application to load, but eventually a window looking similar to this (on the left in the image below) should pop open. Alternatively, if you feel like this interface is finicky to work with, you can also navigate to the the IP `127.0.0.1:8050` adress in your webbrowser (tested with Google Chrome webbrower) and you should see the native Dash interface (on the right in the image below; **click on the images below to enlarge**): 

<img src="https://github.com/sgoldenlab/simba/blob/master/images/Dash_6.JPG" width="425"/> <img src="https://github.com/sgoldenlab/simba/blob/master/images/Dash_7.JPG" width="425"/>


### PART 3: Dashboard overview




















### Part 1: Graph Overview


![](https://github.com/sgoldenlab/simba/blob/master/images/overall_bar.JPG "Graph Interface Overview")

Here we feature the plots visualizing data for both a group mean comparison with groups selected on the Data dashboard to the side, as well as an individual video 
comparison amongst a single group, also selected from the side. In the given scenario...


### Part 2: Graph Types and Features


![](https://github.com/sgoldenlab/simba/blob/master/images/probability2.JPG "Probability Graph Example")

* Probability Graphs: These graphs plot the probability of the behavior as a continuous line graph per video frames as default. The x-axis can be changed [here](https://github.com/sgoldenlab/simba/blob/master/docs/plotly_dash.md#additional-properties) to visualize the probability vs. time in seconds for each video instead. 
By hovering over each of the line graphs, we can see the individual data points for each trace as a coordinate of (frames/seconds, probability) as shown below. You can 
also change this [here](https://github.com/sgoldenlab/simba/blob/master/docs/plotly_dash.md#part-3-plot-settings) to compare data points betwen traces while hovering. 


![](https://github.com/sgoldenlab/simba/blob/master/images/bargraph.JPG "Bar Graph Example")

* Categorical Bar Graphs: These graphs plot specific features of the behavior as designated [here](https://github.com/sgoldenlab/simba/blob/master/docs/plotly_dash.md#select-behavior-to-plot), with the means of the feature and standard error of mean displayed for each group in the Group Means comparison, and the feature for each video in the Individual Groups comparison. The means and feature count can be displayed on the top of each of the bars respectively, and also displayed upon hovering over it with the group/video name and feature count, and for the group mean comparison, the standard error will be displayed as +/- after the mean value feature count, as shown below. The standard error bars can be configured to be displayed traditionally as both above and below the mean, or just one way in either direction [here](https://github.com/sgoldenlab/simba/blob/master/docs/plotly_dash.md#bar-graph-properties). 

* Legends: 


### Part 3: Plot Settings

With each of the plots, Plotly features many settings to better visualize the graphs and compare data as well as download the plots as images to your own computer.

![](https://github.com/sgoldenlab/simba/blob/master/images/probability_features.png "Plotly Graph Features")

* ```Download Plot```: 

* ```Zoom```:

* ```Pan```:

* ```Box Select```:

* ```Lasso Select```:

* ```Zoom In```:

* ```Zoom Out```:

* ```Autoscale```:

* ```Reset Axes```:

* ```Toggle Spike Lines```:

* ```Show closest data on hover```:

* ```Compare data on hover```:


# PART 2: Plot Dashboard


![](https://github.com/sgoldenlab/simba/blob/master/images/data_tab.JPG "Data Tab")
![](https://github.com/sgoldenlab/simba/blob/master/images/graphsettings_tab.JPG "Graph Settings Tab")
![](https://github.com/sgoldenlab/simba/blob/master/images/downloadsettings_2.JPG "Download Settings Tab")


## Part 1: Data

### Select Behavior to Plot

In the following dropdown menus, we can select which type of behavior to visualize for our experiment along with the category of data
and the specific features of the data sets with our given scenario:

![](https://github.com/sgoldenlab/simba/blob/master/images/plot_behavior.JPG "Plot Behavior Selection")

* ```Behaviors:``` In this dropdown menu, we can select between `Attack` and `Sniffing` behaviors.

* ```Category:``` In this dropdown menu, we can select between `VideoData` which plots the probability of the behavior vs. frames,
`SklearnData`, which plots different results of the features analyzed by Sklearn, and `TimeBins`, which plots the prevalence of the features
for each time bin.

* ```Feature:``` In this dropdown menu, we can select the features or statistics to plot for each respective category. VideoData 
plots probability in the form of a line graph. Sklearn plots seven statistics for each behavior with the mean and standard error represented
in a bar graph. Here we can select from ` bout events`, `total events duration (s)`, `mean bout duration (s)`, `first occurrence (s)`, `mean interval (s)`,
and `median interval (s)`. TimeBins plots the mean of the prevalence of these respective features for each time bin selected. 

### Plotting Group Means

With this feature, we can select different combinations of groups of videos to plot and compare the data of each to one another in the Group Means plot.


![](https://github.com/sgoldenlab/simba/blob/master/images/group_means.JPG "Group Selection")


* ```Select Group(s):``` In this dropdown menu, we can select the different groups to add to our plot, and to remove a group
from the selection we can click the `X` next to the group name.

For the VideoData continuous probability data, we can check the `Show Total Mean` box to add a trace to represent the total mean of the group data.


### Plotting Individual Groups

With this feature, we can select a single group of videos to compare the data of each video in the group to one another in the plot.


![](https://github.com/sgoldenlab/simba/blob/master/images/individual_groups.JPG "Individual Group Selection")


* ```Select Single Group to Plot:``` In this dropdown menu, we can select a single group from out list of groups to plot.

* ```Select Video(s):``` In this dropdown menu, we can select which videos we would like to see represented and compared to one another.

For the VideoData continuous probability data, we can check the `Show Group Mean` box to add a trace to represent the mean of the included videos
in the group data.


## Part 2: Graph Settings


### Color Properties


Here we can choose the color properties for both our groups and individual videos for the graphs. 


![](https://github.com/sgoldenlab/simba/blob/master/images/group_colors.png "Group Color Selection")


For the group color selection, simply click on the colored circle underneath the name of the group which opens a color picker in which you can choose 
a specific color and shade for each group to be represented with. Once all the colors have been selected for the groups, you can click the `UPDATE COLORS`
button to set the selection.


![](https://github.com/sgoldenlab/simba/blob/master/images/colorscales.png "Colorscale Selection")


* ```Colorscale Gradient for Individual Videos```: For the individual video color selection, you have the option of choosing from different pre-set or custom
colorscales which can be found in the dropdown menu by clicking on the sample colorscale. Here you can choose the type of colorscale depending on what best
suits your data visualization needs, as well as setting the number of swatches needed to potentially correspond to the number of videos being visualized.
The colors will automatically update upon the selection of the colorscale gradient, and to return to the page, you can click the colorscale already displayed
on the page, and to update the colors with a new combination of colors from the colorscale gradient, you can click on `UPDATE COLORS` to change them.


### Probability Graph Properties

Here we can select the properties for our different probability graphs, both the multi-group and individual group video graphs. Depending on whether you would
like the graph visualized with video frames or seconds as the x-axis, as set [here](https://github.com/sgoldenlab/simba/blob/master/docs/plotly_dash.md#additional-properties), we can custom set the axes below.


![](https://github.com/sgoldenlab/simba/blob/master/images/probability_propertiesJPG.JPG "Probability Graph Properties Selection")


For visualizing the multi-group graphs in frames, you can set the minumum and maximum number of frames you would like visualized, respectively, and similarly 
if you're viewing your graph in seconds, you can set the numbers below. Once set, click `SUBMIT` to view the changes. To reset the axes, click `RESET AXES` 
to view the default minima and maxima values. The same steps can be applied for the individual group and video viewing frames/seconds below.


### Bar Graph Properties

For the bar graphs, as represented by the Sklearn and TimeBins data with our given scenario, we can specify what type of error bars we would like visualized,
being either traditional error bars or just one way around the mean value. The standard error values can also be seen along with the mean if you hover over the
bar itself on the graph.


![](https://github.com/sgoldenlab/simba/blob/master/images/bar_properties.JPG "Bar Graph Properties Selection")


* ```Error Type:```: The default will display traditional error bars as `Both` but to display them only one way, select either the `Above` or `Below` options to
view the bars either above or below the bar, respectively. 


### Additional Properties

Here we can change different graph properties such as the background display, font, and axes if plotting probability data, custom setting the graph titles.


![](https://github.com/sgoldenlab/simba/blob/master/images/additionalproperties_3.JPG "Additional Properties Selection")


* ```Graph X Axis in Seconds```: For the probability graphs, you can choose to display the x-axis in units of seconds instead of frames by checking this box

* ```Show Grid Lines```: The white grid lines in the background of the plot will be displayed by default, but to remove them you can uncheck this box

* ```Show Background```: A light blue background will displayed behind the graph by default, but to remove this to display a white background you can check this box

* ``` Group Means Title``` & ```Individual Videos Title```: For the Group Means graph, the plot title will be the Feature + "Group Means" by default (e.g. 
"Probability_Attack Group Means"), and for the Individual Videos graph, the plot title will be the Feature + Group by default (e.g. Probability_Attack Group_1_test).
To set your own plot titles, type in your new titles into the respective boxes and click `SET` to change them.

* ```Choose Font```: The default font displayed for the page is Verdana, but this can be changed using the dropdown menu as you can select from the following fonts:
Verdana, Helvetica, Calibri, Arial, Arial Narrow, Candara, Geneva, Courier New, and Times New Roman.

* ```Font Size```: The default font size displayed for the page is 12 pt, but this can also be increased or decreased by either clicking on the up or down arrows, respectively,
or by highlighting and typing in a new font size in the display box. 


## Part 3: Download Settings


### CSV Export 


You can download the data for each of the respective Group Means and Individual Videos plots as CSV files and save to your computer. 

![](https://github.com/sgoldenlab/simba/blob/master/images/csv_export.JPG "CSV Export Settings")


* ```Enter csv file name```: To designate a CSV file name, type your desired file name into the box and click either the `MEANS.CSV` or the `VIDEOS.CSV` buttons to select
which data you would like to download as a CSV. Once downloaded, it should display a message saying that you have "Downloaded csv as file_name.csv", as seen below. It will download into the same folder from which the _____.

![](https://github.com/sgoldenlab/simba/blob/master/images/csv_download.JPG "CSV Downloaded Message")

For the Group Means data, the CSV will display the probability group data for each frame and the mean for the groups, or the feature means for each group with standard errors for the categorical data. For Individual Videos data, the CSV  will display the data in the same format with the data represented for each video instead of for each group. Examples can be seen below: 

![](https://github.com/sgoldenlab/simba/blob/master/images/probability_means_csv.JPG "CSV Probability Means Example")
![](https://github.com/sgoldenlab/simba/blob/master/images/probability_videos_csv.JPG "CSV Probability Videos Example")
![](https://github.com/sgoldenlab/simba/blob/master/images/sklearn_means_csv.JPG "CSV Sklearn Means Example")
![](https://github.com/sgoldenlab/simba/blob/master/images/sklearn_videos_csv.JPG "CSV Sklearn Videos Example")

### Image Export


You can also download the images for each of the plots and customize dimensions, name, and file extension. 


![](https://github.com/sgoldenlab/simba/blob/master/images/imageexport_2.JPG "Image Export Settings")


* ``` Enter image dimensions (px)```: The default image dimensions in pixels are 500 X 700, but you can change the height and width respectively by typing in a new value or by
clicking the up and down arrows to increase or decrease the dimensions.

* ```Enter image name```: You can type your desired image download name by typing it in the box here.

* ``` File Extension```: The default file download extension is .SVG but you can also choose the file extension by selecting from the dropdown menu here and save the image as an .SVG, .PNG, or as a .JPEG.

Once you've fixed the download settings, click the `SET DOWNLOAD SETTINGS` button at the bottom, and to actually download the image, [go to the plot settings](https://github.com/sgoldenlab/simba/blob/master/docs/plotly_dash.md#part-3-plot-settings) and click "camera" button as `Download plot` which should download the image to the folder of your choice on your computer.