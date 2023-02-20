# Alpha Trainer with Tensorflow 2.X

## What is this repo do different from the original one with Torch ?

- This repo is based on Tensorflow 2.X
- Mostly the same code structure as the original one
- Focused on production environment
- Only algorithms used in the original repository are applied

## How to install

- Install [Anaconda](https://www.anaconda.com/distribution/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    ```shell
    curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh
    $HOME/miniconda3/bin/conda init $(basename $SHELL)
    ```

- Create a new environment with `Python3.10`

    ```shell
    conda create -n alpha_trainer_tf python=3.10
    ```

- To activate the environment

    ```shell
    conda activate alpha_trainer_tf
    ```

- Install required CUDA packages

    ```shell
    conda install cudatoolkit=11.2 cudnn=8.1.0 -c conda-forge
    ```

- After activating the environment, configure environment variables

    ```shell
    mkdir -p $CONDA_PREFIX/etc/conda/activate.d
    echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
    ```

- Upgrade `pip`, `setuptools` and `wheel`

    ```shell
    pip install -U pip
    ```

- Install `alpha_trainer_tf` package

    ```shell
    # To install only trainer
    pip install -e .
    # To install with development packages
    pip install -e ".[dev]"
    ```

- That's it!

### Additional check

- Run `trainer help` to see the available commands

    ```shell
    trainer help
    ```

- Run `trainer version` to see the version of the package

    ```shell
    trainer version
    ```

## Train Model

- To train with default setup

    ```shell
    trainer segment train
    ```

    It will use the config file inside the project directory.

## Download AlphaDataset

- Since the dataset is too large, it is not included in the repository. To download the dataset, run the following command

    ```shell
    trainer segment download dataset.name="Interscalene" dataset.url="https://github.com/smart-alpha/alpha-dataset.git" dataset.rev="main"
    ```

    It will download the Interscalene segmentation dataset  to `dataset.root` directory from the config file. The files
    already downloaded will be skipped. To download the dataset to a different directory, use `dataset.root` option.

    ```shell
    trainer segment download dataset.root="/path/to/dataset" ...
    ```

- Download all region for spesific task

    ```shell
    trainer segment download dataset.name="all" ...
    ```

- Downloader configurations:

  - `dataset.root`:
    - Root directory of the dataset. Default value gathered from `default.yaml`. `alpha-dataset folder will be created inside this directory.

  - `dataset.name`:
    - Name of the dataset. e.g. `Interscalene`, `Supra` etc. If `all` is given, it will download all the available datasets for givent task. Default value gathered from `default.yaml`.

  - `dataset.url`:
    - URL of the dataset remote git repository. e.g. `https://github.com/smart-alpha/alpha-dataset.git`. Leave it empty to use the default git repository. Default value gathered from `default.yaml`.

  - `dataset.rev`:
    - Revision of the dataset. e.g. `main`, `dev`, `experimental/dataset1`, `v0.1.0` etc. Default value gathered from `default.yaml`.

- Some download examples

    ```shell
    # Download Interscalene dataset to /.dataset with default git repository and revision
    trainer segment download dataset.name="Supra" dataset.root="./dataset"

    # Download Interscalene dataset to /opt/dataset with specific git repository and revision
    trainer segment download dataset.name="Interscalene" dataset.root="/opt/dataset"

    # Download all dataset to /opt/dataset with at given git repository and revision
    trainer segment download dataset.name="all" dataset.root="/opt/dataset" dataset.url="https://github.com/smart-alpha/other-dataset.git" dataset.rev="dev"
    ```

- Please let us know if you have any questions or suggestions on `Downloader`. Use the [issue tracker](https://github.com/smart-alpha/alpha-trainer-tf/issues) to report bugs or request features. When creating a new issue, please provide as much information as possible to help us understand and reproduce the problem and use `downloader` label for downloader related issues.


## Known Issues

- Known and solved issues are collected under this topic to inform readers. Please check all titles if you face an error in alpha-trainer-tf.

#### 1) Can not download data from DVC with GitPython

- If you activate the conda environment with this code:

    ```shell
    # Activating conda environment
    source /home/user_name_smartalpha_ai/miniconda3/bin/activate alpha_trainer_tf2
    ```

You will get a GitPython download error due to not being able to access the specified repository on code. There are two ways to solve this problem. Firstly, you should run the following command if you want to keep activating the conda enviorenment by running activate file such as above.

    ```shell
    # Download p11 kit
    conda install -c conda-forge p11-kit
    ```

Secondly, you can activate the conda environement by running the following command below with no issue. In this case, you do not have to download p11 kit.

    ```shell
    # Download p11 kit
    conda install -c conda-forge p11-kit
    ```

#### 2) ImportError: cannot import name 'builder' from 'google.protobuf.internal'
- If you get such error, please follow these steps:
1. Install the latest protobuf version 
    ```shell
    pip install --upgrade protobuf
    ```
2. Copy builder.py from .../Lib/site-packages/google/protobuf/internal to another folder on your computer 

3. Install a protobuf version that is compatible with your project

    ```shell
    pip install protobuf==3.19.4
    ```
4. Copy builder.py from (let's say 'Documents') to Lib/site-packages/google/protobuf/internal
