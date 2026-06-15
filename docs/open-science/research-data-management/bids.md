# BIDS 

If you are following the project workflow, by now you should have a project set up on the cluster and you might be in the process of organizing your **raw data**. The next step is to convert your data into BIDS format.

[**BIDS**](https://bids.neuroimaging.io/get_started.html) is an acronym that stands for **Brain Imaging Data Structure**. It is a standardized format for the organization and the description of neuroscientific data (see the fMRI example below).</b>  
In a nutshell, BIDS provides a framework for standardizing file names, folder structures and metadata for different kinds of data such as neuroimaging, behavioral, physiological etc.  
We recommend you to go through the [BIDS specification page](https://bids-specification.readthedocs.io/en/stable/) and [BIDS Starter Kit pages](https://bids-standard.github.io/bids-starter-kit/) to understand more in depth the concepts and the phylosophy behind BIDS. [Here](https://bids-website.readthedocs.io/en/latest/datasets/examples.html) you can find a collection of examples of converted BIDS datasets.

```bash
.
├── CHANGES
├── dataset_description.json
├── participants.tsv
├── README
└── sub-01
    ├── anat
    │   ├── sub-01_T1w.nii.gz
    │   ├── sub-01_T1w.json
    │   ├── sub-01_T2w.nii.gz
    │   └── sub-01_T2w.json
    ├── func
    │   ├── sub-01_task-mybrillianttask_bold.nii.gz
    │   ├── sub-01_task-mybrillianttask_bold.json
    │   └── sub-01_task-mybrillianttask_events.tsv
    └── task-mybrillianttask_bold.json

```

## Why BIDS?

- It provides a logical and intelligible structure for data of different kinds.
- It makes your data clear and reusable by other researchers, thus increasing the chance of collaborations and the chance of being cited.
- It helps you to organize your data without coming up by yourself with an efficient and flexible structure.
- It has been adopted by all the major tools for neuroscientific analysis.
- It improves science and also your science. 
  
## How to BIDS 

In the following tabs, you can find links to different tools to convert **unprocessed data** into BIDS for different modalities. We suggest only on specific tools that are either widely used or provide some advantages for the users, for an exstensive list of bids converters, please refer to the official [BIDS page](https://bids.neuroimaging.io/tools/converters.html).

Before you start converting your data, we strongly recommend to go through the [BIDS specification page](https://bids-specification.readthedocs.io/en/stable/) relative to your modalities of interest. Make sure you read the [common principles](https://bids-specification.readthedocs.io/en/stable/common-principles.html) to grasp the jargon used by BIDS. If you have any issue or need help for data conversion, please write to `cecile-admins-l at ovgu.de`

=== "fMRI"

    There several very efficient tools for (f)MRI bids conversion, mainly python based:

    - [**bidscoin**](https://bidscoin.readthedocs.io/en/stable/): It is a GUI based and user friendly converter; it allows users to easily map dicom files with the appropriate bids file names and structure. Once one partipant dataset has been converted, assuming equal settings for all participants, the conversion can be run for all participants at once without the need to specify a participant-specific conversion. It can also be used via command line. [Bidscoin tutorial](https://bidscoin.readthedocs.io/en/stable/tutorial.html)
    - [**dcm2bids**](https://unfmontreal.github.io/Dcm2Bids/3.2.0/): A very powerful command line based converter. Users need to specify a configuration file (.json file) that describes the content of the dataset and allows the tool to map each image to the appropriate bids file. It also provides a helper function that guesses the nature of the images and helps users to fill in the configuration file. [dcm2bids tutorial](https://unfmontreal.github.io/Dcm2Bids/3.2.0/tutorial/)
    - [**heudiconv**](https://heudiconv.readthedocs.io/en/latest/): A very powerful converter based on command line; it requires to define heuristics via python scripting to achieve a correct conversion. It helps users to create proper heuristics by producing a `dicominfo.tsv` which lists all the images in the dicom files and relative information. [Heudiconv tutorial](https://heudiconv.readthedocs.io/en/latest/tutorials.html)

=== "EEG/MEG"

    ### EEG BIDS recommended formats

    Given the variety of EEG data formats, BIDS recommends two main formats (we also strongly suggest to use these two formats):

    - European format: `.edf`
    - BrainVision format: `.vhdr`, `.vmrk`, `.eeg`

    Other BIDS accepted formats, although not recommended by BIDS, are Biosemi: `.bdf` and EEGLAB: `.fdt`, `.set`.

    ### Relevant converters

    - [mne-bids](https://mne.tools/mne-bids/stable/index.html): A python based converter related to the [`mne`](https://mne.tools/stable/index.html) analysis library. It requires python coding. [mne-bids tutorial](https://mne.tools/mne-bids/stable/use.html)
    - [fieldtrip](https://www.fieldtriptoolbox.org/getting_started/othersoftware/bids/): Matlab based converter, it requires matlab coding. It can convert non-recommended data formats to BrainVision or European formats [fieldtrip bids tutorial](https://www.fieldtriptoolbox.org/example/other/bids_eeg/)
    - [eeglab](https://eeglab.org/plugins/EEG-BIDS/): GUI based and user friendly converter; being a Matlab based tool, users can entirely script their data conversion [EEG-BIDS tutorial](https://eeglab.org/plugins/EEG-BIDS/EEG%E2%80%90BIDS-docs.html)
    
=== "Physiological data/Eye-tracking"
    
    ### Physiological data

    We recommend to read the [dedicated page](https://bids-specification.readthedocs.io/en/stable/modality-specific-files/physiological-recordings.html) in the BIDS website. 

    !!! note "Physio data general rules"

        - **What kind of data:** Cardiac, respiratory and other continuous recordings
        - **File types:** Continuous recordings as compressed files `.tsv.gz` (no header). Metadata as `.json` files.  
        - **Where:** Data can go under different `<datatype>/` folders, such as `func`, `anat`, `dwi`, `meg`, `eeg`, `ieeg`, or `beh`
        - **How:** Data must have the following format:</b>  
                        `<matches>[_recording-<label>]_physio.tsv.gz`
                        `<matches>[_recording-<label>]_physio.json`
                    where `<matches>` can be `sub-012_task-mytaskname` and `_recoding-<label>` can be used to distinguish between two or more type of recordings e.g. `recording-breathing` and `recording-eyetracking`. In case you have multiple sessions and runs your file might be: `sub-012_ses-1_task-mytaskname_run-1-breathing_physio.tsv.gz` and the relative metadata `sub-012_ses-1_task-mytaskname_run-1-breathing_physio.json`.

    ### Eye-tracking data

    Eye-tracking data roughly follows the principles of physiological data, but with some fundamental differences, we recommend to carefully read the relevant section in the [official page](https://bids-specification.readthedocs.io/en/stable/modality-specific-files/physiological-recordings.html#eye-tracking)
    
    For eye-tracking data conversion, we suggest to use the `python` based library [`eye2bids`](https://github.com/bids-standard/eye2bids). It works quite well with data collected with all the major eye-trackers. Alternatively, you can use [Fieldtrip](https://www.fieldtriptoolbox.org/example/other/bids_eyetracker/); both software are already available in Cecile.
   
=== Non-invasive Brain Stimulation

    The BIDS proposal for non-invasive brain stimulation is currently in an advanced development stage, but still work in progress; please refer to the [bep037](https://bids.neuroimaging.io/extensions/beps/bep_037.html) page or the [github page](https://github.com/nigelrogasch/nibs-bids/tree/master)

## Populating metadata files

Coming soon ...

## Validate your BIDS dataset

After you have accomplished the BIDS conversion, the last step is to validate your dataset. Keep in mind that this step is very important to ensure that your dataset actually complies with BIDS.

The validation can be done on Cecile by using [`bids-validator`](https://github.com/bids-standard/bids-validator). The `bids-validator` is installed on Cecile, you can use it as any other software, if you do not know how to use software on Cecile, please refer to the [software] page.
[software]: ../../cluster/cecile/software.md

!!! Warning "Periodical validation checks"
    In order to keep the minimal standards on Cecile, there will be periodical BIDS validation on your dataset, in case a dataset is not valid you will receive an email asking to make your dataset BIDS compliant. 


