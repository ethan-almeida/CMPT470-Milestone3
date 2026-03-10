470 milestone 3

CCLearner Reproduction Report

1. Artifact Discovery

The research paper assigned for reproduction is:

CCLearner: A Deep Learning-Based Clone Detection Approach

The official artifact associated with this tool was located through the authors’ repository:

Repository:
https://github.com/liuqingli/CCLearner

The repository contains the following components:
	•	Feature extraction module
	•	Training module
	•	Testing module
	•	Precompiled executable JAR files

2. Environment Setup

The experiments were conducted in a virtual machine environment on a MacBook Air M4 using UTM to ensure reproducibility.

Environment configuration:

Component       	Version
OS	         Ubuntu 22.04 Arm64
Java	         OpenJDK 8
PostgreSQL	 PostgreSQL 14
Virtualization	 UTM


Java was installed using the following command:

sudo apt install openjdk-8-jdk

PostgreSQL was installed using:

sudo apt install postgresql postgresql-contrib

3. Benchmark Acquisition

The assignment requires the use of approved clone detection benchmarks.

For this experiment, the BigCloneBench dataset was selected.

The following artifacts were downloaded:
	1.	BigCloneBench PostgreSQL database dump
	2.	IJaDataset source code dataset
These were downloaded from the course onedrive link 

4. Database Setup

The BigCloneBench database dump was imported into PostgreSQL.

First, a new database was created:

sudo -u postgres createdb bigclonebench

The dataset was then restored:
sudo -u postgres psql -d bigclonebench -f bcb.pgdump/bcb

To verify the database import, the tables were listed:

psql -h localhost -U postgres -d bigclonebench -c "\dt"

The following tables were successfully imported:

clones
false_positives
function_similarity_cache
functionalities
functions
judged
samples
tagged

This confirms that the BigCloneBench was correctly loaded.

5. Configuration Setup

The default configuration file provided in the repository referenced absolute paths from the authors’ system.

The configuration file was modified to match the current environment.

Key parameters updated include:

source.file.path=/home/yeetad/cmpt470-repro/data/BigCloneBench/dataset/
output.dir=/home/yeetad/cmpt470-repro/output/

postgreSQL.conn=jdbc:postgresql://localhost:5432/bigclonebench
postgreSQL.user=postgres
postgreSQL.passwd=postgres

mkdir -p ~/cmpt470-repro/output

6. Compatibility Intervention

During execution, the tool failed to connect to PostgreSQL with the following error:

The authentication type 10 is not supported

This occurred because the PostgreSQL JDBC driver bundled with the tool does not support the SCRAM-SHA-256 authentication method, which is enabled by default in modern PostgreSQL versions.

To resolve this issue, the authentication method in pg_hba.conf was modified.

Original configuration:

scram-sha-256

Updated configuration:

md5

After modifying the configuration, PostgreSQL was restarted:
sudo systemctl restart postgresql

This change allowed the tool to successfully authenticate with the database.


7. Configuration Synchronization Issue

During execution, CCLearner failed to connect to the PostgreSQL database even after the authentication configuration was corrected.

The CCLearner implementation references a hardcoded configuration path used by the original authors.

/home/cclearner/Desktop/CCLearner/CCLearner.conf

As a result, the configuration file located at that path still contained outdated database parameters. The configuration file had to be synchronized so that both configuration files contained the same connection settings.

The following parameters were verified and updated in both configuration files:

postgreSQL.conn=jdbc:postgresql://localhost:5432/bigclonebench
postgreSQL.user=postgres
postgreSQL.passwd=postgres


The configuration files involved were:


~/cmpt470-repro/CCLearner/CCLearner.conf
/home/cclearner/Desktop/CCLearner/CCLearner.conf


After synchronizing the configuration files, the tool was able to connect successfully to the PostgreSQL database.


8. Dataset Directory Layout Mismatch

After resolving configuration and authentication issues, the feature extraction module was executed using:

java -jar CCLearner_Feature.jar


The execution initially failed with the following error:

java.io.FileNotFoundException:
/home/yeetad/cmpt470-repro/data/BigCloneBench/dataset/4/selected/2077739.java

Investigation of the dataset directory showed that the expected file existed but under a different directory structure.

The tool expected source files in the following format:

dataset/<folder_id>/selected/<file>.java

However, the downloaded dataset was organized with the following top-level directories:

dataset/default/
dataset/sample/
dataset/selected/


For example:

Expected path:

dataset/4/selected/2077739.java

Actual path:
dataset/selected/2077739.java

To maintain compatibility with the tool without modifying the original dataset, symbolic links were created to replicate the expected directory structure.

The following commands were used:

cd ~/cmpt470-repro/data/BigCloneBench/dataset

for i in 2 3 4 5 6 7 8 9 10 11; do
  mkdir -p "$i"
  ln -sfn ../selected "$i/selected"
  ln -sfn ../sample "$i/sample"
  ln -sfn ../default "$i/default"
done

This created directories 2 through 11 containing symbolic links to the existing dataset folders, allowing the paths expected by CCLearner to resolve correctly.

9. Feature Extraction Execution

After correcting the dataset directory structure, the feature extraction stage was executed again:

cd ~/cmpt470-repro/CCLearner/Run
java -jar CCLearner_Feature.jar

The tool successfully extracted training data for the clone categories T1, T2, VST3, and ST3.

Execution output:

Extracting True/False Clone Pairs -T1 : 13750/13750
Extracting True/False Clone Pairs -T2 : 3104/3104
Extracting True/False Clone Pairs -VST3 : 1207/1207
Extracting True/False Clone Pairs -ST3 : 4602/4602
Merging Training Files...
Deleting Old Files...
COMPLETE!!
Time Cost: 49869ms

This confirms that the feature extraction module was able to successfully process the benchmark dataset.

The generated training feature file was written to the output directory:

~/cmpt470-repro/output/clone_data_train.csv

Verification of the output directory confirmed that the file was created successfully.


10. Training Stage Failure

After the successful completion of feature extraction, the training stage was executed using:

java -jar CCLearner_Train.jar

The execution failed with the following runtime error:

java.lang.UnsatisfiedLinkError: no jnind4j in java.library.path
Caused by: java.lang.UnsatisfiedLinkError: no nd4j in java.library.path

This error indicates that the training module depends on native ND4J libraries used by the Deeplearning4j framework. These native libraries were not successfully loaded by the Java runtime environment.

An attempt was made to manually specify the native library directory:

java -Djava.library.path=./lib -jar CCLearner_Train.jar
However, the same error persisted, indicating that the required native libraries were not properly resolved at runtime.

Although the required ND4J JAR files were present in the Run/lib directory, the native components required by the deep learning backend could not be successfully loaded in the current environment.


11. Execution Outcome

Based on the experiments performed:
	•	The artifact was successfully located.
	•	The required benchmark dataset was obtained.
	•	The PostgreSQL benchmark database was successfully imported.
	•	Multiple compatibility issues were resolved, including authentication configuration and dataset directory structure.
	•	The feature extraction stage completed successfully and produced the expected output.

However, the training stage failed due to unresolved native ND4J dependencies required by the Deeplearning4j framework.

As a result, the full workflow of the tool could not be completed.


12. Tool Executability Status

According to the Tool Executability Status (TES) taxonomy defined in the assignment, the tool is classified as:

TES-C - Partially Executable

Reason:
	•	The tool successfully completed initial stages of execution, including configuration loading, database interaction, and feature extraction.
	•	The full intended workflow could not be completed due to unresolved native dependency issues during the training phase.


13. Summary

The CCLearner artifact was partially reproducible in the recreated environment. While the initial stages of the pipeline executed successfully after several compatibility adjustments, the training stage could not be completed due to missing or incompatible native deep learning dependencies.

This highlights a common reproducibility challenge in research artifacts where older machine learning frameworks rely on native libraries that may not function correctly in modern environments.
 

