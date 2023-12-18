## Configuration

1. Copy the attached workbook into a DFS based Project in Domino
2. Add another folder `/mnt/input` and add your input files to it
3. Add the JAR file to the location `/domino/datasets/local/{domino_project_name}/jar/model.jar`

Create two environments-

### Workspace Environment

#### Base Docker Image

```
quay.io/domino/compute-environment-images:ubuntu20-py3.9-r4.3-ray2.4.0-domino5.7
```

#### Dockerfile instructions

```
USER root
RUN pip install boto3
RUN pip install torchmetrics
RUN mkdir -p /etc/apt/keyrings
RUN wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
RUN echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^UBUNTU_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
RUN apt update
RUN apt install -y temurin-17-jdk
USER ubuntu
```

#### **Pluggable Workspace Tools**
```
jupyter:
  title: "Jupyter (Python, R, Julia)"
  iconUrl: "/assets/images/workspace-logos/Jupyter.svg"
  start: [ "/opt/domino/workspaces/jupyter/start" ]
  supportedFileExtensions: [ ".ipynb" ]
  httpProxy:
    port: 8888
    rewrite: false
    internalPath: "/{{ownerUsername}}/{{projectName}}/{{sessionPathComponent}}/{{runId}}/{{#if pathToOpen}}tree/{{pathToOpen}}{{/if}}"
    requireSubdomain: false
jupyterlab:
  title: "JupyterLab"
  iconUrl: "/assets/images/workspace-logos/jupyterlab.svg"
  start: [  "/opt/domino/workspaces/jupyterlab/start" ]
  httpProxy:
    internalPath: "/{{ownerUsername}}/{{projectName}}/{{sessionPathComponent}}/{{runId}}/{{#if pathToOpen}}tree/{{pathToOpen}}{{/if}}"
    port: 8888
    rewrite: false
    requireSubdomain: false
vscode:
  title: "vscode"
  iconUrl: "/assets/images/workspace-logos/vscode.svg"
  start: [ "/opt/domino/workspaces/vscode/start" ]
  httpProxy:
    port: 8888
    requireSubdomain: false
rstudio:
  title: "RStudio"
  iconUrl: "/assets/images/workspace-logos/Rstudio.svg"
  start: [ "/opt/domino/workspaces/rstudio/start" ]
  httpProxy:
    port: 8888
    requireSubdomain: false
```
### Ray Cluster Environment

Make sure you click on "Ray" in the "Supported Cluster Settings" 

#### Base Docker Image
```
quay.io/domino/compute-environment-images:ubuntu20-py3.9-r4.3-ray2.4.0-domino5.7
```
#### Dockerfile instructions
```
USER root
RUN pip install boto3
RUN pip install ray==2.4.0
RUN pip install torchmetrics
RUN sudo mkdir -p /etc/apt/keyrings
RUN sudo wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
RUN sudo echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^UBUNTU_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
RUN sudo apt update
RUN sudo apt install -y temurin-17-jdk
USER ubuntu
```
