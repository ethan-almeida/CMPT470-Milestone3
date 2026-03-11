## This is the README file for the AST-GNN tool. 

This artifact was discovered using another research paper. There was no GitHub repository included with the original paper mentioned with this project and hence we used AI-assisted to help us find a suitable alternative that would be the closest match to the research paper that was assigned to us. 

Link to the original research paper assigned to us:- https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10401783

Link to the alternative tool's research paper:- https://arxiv.org/pdf/2002.08653

Link to the GitHub repository of the alternative tool:- https://github.com/jacobwwh/graphmatch_clone

# Environment details:-

- Running on WSL2 Ubuntu v24.04
- Using Python v3.8.20 on a virtual environment
- NVIDIA CUDA v13.2 on an RTX 3060 6GB (laptop variant) w/ 16 GB of system RAM

# Installation and execution steps:-

- there was no requirements.txt file, but there were some libraries mentioned in the tool's repository README
- these are the packages we installed (in order):-
	- numpy
	- torch
	- torchvision
	- torchaudio
	- torch-geometric
	- pycparser
	- tqdm
	- javalang
	- anytree
	- torch_scatter
	
- need to setup datasets, used unzip -n javadata.zip and unzip -n googlejam4_src.zip
- code interventions, please look below for more details
- ran the tool, command used: python ggnn_gcj.py --graphmode astonly --data_setting 0small --num_layers 1 --num_epochs 1 --batch_size 64

# Benchmarks used:-
- Google Code Jam


# Interventions:-
Multiple interventions were needed as there were a number of errors in the actual python files themselves that were preventing us from getting an output.
These include :-

- File parse crash
There was an error for list index being out of range in the createclone_java.py file, this was fixed by adding guards in createdatapair() to strip() each line and continue on lines that were blank or malformed.

- CLI arguments parsed as strings when they are actually numbers
This error was in ggnn_gcj.py.
This was fixed by updating the argparse defintions to use type=int for specifically the batch_size, num_layers, num_epochs parameters

- Tensor shape error
This was in ggnn_gcj.py as well. Error was fixed by just replacing 'label' with '[label]' in both the training and testing loops (test())


# Execution outcome and TES classification:-

On running the tool, we got a precision of 0.36, a recall of 0.95

Since running this tool needed a few interventions to get up and running, we classified this as a TES-B tool

