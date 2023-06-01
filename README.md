# Python3-devcontainer

## Docker Hub

https://hub.docker.com/r/nvidia/cuda/tags

Suit CUDA-Version between host and docker-container.
So, if host's CUDA is 11.7, use docker image "nvidia/cuda:11.7.1-devel-ubuntu22.04". 

```
$ nvidia-smi
Fri Jun  2 00:27:20 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.83       Driver Version: 517.66       CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA T550 Lap...  On   | 00000000:03:00.0 Off |                  N/A |
| N/A   45C    P8     3W /  N/A |      0MiB /  4096MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A        23      G   /Xwayland                       N/A      |
+-----------------------------------------------------------------------------+
```

## settings

- .devcontainer/devcontainer.json

```
"args": {
    "WORKDIR" : "Python3-devcontainer",
    "PYTHON_VERSION" : "3.11",
    "PYTHON_REVISION" : "3"
}
```

|Key|Value|Note|
|---|-----|----|
|WORKDIR|This directory.|Mount this direcotory as /workspace/${WORKDIR} on the docker container.|
|PYTHON_VERSION|Python version||
|PYTHON_REVISION|Python revision||

## How to use poetry.

- create poetry project
    ```
    $ mkdir pyexam
    $ cd pyexam
    $ poetry init
    ```
- add packages
    ```
    $ poerty add numpy
    $ poetry add jupyter
    ```
    The numpy and jupyter is installed to /workspaces/Python3-devcontainer/.cache/virtualenvs/pyexam-xd-L-yZD-py3.11/  that directory belongs host. The default settings of the poetry is /home/vscode/.local/.cache. That is on the docker contaier, therefore the directory will be cleared when the container stop.
- run jupyter notebook
    ```
    $ poetry run jupyter notebook
    ```
    The jupyter notebook will start and wait on the url "
    http://localhost:8888/?token=bc90012644866b11db7d9f309e41cb1010a18ef89ba7f6b1"
- termiate jupyter notebook
    ```
    Ctrl + C
    ```
- rebuild .cache
    ```
    $ poetry update
    ```
    If ".cache" directory is cleared, we can rebuild it from pyproject.toml.
 
