## Known Issues

- Known and solved issues are collected under this topic to inform readers. Please check all titles if you face an error in alpha-trainer-tf

#### 1)Can not download data from DVC with GitPython

- If you activate the conda environment with this code:

    ```shell
    # Activating conda environment
    source /home/user_name_smartalpha_ai/miniconda3/bin/activate alpha_trainer_tf2
    ```

You will get a GitPython download error due to not being able to access the specified repository on code. There are two ways to solve this problem. Firstly, you should run this script if you want to keep activating the conda enviorenment by running activate file such as above.

    ```shell
    # Download p11 kit
    conda install -c conda-forge p11-kit
    ```
Secondly, you can activate the conda environement by running the script below with no issue. In this case, you do not have to download p11 kit.

    ```shell
    # Download p11 kit
    conda install -c conda-forge p11-kit
    ```
