# Individual Mouse Activities for Group Environments (*IMAGE*)

This repository contains the auto-generated observability and behaviour labels used to model collective mouse behaviour as in Chapter 6 of my PhD Thesis “Automated Identification and Behaviour Classification for Modelling Social Dynamics in Group-Housed Mice” [1].

## 1. Repository Structure

```
├── dataset         # Curated Version of the Dataset
│    ├── Adult
│    │    ├── A.df
│    │    :
│    │    └── P.df
│    └── Young
│
├── LICENSE
└── README.md
```

-------------

## 2. Dataset
 * This dataset contains associated features for groups of three mice in the homecage:
    * The data pertains to all male cages (three mice per-cage) from the C57BL/6NTac strain at the 3-month or 12-month age group. There are in total 16 unique cages: refer to [1, §3.3] for general details of the data demographics.
    * The features include mouse positions within the cage, locations in the image and automatically generated behaviour labels: refer to [1, §6.2] for the automated labelling process.
 * The basic unit of processing is the **Behavioural Time Interval (BTI)** (1 second duration): within this, we are interested in the behaviour of each mouse individually.
 * The data is provided for reproducibility purposes and also to allow further research on the data: for this reason, it is provided in its complete form and with minimal processing.

### 2.1 Storage Organisation
 * The dataset is stored organised by age-group: Adult/Young.
 * Within each, we provide a dataframe of features for each cage, enumerated via an alphabetical code running from **A** through **P**.

### 2.2 Format of individual Dataframes
 * The data is stored as pandas dataframes in pickle format with `bz2` compression. They can be read into python using `pd.read_pickle('...', compression='bz2')`.
 * Each Dataframe is indexed by:
    * `Run`: A continuous recording period around a light-to-dark or dark-to-light transition (see [1, §3.3.1] ). Run identifiers are unique across cages within the same age-group.
    * `Segment`: Segment identifier. Segments are unique within each cage
    * `BTI`: Sample index (since start of segment)
    * `Mouse`: Red/Green/Blue identifier
 * The columns in each dataframe are organised in a conceptual two-level index:
    * `Sensors`: 'Groundtruth' information from sensors.
        * `RFID.Ant`: Mode of the antenna-based location for the BTI --- integer from 1 to 18
        * `RFID.Vel`: Number of changes in the antenna pickup within the BTI.
        * `Light`: Boolean indicator of lights-on (True) or lights-off (False).
    * `TIM`: Auto-detections and identifications of mice in the BTI using the Tracking & Identification Module ([1, §4] and [2]). This is the average Bounding-Box in the BTI, represented in CoCo format i.e. top-left corner co-ordinate (x,y) and size (w,h).
    * `ALM`: Auto-classification of the Observability and Behaviour in the BTI using the Activity Labelling Module [1, §5].
        * `Obs`: Observability Label --- Boolean True/False
        * `Beh`: Behaviour Label --- 1 of 7 behaviour labels according to simplified schema in [1, §5.3.1]: **N/Obs** is used if not observable.
    * `ALM.Prob`: Probability scores over the Behaviour labels provided by the ALM [1, §5]. This is the score per behaviour.

-------------

## References
 If you find this data useful, please consider citing our work.

 For the general dataset:
 > [1] M. P. J. Camilleri, “Automated Identification and Behaviour Classification for Modelling Social Dynamics in Group-Housed Mice,” University of Edinburgh, 2023.

 For the Tracking and Identification Module:
 > [2] M. P. J. Camilleri, L. Zhang, R. S. Bains, A. Zisserman, and C. K. I. Williams, “Persistent Object Identification Leveraging Non-Visual Markers,” CoRR (arXiv), cs.CV (2112.06809), Dec. 2021.
