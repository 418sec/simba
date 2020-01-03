# SimBA Tutorial:

To faciliate the initial use of SimBA, we provide several use scenarios. We have created these scenarios around a hypothetical experiment that take a user from initial use (completely new start) all the way through analyzing a complete experiment and then adding additional experimental datasets to an initial project.

All scenarios assume that the videos have been [pre-processed](https://github.com/sgoldenlab/simba/blob/master/docs/tutorial_process_videos.md) and that [DLC behavioral tracking .CSV dataframes](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_DLC.md) have been created.

# **Hypothetical Experiment**:
Three days of resident-intruder testing between aggressive CD-1 mice and subordinante C57 intruders. Each day of testing has 10 pairs of mice, for a total of 30 videos recorded across 3 days. Recordings are 3 minutes in duration, in color, at 30fps.

# **Scenario 2**: Using a classifier on new experimental data...
In this scenario you have either (i) generated a classifier yourself which performance you are happy with. For example, you have followed [Scenario 1](https://github.com/sgoldenlab/simba/edit/master/docs/Scenario1.md), and generated the classifier for "Behavior that Will Get a Nature Paper (Behavior BtWGaNP)" and its working well. Or, you have (ii) got a behavioral classifier generated somewhere else, and now you want to use the classifier to score Behavior BtWGaNP on your experimental videos. For example, you have downloaded the classifier from [this](https://osf.io/d69jt/) website.

## Part 1: Create a new project

We will need start with a project directory tree that does not contain any other data than the data we want to analyze. If you are coming along from [Scenario 1](https://github.com/sgoldenlab/simba/edit/master/docs/Scenario1.md), you will have a project tree already. However, this project tree contains the files used to create the BtWGaNP classifier: if you look in the subdirectories of the `project_folder/csv/input` directory, you will see the 19 csv files we used to generate the project. If we continue using this project, SimBA will see these csv files and analyze these files in addition to your Experimental data. Thus, the option is to remove these files from the subdirectories of our project, or alternatively, to [create a new](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#part-1-create-a-new-project) project that only contains the data from our Experiment. In this Scenario, we will create a [new project](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#part-1-create-a-new-project). 

To create a new project with your experimental data, follow the instructions for creating a new project in either of these tutorials: [1](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#part-1-create-a-new-project-1), [2](https://github.com/sgoldenlab/simba/blob/master/docs/tutorial.md#step-1-generate-project-config). Instead of using your pilot data as indicated in the tutorial from [Scenario 1]((https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#part-1-create-a-new-project-1), use the data, videos for the Experiment you want to analyze.

**Important**: In the final (*Step 4*) of the tutorials for creating a new project, we extract the frames from the imported videos. Having the frames is only necessery if you wish to visualize the predictive classifications generated in this Scenario. If you do not want to visualize the machine predictions you can skip this step. However, we recommend that you at least visualize the machine predictions using a few videos to gauge its accuracy. How to visualize the machine predictions is descriped in [Part 5] of this tutorial. 

## Part 2: Load project and process your tracking data

In Part 1, we created a project that contains your experimental data. To continue working with this project, we **must** load it. To load the project and process your experimental data, follow the instructions for **Step 1-5** in either of these tutorials: [1](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#part-1-create-a-new-project-1), [2](https://github.com/sgoldenlab/simba/blob/master/docs/tutorial.md#step-1-generate-project-config]. In this Scenario you can **ignore Steps 6-7 of the Load Project tutorials**. **Step 1-5**, which we need to compplete, and deals with [correcting outliers in the tracking](https://github.com/sgoldenlab/simba/blob/master/misc/Outlier_settings.pdf), and [extracting features](https://github.com/sgoldenlab/simba/blob/master/misc/Feature_description.csv) from our data.

## Part 3: Run the classifier on new data

At this point we have Experimental data, that has been corrected for outliers and with features extracted, and we want to predict behavior BtWGaNP in these videos. If you have downloaded the predictive classifier, we will also include information on how you can use those files to predict behavior BtWGaNP in these videos. 

1. In the Load Project menu, navigate to the **Run Machine Model** tab and you should see the following window. 

<img src="https://github.com/sgoldenlab/simba/blob/master/images/runrfmodel.PNG" width="343" height="132" />

2. Click on `Model Selection`. The following window, containing the classifier names that were defined when you created the project, will pop up.

<p align="center">
  <img width="312" height="256" src="https://github.com/sgoldenlab/simba/blob/master/images/rfmodelsettings.PNG">
</p>

3. Click on `Browse File` and select the model (*.sav*) file associated with the classifier name. If you are following along from [Scenario 1](https://github.com/sgoldenlab/simba/edit/master/docs/Scenario1.md), the *.sav* file will be saved in your earlier project, in the `project_folder\models\generated_models` directory. You can also select an *.sav* file located in any other location. For example, if you have downloaded a random forest model (i.e., from [our OSF repository](https://osf.io/d69jt/), you canspecify the path to that file here. 

4. Once the path has been selected, click on `Set Model` to save the path.

>**Note**: In the real world you may want want to run mutiple classifiers on each video, one for each of the behaviors you are intrested in. In such a scenario you have defined mutiple predictive calssifier names when you created the project. Each one will be displayed in the `Model Selection`, and you can specify a different path to a different *.sav* file for each of them. 

>**Important**: Each random forest expects a specific number of features. The number of features are detemined by the number of body-parts tracked in pose-estimation in DLC. *For example*: if the input dataset contains the location of three body parts, then a fewer number of features can be calculated than if 8 body parts were tracked. This means that you will get an error if yiu run a random forest model *.sav* file that has been generated using 8 body parts on a new dataset that contains 3 body parts.

5. Fill in the `Discrimination threshold` and click on `Set` to save the settings.

- `Discrimination threshold`: This value represents the level of probability required to define that the frame belongs to the target class and it accepts a float value between 0.0 and 1.0.  In other words, how certain does the computer have to be that behavior BtWGaNP occurs in a frame, in order for the frame to be classified as containing behavior BtWGaNP? For example, if set to 0.50, then all frames with a probability of containing the behavior of 0.5 or above will be classified as containing the behavior. For more information on classification theshold, click [here](https://www.scikit-yb.org/en/latest/api/classifier/threshold.html). In this Scenario, we suggest using a 0.5 discrimination threshold. *Note*: You can titrate the discrimination threhold to best fit your data. Decreasing the threshold will predict that more behaviors are occuring, while increasing the threshold will predict that less behaviors as occuring.

6. Fill in the `Minimum behavior bout length` and click on `Set` to save the settings.

- `Minimum behavior bout length (ms)`:  This value represents the minimum length of a classified behavioral bout. **Example**: The random forest makes the following predictions for behavior BtWGaNP over 9 consecutive frames in a 50 fps video: 1,1,1,1,0,1,1,1,1. This would mean, if we don't have a minimum bout length, that the animals enganged in behavior BtWGaNP for 80ms (4 frames), took a brake for 20ms (1 frame), then again enganged in behavior BtWGaNP for another 80ms (4 frames). You may want to classify this as a single 180ms behavior BtWGaNP bout, rather than two separate 80ms BtWGaNP bouts. If the minimum behavior bout length is set to 20, any interruption in the behavior that is 20ms or shorter will be removed and the example behavioral sequence above will be re-classified as: 1,1,1,1,1,1,1,1,1 - and instead classified as a single 180ms BtWGaNP bout.

7. Click on `Run RF Model` to run the machine model on the new experimental data. You can follow the progress in the main SimBA terminal window. A message will be printed once all the behaviors have been predicted in the experimental videos. *Note*: New csv files, that contain the predictions, are saved in the `project_folder/csv/machine_results` directory. 

## Part 4:  Analyze Machine Results

Once the classifications have been generated, we want to create descriptive statistics for the behavioral predictions. For example, we might like to know how much time the animals in each video enganged in behavior BtWGaNP, how long it took for each animal to start enganging in behavior BtWGaNP, how many bouts of behavior BtWGaNP did occur in each video contain and what where the mean/median interval and bout length. We may also want some descriptive statistics on the movements, distances and velocities of the animals. If applicable, we can also generate an index on how 'severe' behavior BtWGaNP was. To generate such descriptive statistics summaries, click on the `Run machine model` tab in the `Load project` menu. In the sub-menu `Analyze machine results`, you should see the following menu with three buttons:

<img src="https://github.com/sgoldenlab/simba/blob/master/images/analyzemachineresult.PNG" width="331" height="62" />

1. `Analyze`: This button generates descriptive statistics for each predictive classifier in the project, including the total time, the number of frames, total number of ‘bouts’, mean and median bout interval, time to first occurrence, and mean and median interval between each bout. Clicking the button will run the desciptive statistics on all the csv files in `project_folder/csv/machine_results` directory. A date-time stamped output csv file with the data is saved in the `/project_folder/log` folder. 

2. `Analyze distance/velocity`: This button generates descriptive statistics for mean and median movements and distances between animals. Clicking the button will run the desciptive statistics on all the csv files in `project_folder/csv/machine_results` directory. A date-time stamped output csv file with the data is saved in the `/project_folder/log` folder. 

3. `Analyze severity`: This button and analysis is only relevant of your behavior can be graded on a scale ranging from mild (the behavior occurs in the presence of very little body part movements) to severe (the behavior occurs in the presence of a lot of body part movements). For instance, attacks could be graded this way, with 'mild' or 'moderate' attacks happening when the animals aren't moving as much as they are in other parts of the video, while 'severe' attacks occur when both animals are tussling at full force.  This button and code calculates the ‘severity’ of each frame classified as containing the behavior based on a user-defined scale. **Example:** the user sets a 10-point scale. One frame is predicted to contain an attack, and the total body-part movements of both animals in that frame is in the top 10% percentile of movements in the entire video. In this frame, the attack will be scored as a 10 on the 10-point scale. Clicking the button will run the 'severity' statistics on all the csv files in `project_folder/csv/machine_results` directory. A date-time stamped output csv file with the data is saved in the `/project_folder/log` folder.

Congrats! You have now used machine models to classify behaviors in new data. To visualize the machine predictions by rendering images and videos with the behavioral predictions overlaid, and plots describing on-going behaviors and bouts, proceed to *Part 5* of the tutorial below. 

## Part 5:  Visualizing machine predictions

In this part of the tutorial we will generate visualizations of features and machine learning classification results. This includes images and videos of the animals with prediction overlays, gantt plots, line plots, paths plots and data plots. In this step the different frames can also be merged into video mp4 format. Not that generating images and videos of the animals with prediction overlays requires the frames for the videos to have been generated in **Part 1** of this tutorial. Also note that the rendering of images and videos can take up a lot of hardrive space, if your videos are recorded at high frame rates, are long, or high resolution. One suggestion, if you are short on harddrive space, is to only generate frames for a few select videos (see below).  

1. In the `Load Project` menu, click on the `Visualization` tab and you should see the following screen. 

<img src="https://github.com/sgoldenlab/simba/blob/master/images/plotsklearn.PNG" width="1246" height="380" />

2. **Visualize predictions**. On the left of the `Visualization` menu, there is a sub-menu with the heading `Sklearn visualization`. This button grabs the frames of the videos in the project, and draws circles at the location of the tracked body parts, the convex hull of the animal, and prints the behavioral predictions on top of the frame together with the predicted time spent enganging in the behavior. 

*Note I*: Code will run through each csv file in your `project_folder\csv\machine_results` directory, find a matching folder of frames in your `project_folder\frames\input` directory, and generate new frames in the `project_folder\frames\output\sklearn_results` directory, contained within a new folder named after the video file. If you would like to generate visualization for only a select csv file, remove the files you want to omitt from visualizing from the `project_folder\csv\machine_results` directory. For example, you can manually create a temporary `project_folder\csv\machine_results\temp` directory and place the files you do **not** want to visualize in this temporary folder. 

*Note II*: If you do not want to create any further visualizations (e.g., gantt plots, path plots, data plots, or distance plots), you can stop at this point of the tutorial. However, if you want to create a video from the frames generated in the `project_folder\frames\output\sklearn_results` directory, you can do so by using the [SimBA tools menu](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md) and the [`Merge images to video` tool](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#merge-images-to-video). You could use the [`Merge images to video` tool](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#merge-images-to-video) to generate a video like the follwoing [example](https://www.youtube.com/watch?v=lGzbS7OaVEg&feature=youtu.be)

3. **Generate gantt plots**. In the `Visualization` menu, and the sub-menu `Visualizations`, use the first button named `Generate gantt plot` to create a gantt plot (a.k.a, horixontal bar chart or harmonogram) displaying the occurance, length, and frequencies of bouts of the behavior of intrest. These charts are similar to the ones generated by [popular propriatory software for behavioral analysis](https://www.noldus.com/the-observer-xt/data-analysis), and can look like this when generated through SimBA: 

<img src="https://github.com/sgoldenlab/simba/blob/master/images/gantt_plot.gif" width="300" height="225" />

*Note*: Code will run through each csv file in your `project_folder\csv\machine_results` directory, and generate one gantt frame for each frame of the video in the `project_folder\frames\output\gantt_plots` directory, contained within a new folder named after the video file. If you would like to generate gantt charts for only a select csv file, remove the files you want to omitt from visualizing  gantt charts from the `project_folder\csv\machine_results` directory. For example, you can manually create a temporary `project_folder\csv\machine_results\temp` directory and place the files you do **not** want to visualize in this temporary folder. If you'd like to create a video or gif (like above) from the gantt frames, you can do so by using the [SimBA tools menu](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md) and the [`Merge images to video`](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#merge-images-to-video) or [Generate gifs](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#generate-gifs) tools. 

4. **Generate data plot**. In the `Visualization` menu, and the sub-menu `Visualizations`, use the second button named `Generate data plot` to create a frames that display the velocities, movements, and distances between the animals:

<img src="https://github.com/sgoldenlab/simba/blob/master/images/dataplot.gif" width="300" height="200" />

*Note*: Code will run through each csv file in your `project_folder\csv\machine_results` directory, and generate one data frame for each frame of the video and save it in the `project_folder\frames\output\live_data_table` directory, contained within a new folder named after the video file. if you would like to generate data plots for only a select csv file, remove the files you want to omitt from visualizing  gantt charts from the `project_folder\csv\machine_results` directory. For example, you can manually create a temporary `project_folder\csv\machine_results\temp` directory and place the files you do **not** want to visualize in this temporary folder.

4. **Generate path plot**. In the `Visualization` menu, and the sub-menu `Visualizations`, use the third menu named `Path plot` to generate graphs depcting the movement of the animals, the location of the animals, where the behaviors of intrest occurs, and how 'severe' the behavior is at each location. The output may look something like this:

<img src="https://github.com/sgoldenlab/simba/blob/master/images/pathplot.gif" width="199" height="322" />

The generation of the path plots requires several user-defined parameters:

- `Max Lines`: Integer specifying the max number of lines depicting the path of the animals. For example, if 100, the most recent 100 movements of animal 1 and animal 2 will be plotted as lines.

- `Severity Scale`: Integer specifying the scale on which to classify 'severity'. For example, if set to 10, all frames containing attack behavior will be classified from 1 to 10 (see above). 

- `Bodyparts`: String to specify the bodyparts  tracked in the path plot. For example, if Nose_1 and Centroid_2, the nose of animal 1 and the centroid of animal 2 will be represented in the path plot.

- `plot_severity`: Tick this box to include color-coded circles on the path plot that signify the location and severity of attack interactions.

After specifying these values, click on `Generate Path plot`.

*Note*: After clicking on `Generate Path plot`, the code will run through each csv file in your `project_folder\csv\machine_results` directory, and generate one path plot frame for each frame of the video and save it in the `project_folder\frames\output\path_plots` directory, contained within a new folder named after the video file. if you would like to generate path plots for only a select csv file, remove the files you want to omitt from visualizing path plots for from the `project_folder\csv\machine_results` directory. For example, you can manually create a temporary `project_folder\csv\machine_results\temp` directory and place the files you do **not** want to visualize in this temporary folder.

5. **Generate distance plot**. In the `Visualization` menu, and the sub-menu `Visualizations`, use the last sub-menu titled `Distance plot` to create a frames that display the distances between the animals like this:

<img src="https://github.com/sgoldenlab/simba/blob/master/images/distance_plot.gif" width="300" height="225" />

The generation of the distance plots requires two user-defined parameters:

- `Body part 1`: String that specifies the the bodypart of animal 1. Eg., Nose_1

- `Body part 2`: String that specifies the the bodypart of animal 1. Eg., Nose_2

Click on `Generate Distance plot`, and the distance plot frames will be generated in the `project_folder/frames/output/line_plot` folder.

*Note*: After clicking on `Generate Distance plot`, the code will run through each csv file in your `project_folder\csv\machine_results` directory, and generate one distance plot frame for each frame of the video and save it in the `project_folder\frames\output\line_plot` directory, contained within a new folder named after the video file. if you would like to generate path plots for only a select csv file, remove the files you want to omitt from visualizing distance plots for from the `project_folder\csv\machine_results` directory. For example, you can manually create a temporary `project_folder\csv\machine_results\temp` directory and place the files you do **not** want to visualize in this temporary folder.

6. **Merge Frames**. If you have followed through all of **Part 5** of this tutorial, you should have generated several graphs of your machine predictions and data (i.e., gantt plots, line plots, path plots, data plots, sklearn plots) stored in different sub-directories in the `project_folder\frames\output` folder. You may want to merge all these frames into single frames, and later a video, to more readily observe the behavior of intrest and its different expression in different experimental groups, like in the following video example:   

<img src="https://github.com/sgoldenlab/simba/blob/master/images/mergeplot.gif" width="600" height="348" />

To merge all the generated plots from the previous step into single frames, navigate to the following button in the `Visualization` menu and click it: 

<img src="https://github.com/sgoldenlab/simba/blob/master/images/mergeframes.PNG" width="121" height="62" />

Clicking `Merge Frames`, all the generated plots for each video will be combined and saved in the `project_folder/frames/output/merged` folder, in subfolders named after each video. 

*Note*: After clicking on `Merge Frames`, the code will look in each folder contained in the `project_folder\frames\output` directory, for subfolders with matching names (e.g. `project_folder\frames\output\gantt_plots\Video1`, `project_folder\frames\output\line_plot\Video1`, `project_folder\frames\output\sklearn_results\Video1` etc...). If any of the folders are missing, or of any matching folder differ in the numer of frames contained, you will get an error. 

7. **Create Videos**. At this point, you may want to merge the frames contained in the `project_folder/frames/output/merged` directory to video files, with one video for each of the subdirectories in the `project_folder/frames/output/merged` folder. In the `Visualization` menu, navigate to the following sub-menu:

<img src="https://github.com/sgoldenlab/simba/blob/master/images/createvideoini.PNG" width="200" height="100" />

To create a video from the frames, SimBA requires two user-defined parameters: two video file format(s), and the bitrate:

- `Bitrate`: Bitrate is the number of bits per second. It generally determines the size and quality of video and audio files: the higher the bitrate, the better the quality and the larger the file size. If unsure, try setting bitrate to something relatively small like **2400**. To read more about bitrate, click [here](https://help.encoding.com/knowledge-base/article/understanding-bitrates-in-video-files/). 

- `File format`: The format of the output video, it can be mp4, mov, flv, avi, mpeg. Please enter the file format without the ".". (e.g., enter *mp4*, not *.mp4*). 

The output video from the merged frames will be stored in the `project_folder\frames\output\merged` directory. 

*Note*: If the videos are very large, and you would like to down-sample the resolution of the videos to make them smaller, you can do so by using the [SimBA tools menu](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md) and the [Downsample video](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#downsample-video) tools. In the SimBA tools menu, you can also [crop](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#crop-video), and [trim](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#shorten-videos) select videos as well as many more things. Also, 
if you want to create a video or gif from the merged frames of only one video, you can do so by using the [SimBA tools menu](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md) and the [`Merge images to video`](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#merge-images-to-video) or [Generate gifs](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md#generate-gifs) tools. 

















