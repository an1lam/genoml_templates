# Setup Instructions
Sections organized by provider (AWS, GCP, etc.) but there's probably a lot of overlap.

## AWS
Before you dive into the following sections you will need to create a VM
instance on AWS.  We use the "p2" GPU instance type with 16 vCPUs and 100GB
memory (you may need to request a vCPU limit increase for this specific
instance type). Make sure to download a `.pem` file that will allow you
to access your instance via SSH.

### Launch an instance on AWS & SSH into it
SSH using the PEM file you used/created when you launched your instance:

    ssh -i <path to PEM file> ubuntu@<ec2 instance address>

### Set up the Python environment
Install anaconda for the Linux VM using curl ([installer link](https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh)).

    # double check if link is latest version though
    curl https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh > AnacondaInstaller.sh 
    chmod +x AnacondaInstaller.sh
    ./AnacondaInstaller.sh

Create a new conda environment with the required dependencies.
_Note:_ If you have a different cuda version than the one used
in the `cudatoolkit` version specification, you should change it
in the environment YAML file before running the following command.

    conda create -n <env name> -f dev-dependencies.yml 
    conda activate <env name>

(Optional) Set up a Jupyter (notebook) environment:

    python -m ipykernel install --user --name=genchrom 


### Download DeepSEA's data/metadata and make sample predictions

Create a data directory & run the following commands inside of it:

    # Download training bundle
    curl http://deepsea.princeton.edu/media/code/deepsea_train_bundle.v0.9.tar.gz > deepsea_train_bundle.v0.9.tar.gz
    # (Optional) Download hg19 for Genome reference which we will use in our data loader 
    curl http://hgdownload.cse.ucsc.edu/goldenpath/hg19/bigZips/latest/hg19.fa.gz > hg19.fa.gz
    gunzip hg19.fa.gz


Download the bed files:

    curl http://deepsea.princeton.edu/media/code/allTFs.pos.bed.tar.gz > allTFs.pos.bed.tar.gz
    gunzip allTFs.pos.bed.tar.gz

Test using the following two notebooks: [Preprocess DeepSEA data](https://github.com/an1lam/genoml_templates/blob/main/src/deepsea/Preprocess%20DeepSEA%20Data.ipynb), [Kipoi DeepSEA](https://github.com/an1lam/genoml_templates/blob/main/src/deepsea/Kipoi%20DeepSEA%20PoC.ipynb). 

### Success!
At this point you have a way to access the pre-trained DeepSEA model and also the sequences (in one-hot form) and labels used to train and evaluate it (inside `data/`).
Go forth and prosper.
