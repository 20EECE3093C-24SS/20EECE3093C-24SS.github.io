# Lab 3 - Containerization

Due Date: Tuesday, February 20, 2024 at 11:59pm

## Objective
Create a containerized development environment configured for Python 3.10, utilizing a `Dockerfile` and a `devcontainer.json` file for setup. The Python environment, defined by a requirements file, must explicitly include the Pandas library with a version constraint of >= 1.5 and < 2.1.

## Asynchronous Learning
You are required to complete the [Learning GitHub Codespaces for Enterprise](https://www.linkedin.com/learning/learning-github-codespaces-for-enterprise) video series.  

The video series, [GitHub Codespaces for Students](https://www.linkedin.com/learning/github-codespaces-for-students), is optional and available for those seeking additional practice and learning.

## Deliverables
A GitHub repository created from the assignment template repository, augmented with the following additions:
- A `.devcontainer` directory.
- A `devcontainer.json` file configured for Codespaces.
- A `requirements.txt` file that specifies the Pandas dependency.
- A `Dockerfile` utilizing the Python 3.10 base image, which includes commands to install the dependencies listed in the `requirements.txt` file.


## Grading Criteria
This lab assignment will be assessed on a pass/fail basis. Successful completion of the assignment according to the specified criteria will result in a grade of 100% (Pass). Failure to meet the requirements will result in a grade of 0% (Fail). Please reach out to the instructor or teaching assistants if you have any questions or need clarification on the assignment requirements.

You can test your environment by running the following command in the terminal:

  ```python -m unittest tests/autograder_tests.py```



## Instructions

```{warning}
In this lab assignment you will be editing JSON and Dockerfile files which are sensitive to indentation.  Please be careful when editing these files to ensure that the indentation is correct.  Incorrect indentation can cause your Codespace to fail to build.
```

### Step 1: Create a GitHub Repository
 - Accept the GitHub Classroom [assignment invitation](https://classroom.github.com/a/15Im3lXo) to create your starter repository.
    ```
    https://classroom.github.com/a/15Im3lXo
    ```


### Step 2: Configure the Development Environment
1. **Create the `.devcontainer` Directory:**
   - Inside your repository, make a `.devcontainer` directory:
     ```
     mkdir .devcontainer && cd .devcontainer
     ```

2. **Create the Dockerfile:**
   - Create a `Dockerfile` in the `.devcontainer` directory.
   - Populate it with the following, ensuring Pandas is installed within the specified version range:
     ```Dockerfile
     FROM python:3.10-slim

     # Set the working directory
     WORKDIR /workspace

     # Install Python packages
     COPY requirements.txt /workspace/
     RUN pip install --no-cache-dir -r requirements.txt
     ```

3. **Create the `devcontainer.json` File:**
   - Still in the `.devcontainer` directory, create a `devcontainer.json` file.
   - Configure it for your development environment:
     ```json
     {
       "name": "Python 3.10 with Pandas Development Environment",
       "build": {
         "context": "..",
         "dockerfile": "Dockerfile"
       },
       "customizations": {
          "vscode": {
              "settings": {
                  "terminal.integrated.shell.linux": "/bin/bash"
              }
          }
        },
       "forwardPorts": [],
       "postCreateCommand": ""
     }
     ```

4. **Modify the `devcontainer.json` File:**
   - Add the following to the `devcontainer.json` file to enable GitHub CLI in your Codespace:
      ```json
      "features": {
        "ghcr.io/devcontainers/features/github-cli:1": {}
      }
      ```
    - Add the following vscode customization to the `devcontainer.json` file to enable the Python extension in your Codespace:
      ```json
      "extensions": ["ms-python.python"],
      ```
    - Your **final** `devcontainer.json` file should look like this:
      ```json
      {
        "name": "Python 3.10 with Pandas Development Environment",
        "build": {
            "context": "..",
            "dockerfile": "Dockerfile"
        },
        "customizations": {
          "vscode": {
              "settings": {
                  "terminal.integrated.shell.linux": "/bin/bash"
              },
              "extensions": [
                  "ms-python.python"
              ]
          }
        },
        "features": {
          "ghcr.io/devcontainers/features/github-cli:1": {}
        },
        "forwardPorts": [],
        "postCreateCommand": ""
      }
      ```
6. **Create the `requirements.txt` File:**
   - In the root of your repository, create a `requirements.txt` file.
   - Add the following to the `requirements.txt` file:
     ```
     pandas>=1.5,<2.1
     ```

### Step 3: Commit Your Changes
After configuring your environment, add, commit, and push your changes:

  ```
  git add .
  git commit -m "Added Python 3.10 development environment with Pandas"
  git push
  ```

### Step 4: Launch Your Codespace
- Go to your GitHub repository online.
- Click "Code" > "Open with Codespaces" > "New codespace".

### Step 5: Verify Your Environment

Once your Codespace is ready, open a terminal and run the following command:
  ```
  python -m unittest tests/autograder_tests.py
  ```

If the test passes, it means your environment is configured correctly. The output will indicate that 4 tests were run successfully with the message 'OK', as shown below:
  ```
  ----------------------------------------------------------------------
  Ran 4 test in 1.000s

  OK
  ```

If the test fails, please reread the instructions. A failure will be clearly marked in the output, stating "FAILED" along with the number of tests that failed. For example:
  ```
  ----------------------------------------------------------------------
  Ran 4 test in 1.000s

  FAILED (failures=1)
  ```

### Step 6: Submit Your Assignment to CANVAS

```{warning}
Your grade will be based on your final commit to the **main** branch of your GitHub repository.  Modifications made directly to files within the GitHub Codespace are not saved to your GitHub repository unless you commit and push them. 
```

After verifying that your environment is correctly configured, and you have committed all files to your **main** branch, please submit the URL to your GitHub repository to the Canvas assignment.
