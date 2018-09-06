# A-Teams

Study

There were two parts to this study, each conducted in similar ways and divided into sessions.
A "session" consists of 6, 10-minute long sections with a roughly 2-minute break in between each section, although the length of the break is not exact and can vary by a half of a minute at times. During each session, a subject is shown images in rapid succession (more than 5 images per second). There are five main categories of images: chairs, containers, doors, posters, and stairs. Some images may contain more than one category of image or none at all, the latter of which is used as a distractor. For each section, one of these categories is the "target." Subjects hold a clicker in their hand, and are instructed to click whenever they see an image that contains the target category, herein referred to as a "target image."
Under these constraints, a "Baseline" and "Expertise" study was conducted.
Baseline
One session, conducted once
Each section within the session has a different target image, except the first and last section have the same target
Expertise
Five sessions, conducted over five days with one per day
Sessions were conducted at roughly the same time each day
Each session had the same target image for all sections, but each day had a different target
Data

The study was performed with a BioSemi 256 (+8) EEG channel system with 4 eye and 2 mastoid channels recorded, with a sampling rate of 2048 Hz.
Preprocessing

 The raw EEG data has been processed with the PREP pipeline. The PREP pipeline is a standardized pipeline for processing EEG data that identifies bad channels and interpolates them.
Full steps of the PREP pipeline (from the above link):
Handle boundary events prior to processing
Remove trend (high pass) temporarily to properly compute thresholds
Remove line noise without committing to a filtering strategy
Robustly reference the signal relative to an estimate of the “true” average reference
Detect and interpolate bad channels relative to this reference
The data has also been down sampled to represent a sampling rate of 256 Hz.

Pilot 1

Following the PREP pipeline, the data has been further standardized around the channel averages: (x - μ) / σ. Each row of data corresponds to a single reading, and the ordering of the rows has been shuffled so is not necessarily the order in which they were read.
All data is provided in a single HDF5 file. Within this file there are 11 Datasets, DS0 through DS9 and DSTest. The numbered Datasets each contain EEG data that correspond to a single subject. DSTest contains the test data, with each row of data coming from one of the 10 subjects. The goal of this competition is to determine which subject each of the rows come from.
Pilot 2

Following the PREP pipeline, the data has been downsampled to include only every fourth reading that was originally taken. In addition, only half the original 256 channels have been included, with every other channel being removed from the data. All 6 eye/mastoid channels remain, however.
The data is provided in three HDF5 files: `Correct_Targets.h5,` `Incorrect_Targets.h5,` and `Not_Targets.h5.` There is also a fourth file which contains the test data. The goal of this competition is to predict, for each Dataset in the test file, which of the three categories it belongs to.
Category descriptions:
Correct_Targets: The subject was shown a target and correctly identified it by clicking a button. Both the time of the target being shown and the click are within the one second snapshot.
Incorrect_Targets: The subject was shown a target but did not correctly identify it by clicking. The time of target being shown is within the snapshot.
Not_Targets: The subject was not shown a target and did not click. During this time period, the subject was shown non-target images.
Main

The task for this competition is to determine, given a one second snapshot of EEG data, which of the image categories the subject was shown and whether or not that image was the target image for the session it was taken from. Although in the study subjects were shown target images that contained multiple of the categories, these instances have been filtered out for this competition.
The data is spread throughout 11 HDF5 files, with their names indicating the data contained within. For example, `chair_T.h5` contains examples of when the subject was shown an image of a chair and the chair was the target image at that time, while `door_NT.h5` contains examples of when the subject was shown an image of a door and it was not the target image. Within each file is a number of examples for that category, with each example being contained in a single Dataset. Each Dataset contains a 1000x262 matrix. Each row represents a single reading of the EEG channels and each column represents a channel. The goal of this competition is to determine which of the 10 categories a given one second reading is from, with these readings contained in the file `test.h5.`
