# SEA-pipeline


## Running the FreeSurfer longitudinal pipeline

All of the steps below must be run prior to running fmriprep (example below for one subject with three sessions):

1.	Cross-sectionally process all time points with the default parameters.
```bash
export SUBJECTS_DIR="${BIDS_DIR}/derivatives/long-freesurfer"
recon-all -all -s 100101 -i ${BIDS_DIR}/sub-1001/ses-01/anat/sub-1001_ses-01_T1w.nii.gz
recon-all -all -s 100102 -i ${BIDS_DIR}/sub-1001/ses-02/anat/sub-1001_ses-02_T1w.nii.gz
recon-all -all -s 100103 -i ${BIDS_DIR}/sub-1001/ses-03/anat/sub-1001_ses-03_T1w.nii.gz
```

2.	Create an unbiased template from all sessions for each subject.
```bash
recon-all -base 1001 -tp 100101 -tp 100102 -tp 100103
```

3.	Longitudinally process all sessions with the unbiased template as input. 
```bash
recon-all -long 100101 1001 -all
recon-all -long 100102 1001 -all
recon-all -long 100103 1001 -all
```


## Setting up the BIDS directory structure with session # incorporated into each subject ID

1. Symbolically link the longitudinal freesurfer directories (from step 3 above) into freesurfer directory that fmriprep will read from.
```bash
ln -s ${BIDS_DIR}/derivatives/long-freesurfer/100101.long.1001 ${BIDS_DIR}/derivatives/fmriprep/sub-100101
ln -s ${BIDS_DIR}/derivatives/long-freesurfer/100102.long.1001 ${BIDS_DIR}/derivatives/fmriprep/sub-100102
ln -s ${BIDS_DIR}/derivatives/long-freesurfer/100103.long.1001 ${BIDS_DIR}/derivatives/fmriprep/sub-100103
```
