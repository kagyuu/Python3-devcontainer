# Python3-devcontainer

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
    $ poetry add jupyterlab
    ```
    The numpy and jupyter is installed to /workspaces/Python3-devcontainer/.cache/virtualenvs/pyexam-xd-L-yZD-py3.11/  that directory belongs host. The default settings of the poetry is /home/vscode/.local/.cache. That is on the docker contaier, therefore the directory will be cleared when the container stop.
- run jupyter lab
    ```
    $ poetry run jupyter lab
    ```
    The jupyter lab will start and wait on the url "
    http://localhost:8888/?token=bc90012644866b11db7d9f309e41cb1010a18ef89ba7f6b1"
- termiate jupyter lab
    ```
    Ctrl + C
    ```
- rebuild .cache
    ```
    $ poetry update
    ```
    If ".cache" directory is cleared, we can rebuild it from pyproject.toml.
 