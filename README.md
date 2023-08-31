<include a CircleCI status badge, here>
[circleci link](https://app.circleci.com/pipelines/github/nvnhann/Operationalize-a-Machine-Learning-Microservice-API/1/workflows/7a2e0d09-fa1c-4695-ba98-01316d297de6/jobs/1)
## Project Overview

In this project, you will apply the skills you have acquired in this course to operationalize a Machine Learning Microservice API. 

You are given a pre-trained, `sklearn` model that has been trained to predict housing prices in Boston according to several features, such as average rooms in a home and data about highway access, teacher-to-pupil ratios, and so on. You can read more about the data, which was initially taken from Kaggle, on [the data source site](https://www.kaggle.com/c/boston-housing). This project tests your ability to operationalize a Python flask app—in a provided file, `app.py`—that serves out predictions (inference) about housing prices through API calls. This project could be extended to any pre-trained machine learning model, such as those for image recognition and data labeling.

### Project Tasks

Your project goal is to operationalize this working, machine learning microservice using [kubernetes](https://kubernetes.io/), which is an open-source system for automating the management of containerized applications. In this project you will:
* Test your project code using linting
* Complete a Dockerfile to containerize this application
* Deploy your containerized application using Docker and make a prediction
* Improve the log statements in the source code for this application
* Configure Kubernetes and create a Kubernetes cluster
* Deploy a container using Kubernetes and make a prediction
* Upload a complete Github repo with CircleCI to indicate that your code has been tested

You can find a detailed [project rubric, here](https://review.udacity.com/#!/rubrics/2576/view).

**The final implementation of the project will showcase your abilities to operationalize production microservices.**

---

## Setup the Environment

* Create a virtualenv with Python 3.7 and activate it. Refer to this link for help on specifying the Python version in the virtualenv. 
```bash
python3 -m pip install --user virtualenv
# You should have Python 3.7 available in your host. 
# Check the Python path using `which python3`
# Use a command similar to this one:
python3 -m virtualenv --python=<path-to-Python3.7> .devops
source .devops/bin/activate
```

### ```run_docker.sh```

Sure, let me explain each part of the script:

- `#!/usr/bin/env bash`: This is called a "shebang" line. It tells the system which interpreter to use to execute the script. In this case, it specifies that the script should be executed using the Bash shell (`/bin/bash`), which is commonly available on Unix-like systems.

- `docker build --tag=project3 .`: This command is used to build a Docker image. It creates an image based on the contents of the current directory (`.`) and adds a descriptive tag to the image. The tag in this case is `project3`, which will be used to identify the image later.

- `docker images list`: This line seems to be incorrect. It should be `docker images` instead of `docker images list`. The command `docker images` lists all the Docker images available on your system.

- `docker run -p 8000:80 project3`: This command runs a Docker container based on the previously built image (`project3`). It maps port 8000 on your host machine to port 80 inside the container. This means that when you access `http://localhost:8000` in your web browser, the traffic will be forwarded to the container's port 80, where the Flask app is running.

### ```run_kubernetes.sh```

- `dockerpath="nvnhan/project3"`: This line assigns the Docker image path to the variable `dockerpath`. In this case, the image is intended to be pushed to the Docker Hub repository with the path `nvnhan/project3`.

- `kubectl run project3 --generator=run-pod/v1 --image=$dockerpath --port=80 --labels "app=project3"`: This command is using `kubectl` (Kubernetes command-line tool) to deploy a pod in a Kubernetes cluster. Here's what each option does:
  - `kubectl run project3`: This specifies the name of the deployment, which is `project3`.
  - `--generator=run-pod/v1`: This is specifying the generator to use, indicating a basic pod deployment.
  - `--image=$dockerpath`: This specifies the Docker image to use for the pod, which is defined by the previously set `dockerpath` variable.
  - `--port=80`: This specifies that the container inside the pod will be listening on port 80.
  - `--labels "app=project3"`: This assigns the label `app=project3` to the pod, which can be used for organizing and identifying pods.

- `kubectl get pods`: This command lists the pods in the Kubernetes cluster. After running the previous command to deploy the pod, this step is likely included to show the status of the newly deployed pod.

- `kubectl port-forward project3 8000:80`: This command sets up port forwarding, allowing you to access the pod's port 80 on your local machine's port 8000. This way, when you access `http://localhost:8000` in your web browser, the traffic will be forwarded to the pod's port 80, where your application is running.

###```upload_docker.sh```

- `echo "Docker ID and Image: $dockerpath"`: This line prints a message indicating the Docker ID and Image that will be used in the subsequent steps.

- `docker login`: This command prompts the user to log in to their Docker Hub account, enabling them to push images to the Docker Hub repository.

- `docker image tag project3 $dockerpath`: This command tags the locally built Docker image `project3` with the specified `dockerpath`. This is necessary to ensure that the image is properly identified when pushing to the Docker Hub repository.

- `docker push $dockerpath`: This command pushes the tagged Docker image to the specified Docker Hub repository using the previously set `dockerpath`.

### Makefile

This `Makefile` is used to automate the setup, installation of dependencies, testing, and linting processes for a project. It provides convenient commands that developers can use to perform common tasks without remembering the exact commands required. Here's a breakdown of the targets and their functionalities:

- `setup`: This target is responsible for setting up the project environment. It creates a Python virtual environment using `python3.7 -m venv ~/.devops`. This is a common practice to isolate project dependencies from the system-wide Python installation.

- `install`: This target installs project dependencies specified in the `requirements.txt` file. It uses the `pip install` command to upgrade `pip` and install the required packages.

- `test`: This target is intended for running tests on the project. In the example, it contains comments showing how additional tests could be added, like running tests with `pytest` or testing Jupyter notebooks using `nbval`.

- `lint`: This target performs linting checks on the project files. It uses two linters:
  - `hadolint` to lint the Dockerfile. `hadolint` is a linter for Dockerfiles, and it helps ensure best practices in Dockerfile creation.
  - `pylint` to lint the Python source code. `pylint` is a widely used Python linter that checks for code quality and coding style.

- `all`: This is the default target that developers can run with the `make` command without specifying a target explicitly. It combines the `install`, `lint`, and `test` targets. This way, you can quickly run all the necessary steps to prepare, lint, and test the project.

Overall, this `Makefile` simplifies the development workflow by providing easy-to-use commands to set up the environment, install dependencies, run tests, and perform linting checks. Developers can simply use `make` followed by the desired target to automate these tasks.

* Run `make install` to install the necessary dependencies

### Running `app.py`

1. Standalone:  `python app.py`
2. Run in Docker:  `./run_docker.sh`
3. Run in Kubernetes:  `./run_kubernetes.sh`

### Kubernetes Steps

* Setup and Configure Docker locally
* Setup and Configure Kubernetes locally
* Create Flask app in Container
* Run via kubectl
