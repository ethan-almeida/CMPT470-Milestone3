This is the README file for the CodeGraph4CCDetector.

This artifact was discovered from the research paper associated with this topic.

## Environment details:-
- Running on WSL2 Ubuntu v24.04
- Using Python v3.8.20 on virtual environment
- NVIDIA CUDA v13.2 on an RTX 3060 6GB (laptop variant) w/ 16 GB of system RAM

## Installation and execution steps:-
- original requirements.txt many issues so ended up installing different versions of the libraries required
- pip install torch==2.1.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
- pip install torch-geometric
- pip install pyg-lib torch-scatter torch-sparse -f https://data.pyg.org/whl/torch-2.1.0+cu121.html
- pip install numpy==1.19.2 scikit_learn==1.1.2 tqdm==4.62.3 utils==1.0.1 
- first execute "cd/CodeGraph4CCDetector"
- then run "python main.py" for 1 Epoch --> this gets saved to a savedModel/ folder
- then run "main_test_fast.py"


## Benchmarks used:-
- Google Code Jam (java samples, trainsmall dataset) uses trainsmall.txt for training and valid.txt for validation
- Test file is test.txt
- Original dataset was not used since training time was taking way too long (35 hrs+). This dataset took about 10-15 mins


## Any interventions used:-
- running requirements.txt by itself was causing way too many issues.
- had to install torch v1.9.0+cu102
- had to install torch_scatter v2.0.9 (changed from 2.0.8 in original requirements to match above torch version)
- had to install torch_sparse 0.6.12 (to match cu102 build of torch we installed)
- used a subset of benchmark to save time. changed 'id' parameter in main.py to 'id = 0small' to use the smaller dataset



## Execution outcome and TES classification:-

TES classification: TES-E


While we were able to run the tool and get results, the process of setting up the environment to obtain these results was a challenge. The paper did not mention which version of Python they were using which made it very difficult to
figure out what version would work well with their requirements. Addtionally, since using a GPU would give us the best result, setting up the torch libraries to use the GPU was tedious and again not covered in the tool's documentation or in the
paper. The results that we obtained after executing this tool was far from what the paper had mentioned in their results section. While we did use a smaller dataset due to not having enough compute resources, the results were way off from what the paper had reported. All these reasons led up to us classifying this tool as TES-E.


## Results using the smaller dataset were:-


Precision: 0.3973
Recall: 0.5000
F1-Score: 0.4428

These are lower when compared to the paper. This can be attributed to the in the library versions being used, the hardware we are running this on (the paper was likely produced using workstation grade hardware which is often
very expensive).


