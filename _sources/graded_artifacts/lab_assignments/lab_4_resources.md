# Lab 4 Resources

These resources are provided to help you complete the lab assignment.  These will help you understand the concepts and techniques used in the lab.  Resources are provided by task number and are intended to be used as a reference while working sequentially through the lab assignment.


## Task 2: Grade Conversion Function

- **Raising Exceptions**: You can raise exceptions in Python using the `raise` keyword.  When raising an exception, you can specify the type of exception and a message to display. Example:

    ```python
    raise ValueError("Invalid input. Please enter a number between 0 and 100.")
    ```

  For more information see [Python Raise an Exceptions]https://www.w3schools.com/python/gloss_python_raise.asp).

- **Google Docstrings**: The Google Docstrings format is one of many styles you can use when documenting your functions.  Google Docstrings are written between triple quotes and include a description of the function, the parameters, and the return value.  Here is an example of a Google Docstring that includes a description, parameters, exceptions, and return value:

    ```python
    def sum_numbers(a, b):
        """
        This function takes two numbers as input and returns the sum.
        
        Parameters:
        a (int/float): The first number.
        b (int/float): The second number.
        
        Raises:
        TypeError: If either a or b is not a numeric type (int or float).
        
        Returns:
        int/float: The sum of the two numbers.
        """
        if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
            raise TypeError("Both parameters must be numeric types (int or float).")
        
        return a + b
    ```

  For additional information, please refer to Section 3.8, "Comments and Docstrings," in the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html).

## Task 3: Grade Conversion Unit Tests

You are required to use the `pytest` library to write unit tests for the `convert_to_letter_grade` function.  The `pytest` library is a testing framework that makes it easy to write simple and scalable tests. 


- **Writing Tests**: To create tests you simply create a module with functions that start with `test_`.  Here is an example of a simple test function:

    ```python
    def test_sum_numbers():
        assert sum_numbers(1, 2) == 3
        assert sum_numbers(0, 0) == 0
        assert sum_numbers(-1, 1) == 0
    ```

  You can add a message to the `assert` statement to provide more information about the test.  Here is an example:

    ```python
    def test_sum_numbers():
        assert sum_numbers(1, 2) == 3, "Test failed: sum_numbers(1, 2) should return 3."
        assert sum_numbers(0, 0) == 0, "Test failed: sum_numbers(0, 0) should return 0."
        assert sum_numbers(-1, 1) == 0, "Test failed: sum_numbers(-1, 1) should return 0."
    ```

  When you need to test for exceptions, you can use the `pytest.raises` context manager.  Here is an example of how to test for a `TypeError` exception:

    ```python
    with pytest.raises(TypeError):
        sum_numbers("a", 2)
    ```

  If you want to test for a specific message in the exception, you can use the `match` keyword argument.  Here is an example:

    ```python
    with pytest.raises(TypeError, match="Both parameters must be numeric types (int or float)."):
        sum_numbers("a", 2)
    ```

  Or alternatively, you can use the `pytest.raises` context manager with the `exc_info` attribute to check the exception message.  Here is an example:

    ```python
    with pytest.raises(TypeError) as exc_info:
        sum_numbers("a", 2)
    assert str(exc_info.value) == "Both parameters must be numeric types (int or float)."
    ```

  Documentation for the `pytest` library can be found at [pytest Documentation](https://docs.pytest.org/en/6.2.x/). You can also review the [pytest Quick Start Guide](https://docs.pytest.org/en/6.2.x/getting-started.html) for more information.


## Task 4: GitHub Actions Pytest Workflow

- **Workflow Trigger**: The GitHub Actions workflow can be triggered by many different events, such as push, pull request, or schedule.  You can specify the event that triggers the workflow in the `on` section of the workflow file. For more information see:

  - [Events that Trigger Workflows](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).
  - [Triggering a workflow](https://docs.github.com/en/actions/using-workflows/triggering-a-workflow)

- **Workflow Trigger Filters**: You can use filters for further customization of the workflow trigger.  For example, you can specify the branches that trigger the workflow.  Here is an example of a workflow that triggers on push events for the `main` and `develop` branches:

    ```yaml
    on:
      push:
        branches:
          - main
          - develop
    ```

  Or use exclude filters to exclude branches from triggering the workflow.  Here is an example of a workflow that triggers on push events for all branches except `main` and `develop`:

    ```yaml
    on:
      push:
        branches:
          - '*'
        branches-ignore:
          - main
          - develop
    ```
  For more information see:

  - [Filter pattern cheat sheet](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet).
  - [Using Filters](https://docs.github.com/en/actions/using-workflows/triggering-a-workflow#using-filters).

- **Matrix Strategy**: You can use a matrix strategy to run the workflow on multiple operating systems, Python versions, or other parameters.  Here is an example of a matrix strategy that runs the workflow on Ubuntu, Windows, and macOS:

    ```yaml
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    ```

  And here is an example of a matrix strategy that runs the workflow on multiple Python versions:

    ```yaml
    strategy:
      matrix:
        python-version: [3.6, 3.7, 3.8, 3.9]
    ```

  For more information see [Using a matrix for your jobs](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs).

- **Run Commands**: You can run multiple commands in a job by specifying them in the `run` section of the workflow file. Here is an example of a job that runs multiple commands:

    ```yaml
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout repository
            uses: actions/checkout@v2
          - name: Set up Python
            uses: actions/setup-python@v2
            with:
              python-version: '3.x'
          - name: Install dependencies
            run: |
              python -m pip install --upgrade pip
              pip install -r requirements.txt
          - name: Run tests
            run: |
              pytest
    ```

  In the above example, the `run` section contains multiple commands separated by a newline character.  For more information see [Workflow commands for GitHub Actions](https://docs.github.com/en/enterprise-cloud@latest/actions/using-workflows/workflow-commands-for-github-actions).

## Task 5: GitHub Actions Sphinx Workflow

In this task we use the `sphinx` library to generate documentation for the Python code.  The `sphinx` library can be used to create documentation for Python modules and packages by converting `docstrings` in the code to HTML pages.  We will not delve into the details of the `sphinx` library in this lab, but you can find more information in the [Sphinx Documentation](https://www.sphinx-doc.org/en/master/).

- **Artifacts**: You can use the `actions/upload-artifact` action to upload artifacts to GitHub.  Artifacts are files that are produced by a workflow and can be used in subsequent jobs or steps.  Here is an example of how to upload a file `api_docs.zip` as an artifact:

    ```yaml
    - name: Upload artifact
      uses: actions/upload-artifact@v2
      with:
        name: sphinx-docs
        path: api_docs.zip
    ```

  For more information see [Storing workflow data as artifacts](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts).

## Task 6: GitHub Actions Badge

- **Workflow Status Badge**: You can add a badge to your repository to display the status of your GitHub Actions workflows.  The badge provides a visual indicator of the status of the workflow, such as passing, failing, or in progress.  You can add the badge to your `README.md` file by using the following markdown code:

    `![example event parameter](https://github.com/github/docs/actions/workflows/main.yml/badge.svg?event=push)`

  This example displays the status of the workflow in the `main.yml` file for the `push` event.  For more information see [Adding a workflow status badge](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge)

## Task 7: Verify It Works

- **Downloading Artifacts**: You can download artifacts from a workflow run by clicking on the `Artifacts` tab in the workflow run details.  The artifacts are stored in a zip file that you can download to your local machine.  For more information see [GitHub Actions Artifacts](https://earthly.dev/blog/github-action-artifacts/).

