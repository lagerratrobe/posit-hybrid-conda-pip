Next to this README, there is a notebook. The goal is to run this notebook in both Posit Connect and Posit Workbench. Only install the dependencies found in environment.yml with `conda`, and only install the dependencies found in requirements.txt with `pip`.

To install Conda in your Workbench/Connect image, have your Dockerfile run:

``` bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```

If that doesn't work, read further instructions here: https://docs.conda.io/projects/miniconda/en/latest/. Once you've achieved that, install your Conda dependencies. We have an Dockerfile that does something similar to this to ensure the Conda environment activates when the image is run:

``` Dockerfile
RUN conda install -f environment.yml -n conda-env
RUN conda init bash
RUN echo "\nconda activate conda-env" >> ~/.bashrc
ENTRYPOINT ["bash"]
```

Use this image in Connect/Workbench. The next part is specific to each service:

1. Connect

Import the notebook with the `manifest.json` given. Just validate that the notebook runs successfully. If scipy was installed from requirements.txt, and pandas was installed to the image from environment.yml, and the `conda-env` environment was activated per the Dockerfile, then it might work.

2. Workbench

- Create a JupyterLab session using the image created previously.
- Go into the terminal, and `source ~/.bashrc` to activate conda.
- Run `conda activate conda-env`
- `pip install -r requirements.txt` from inside the `(conda-env)` environment which was just activated.
- Go to the notebook and check if the Python environment, `conda-env`, installed by `conda` in the Devcontainer, is available as a kernel for the notebook to use.
- Run the notebook using this kernel.
