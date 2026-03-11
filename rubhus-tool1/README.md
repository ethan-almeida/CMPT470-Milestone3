# Rubhus Cross-Language Clone Detector

This artifact was discovered from the research paper associated with this topic.

## Environment Details

- Running on WSL2 Ubuntu v24.04
- Using Python v3.7.20 on virtual environment
- CPU-only execution (RTX 3060 not supported by torch 1.7.1)

## Installation and Execution Steps

- Original requirements.txt had compatibility issues. Used the following installation steps:
- pip install torch==1.7.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
- pip install torch-geometric==1.6.3
- pip install torch-scatter==2.0.5 torch-sparse==0.6.8 torch-cluster==1.5.8 torch-spline-conv==1.2.0 -f https://data.pyg.org/whl/torch-1.7.1+cpu.html
- pip install numpy==1.16.2 arguments==76 tensorboard


## Code Fixes:
Fixed import in trainerRubhus.py line 285: changed from modelCloneDetection import Net to from rubhusModel import Net

## Dataset:
Downloaded AtCoder SQLite3 database (75MB)

## Execution:
- Attempted to run trainerRubhus.py via the command "python trainerRubhus.py"
- The training process would not initiate since it always threw up an error asking for the files "ClonePairs.txt" and "NonClonePairs.txt". The repository or the paper did not go into how they got these files. I did have access to the database files and the
additional .py files they used but the documentation or the repository's README did not mention the folder structure of those datasets.
- Because of the above error, the tool would not pass the build stage at all.
- We also tried, using the CodeChef database as well (since they mentioned using it). This also failed because again it asked for the same text files mentioned above and that dataset did not include those files either. 

## Benchmarks Used
Dataset: AtCoder Cross-Language Clone Dataset, CodeChef database

## Languages: Java and Python
## Format: SQLite3 database

## Interventions Used
Downgraded to Python 3.7 (numpy 1.16.2 incompatible with Python 3.8+)
Used CPU-only torch builds (GPU not supported by torch 1.7.1)
Fixed incorrect import statement in trainerRubhus.py
Added missing tensorboard dependency


## Execution Outcome and TES Classification
TES Classification: TES-D (Non-Executable)
The tool cannot execute due to missing critical dataset files. The code expects ClonePairs.txt and nonClonePairs.txt in CloneDetectionSrc/ folder, but these files are not provided in the dataset downloads. The SQLite3 database and raw data archive do not contain these text files, and no preprocessing scripts are provided to generate them. Repository documentation does not explain how to create these required files from the available datasets.

Results: N/A - Tool fails at data loading stage before training can begin.
