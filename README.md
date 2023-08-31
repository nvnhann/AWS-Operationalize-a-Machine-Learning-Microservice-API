<include a CircleCI status badge, here>

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

CI/CD: success [here](https://app.circleci.com/pipelines/github/nvnhann/Operationalize-a-Machine-Learning-Microservice-API/1/workflows/7a2e0d09-fa1c-4695-ba98-01316d297de6/jobs/1)

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

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

* Setup and Configure Kubernetes locally
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
* Create Flask app in Container
`./run_docker.sh`
* Run via kubectl
`./run_kubernetes.sh`
