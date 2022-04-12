# Neural-Ideal-Experimental-Pipeline
Pipeline for using ephysiology DandiSet data in SageMath for the extraction of receptive field information

Starting in the Dandihub (for more information regarding using DANDI see \ref{sec:Intro:DANDI}), the attached python file, DandiSets to Neural Codewords.ipynb must be downloaded and opened for the code implementation. This file can be saved within the hub and can be edited for additional data analysis desired through the DANDI archive. The first section of code required is the importation of necessary packages and defining the necessary variables. Of the necessary variables is the Dandiset ID and file name desired to investigate.  
Determination of which dataset to use in the pipeline can be decided through the DANDI archive. The DANDI archive can be accessed from the URL:
    https://gui.dandiarchive.org/#/
with the list of Dandisets is visible under the Public Dandisets tab.

  The archive lists all of the publicly accessible Dandisets and clicking on each Dandiset will enable you to view the last update of the dataset, any associated publications (for further context of the study), and the metadata for the experimental data. While there are numerous data collection methods included in the DANDI archive, the following pipeline can only access spike train data from electrophysiology experiments, which is denoted under the Assets Summary section at the bottom of the page for the specific Dandiset. Opening the files will lead you to a structure of nested folders containing a file for each test trial run on each subject tested. For use in the Dandihub, only one file can be streamed in at a time so it will be necessary to choose a particular file to investigate first. Once a file is chosen, for the sake of adding to the code, record the name of the specific file, the file it is directly located in, and the Dandiset ID number, which is located directly under the name of the full Dandiset. 

Now that the file is denoted, the code for streaming in the desired Dandiset for the code to read. A benefit to this method of streaming in the file is that it only temporarily reads in the file and therefore saves time and memory. Additionally, by streaming in the file prior to running the function needed to convert the data, additional analyses can be done to determine that the variables defined are appropriately defined. One tool useful to use prior to obtaining neural code data is
nwb2widget(nwb)
The nwb widget is optional in that it doesn't provide any computations that can directly be used towards the pipeline, but it is useful for further understanding the context of the experiment contained within the file. This code can be implemented to provide initial summaries and visualizations of the file, including a raster plot of the spike train data for the individual neuronal units being recorded from.

In order to analyze the experimental data in the Neural Ideals SageMath package, the neuronal firing patterns must be binarized and written as the neuronal codewords. For the sake of binarization, the spike train data is compiled into 1 second long bins, and if the neuron fires within the second window of time, the bin is represented to have fired by being denoted as a '1'. Otherwise, the neuron did not fire during that second of time and is denoted as a '0'. To create the matrix containing all of the values, the code loops through the spike train data for each of the units and then transposed. The resulting matrix is comprised of columns of spike train data for each unit where each row is a different second time point. Then this matrix must be rewritten as a list of code words for input into SageMath, where each value of the codeword represents a different neuron and each codeword represents a different snapshot of the firing patterns at a particular time.

Once the matrix is rewritten, the codewords are then written to a text file. For the sake of clarity, the new file has the default name reflecting that it contains the neural code for the specific file of the particular Dandiset and the date of creation. These files should be downloaded to a local folder for access in the SageMath Jupyter notebook.

After files of the neural code matrices have been created through the Dandihub and downloaded to a local folder, it is time to read the matrices into the SageMath Jupyter notebook (for more information regarding using SageMath see \ref{sec:Intro:SageMath}). This notebook can be locally hosted by opening the SageMath 9.3 Notebook application downloaded to your computer. The notebook will be launched on the default web browser. This notebook does not automatically come equipped with the Neural ideal package by Petersen et al. 2016. Additionally, SageMath has been updated since the Neural Ideal repository was created so the Neural Ideal package files have been updated and available within this repository. These Neural Ideal package files (iterative_canonical.spyx, neuralcode.py, and examples.py) will need to be downloaded locally and loaded into the SageMath code. 
load("NeuralIdeals/iterative_canonical.spyx")
load("NeuralIdeals/neuralcode.py")
load("NeuralIdeals/examples.py")
For this example, these files are all saved within the folder "NeuralIdeals" within the file tree displayed in the other browser window when the notebook is launched.

The first necessary piece of code towards running analyses is implementing code for reading in the neural code from the file written by the Dandihub. 
file_name =r"C:\Users\Colle\Honors\
            neural_code_0000052022-04-052.txt"
f = open(file_name, "r")
codewords = f.read()
codewords_list = codewords.split(",")
f.close()

It is best to document the file name as the full file path of the text file so that there is no confusion as to where the file is located.

Next, code can be implemented to compute the neural ideal and the associated canonical receptive field structure. 
neuralCode = NeuralCode(codewords_list)
neuralIdeal = neuralCode.neural_ideal()
neuralIdeal

Finally, depending on the analyses desired to run there are different codes established in the Neural Ideal package which are listed below. [insert the different codes]

For the sake of our analyses we used the code to compute the canonical receptive field structure and the dimension of the ideal.
    neuralCode.canonical_RF_structure()
    neuralIdeal.dimension()
