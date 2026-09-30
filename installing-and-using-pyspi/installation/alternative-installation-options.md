---
cover: >-
  https://images.unsplash.com/photo-1525547719571-a2d4ac8945e2?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw0fHxjb21wdXRlcnxlbnwwfHx8fDE3MTAzNTY1NTV8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Alternative Installation Options

For users encountering issues with installing _pyspi,_ we recommend you first consult the troubleshooting guide. If your issue has not been addressed, we offer three alternative installation options.&#x20;

## 1. Local Installation

***

To install _pyspi_ using a local `pip` install, download or clone the [latest version](https://github.com/olivercliff/pyspi) from GitHub, unpack and install:

```bash
git clone https://github.com/DynamicsAndNeuralSystems/pyspi
cd pyspi
pip install .
```

***

## 2. _pyspi_ Docker Image

***

> _Why should I use a **Docker** Image?_

By using a Docker image, you're not just simplifying the initial setup of _pyspi_, you're also setting the stage for a more reliable and reproducible workflow. Here are some reasons why we recommend users work with a Docker image:

1. **Ease of Setup:**

Configuration issues, operating systems compatibility problems, and dependency conflicts are a just a few of the hurdles you may encounter when trying to install any software package locally. A Docker image eliminates these hassles by providing you with a pre-configured, read-to-run container that has everything the software needs. Just download the image, and you're ready to go!

2. **Reproducibility:**&#x20;

Docker images encapsulate the entire runtime environment - the software, the exact versions of all packages and the necessary configurations. This ensures that the software runs identically, no matter where or when it's executed, and that your results are reproducible each time.&#x20;

### Using the _pyspi_ Docker Image

To get started with using a _pyspi_ docker image, follow these steps:

1.  **Download the Docker Desktop Application**

    Install Docker Desktop from the [official Docker website](https://www.docker.com/products/docker-desktop/). This application runs Docker on your machine.&#x20;
2.  **Create a Docker Account**

    Register for an account on [Docker Hub](https://hub.docker.com) if you haven't already. This account is needed to download ('pull') Docker images.&#x20;
3.  **Pull and Run the pyspi Docker Image**

    With the Docker Desktop application running in the background, open the terminal or command prompt and run the following command:

    ```bash
    $ docker pull jmoo2880/pyspi:latest
    ```

    Now, to run the Docker image:

    ```bash
    $ docker run -it jmoo2880/pyspi:latest
    ```

This should start a python session with _pyspi_ installed and ready to import.&#x20;

***
