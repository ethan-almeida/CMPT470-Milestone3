This is the README file for the Rubhus Cross-Language Clone Detector.

This artifact was discovered from the research paper associated with this topic.

Environment details:

Running on WSL2 Ubuntu v24.04
Using Python v3.7.20 on virtual environment
CPU-only execution (RTX 3060 not supported by torch 1.7.1)
Installation and execution steps:

original requirements.txt had compatibility issues
pip install torch==1.7.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
pip install torch-geometric==1.6.3
pip install torch-scatter==2.0.5 torch-sparse==0.6.8 torch-cluster==1.5.8 torch-spline-conv==1.2.0 -f https://data.pyg.org/whl/torch-1.7.1+cpu.html
pip install numpy==1.16.2 arguments==76 tensorboard
Fixed import in trainerRubhus.py line 285: changed "from modelCloneDetection import Net" to "from rubhusModel import Net"
Downloaded AtCoder SQLite3 database (75MB)
Attempted to run "python trainerRubhus.py"

Benchmarks used:

AtCoder Cross-Language Clone Dataset
Languages: Java and Python
Format: SQLite3 database 

Any interventions used:

Downgraded to Python 3.7 (numpy 1.16.2 incompatible with Python 3.8+)
Used CPU-only torch builds (GPU not supported by torch 1.7.1)
Fixed incorrect import statement in trainerRubhus.py
Added missing tensorboard dependency
Execution outcome and TES classification:

TES classification: TES-D (Non-Executable)

The tool cannot execute due to missing critical dataset files. The code expects ClonePairs.txt and nonClonePairs.txt in CloneDetectionSrc/ folder, but these files are not provided in the dataset downloads. The SQLite3 database and raw data archive do not contain these text files, and no preprocessing scripts are provided to generate them. Repository documentation does not explain how to create these required files from the available datasets.

Results:
N/A - Tool fails at data loading stage before training can begin.
