This is a ReadMe for TreeCen


Artifact Verification:
The artifact was discovered from the research paper associated with this topic. The datasets use in the paper were CodeJam and BSB, however, the CodeJam datasets used in the paper are not accessible. 

Another thing that is important to note is that, while the paper mentioned which dataset they used, the github repository itself is not using this. It is using the dataset that is linked below:
https://github.com/CGCL-codes/Toma/tree/main/dataset

Environment Setup:
The Setup is run as follows:
1. It is running on Windows 11.
2. It uses Python 3.13.5 with Conda as the interpreter.

Installation:
The artifact uses libraries not native to conda and are the following:
1. javalang (pip install javalang)
2. anytree (pip install anytree)
3. numpy (pip install numpy)

Execution:
To run the whole experiment:
1. Dataset (from the given github link above), must first be saved in the project folder. More specifically, you need the following files:
     - clone.csv
     - nonclone.csv
     - id2sourcecode.zip (must be unzipped)
2. Once saved, you must first run python get_centmatrix.py in the terminal, which will give you the central matrix as a json file. The project, however uses hardcoded file paths (uses their own absolute paths), so make the following changes in the get_centmatrix.py
     - java_src_path: the path where your unzipped id2sourcecode folder exists
     - out_path: the path where you want to save your cent_matrix1.json
  
3. Next, you must run the classification.py. It is important to note however, that the python script itself is not working as making it work will need changes in the logic of how they read and run the classification, which is not allowed in this milestone. The errors thrown by running the script can be seen in the classification_py_error.png in the TreeCen folder of this repository.

Result Assessment: None 
As the classification.py, which is needed for the results, cannot be run without major modifications such as changing how the script reads the csv file it needs. It is also important to mention now that since the python program itself needs a specific formatting of the csv files its reading, it will not be reproduced with other benchmarks given for this milestone.

Tool Executability Status: TES-C
Since the get_centmatrix.py worked with changes in the absolute path used. We concluded that this is TES-C (Partially Executable). The tool did not complete the full intended workflow (classification.py will not run without major modifications in the inner working of the program).
