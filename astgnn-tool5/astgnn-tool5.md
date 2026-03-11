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

# Benchmarks used:-
- BigCodeBench


# Interventions:-

There was 1 small step required, as the models.py file was importing an older version of the torch-scatter library and hence needed to be refactored to use the correct name.


# Execution outcome and TES classification:-


