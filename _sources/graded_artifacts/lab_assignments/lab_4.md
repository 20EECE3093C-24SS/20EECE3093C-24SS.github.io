# Lab 4 - Software Development Practices

**Due Date:** Tuesday, April 23, 2024, at 11:59 PM

## Objective

This lab is designed to help you practice key aspects of good software development practices, including writing unit tests, using GitHub Actions for continuous integration, and generating documentation with Sphinx. You will be tasked with writing a Python function that converts a student's numerical score into a corresponding letter grade. Additionally, you will write a comprehensive set of unit tests to verify the function's behavior and edge cases. Finally, you will configure GitHub Actions workflows to run the unit tests and generate documentation with Sphinx, and you will add badges to the README file to display the status of the GitHub Actions workflows.

## Grading Rubric

The lab is worth **100 points** and will be graded according to the following components:

- **Function Implementation (30 points):**
  - The `convert_to_letter_grade` function is correctly implemented.
  - The function correctly converts numerical scores to letter grades.
  - The function raises the appropriate exceptions for invalid input values.
  - The function is documented with a Google Python Style Guide docstring.
- **Unit Tests (30 points):**
  - The unit tests cover all aspects of the `convert_to_letter_grade` function.
  - The tests verify the function's behavior at exact grade boundaries.
  - The tests validate the function's handling of invalid input values.
- **GitHub Actions Pytest Workflow (15 points):**
  - The Pytest workflow runs successfully on push events to the main branch.
  - The workflow uses a matrix strategy to test the code against different Python versions.
  - The workflow installs the Python dependencies specified in the requirements.txt file.
  - The workflow runs the Pytest command to execute the unit tests.
- **GitHub Actions Sphinx Workflow (15 points):**
  - The Sphinx workflow runs successfully on push events that include a tag.
  - The workflow uses Python 3.11 to build the Sphinx documentation.
  - The workflow installs the Python dependencies specified in the requirements.txt file.
  - The workflow runs the `make html` command to generate the Sphinx documentation.
- **GitHub Actions Badges (10 points):**
  - The Pytest badge displays the status of the Pytest workflow.
  - The Sphinx badge displays the status of the Sphinx workflow.


## Task 1: Accept GitHub Classroom Assignment

Accept the GitHub Classroom assignment by clicking on the following link:

https://classroom.github.com/a/Gc-fowPs

This link will create a private repository for you on GitHub, where you will complete the lab assignment.

## Task 2: Grade Conversion Function

Your first task is to write a Python function that converts a student's numerical score into a corresponding letter grade. This function should be added to the **sample/course_grader.py** file below the line that reads **# TODO-1: add convert_to_letter_grade(score) function**.

### Function Requirements:

- **Function Name:** The function must be correctly named `convert_to_letter_grade`.
- **Input Argument:** It should accept a single parameter named `score`, representing the student's numerical score to be converted into a letter grade.
- **Output:** The function is expected to return an **upper-case** string indicating the student's letter grade, determined based on the following score ranges:
  - **A:** 90 to 100
  - **B:** 80 to 89
  - **C:** 70 to 79
  - **D:** 60 to 69
  - **F:** Below 60
- **Invalid Input:**
  - The function must raise a `ValueError` with the message "Score must be between 0 and 100." for input scores outside the 0 to 100 range.
  - The function must raise a `TypeError` with the message "Score must be a numeric value." if the input score is not an `int` or `float`.
- **Docstring:** The function must be documented through a docstring that follows the Google Python Style Guide. The docstring must have a description of the function, specifying the nature of its inputs, outputs, and the exceptions it raises.

## Task 3: Grade Conversion Unit Tests

The second task involves creating a comprehensive set of unit tests for the `convert_to_letter_grade` function to confirm it adheres to all the specified requirements and correctly handles edge cases. These tests should be placed in the **tests/test_course_grader.py** file.

### Test Requirements:

- **Testing Library:** You must use the **pytest** library for writing the unit tests. The provided **requirements.txt** file already includes the `pytest` package with version constraints. Do not modify the **requirements.txt** file.
- **Test Exact Boundaries:** Below the line that reads **# TODO-1: Add test_exact_grade_boundaries() function**, add a method named `test_exact_boundaries` to check the function's behavior at the precise score boundaries for each letter grade. Examples of the exact boundaries include scores like 0, 59, 60, ..., 89, 90, and 100. This method should contain a total of **9 test cases**.
- **Test Invalid Input Value:** Below the line that reads **# TODO-2: Add test_invalid_numerical_score() function**, add a method named `test_invalid_numerical_score` to verify that the function raises a `ValueError` when provided with scores outside the permissible range of 0 to 100. Additionally, you must validate that the message in the exception matches the expected error message (e.g., "Score must be between 0 and 100." for `ValueError`). There should be **2 test cases** for this method, but remember to check both the error type and the message returned by the exception.
- **Test Invalid Input Type:** Below the line that reads **# TODO-3: Add test_non_numeric_input() function**, add a method named `test_non_numeric_type` to verify the function raises a `TypeError` when given inputs that are not valid numerical values. Test at least one case for each of the following types: `string`, `list`, and `None`. Additionally, validate that the message in the exception matches the expected error message ("Score must be a numeric value."). There should be **3 test cases** for this method, but remember to check both the error type and the message returned by the exception.

## Task 4: GitHub Actions Pytest Workflow

GitHub Actions workflows are located in the **.github/workflows** directory. The workflow defined by **ci-pytest.yaml** runs tests using Pytest whenever a new commit is pushed to the repository. The workflow requires modifications to ensure it runs the tests correctly.

### Pytest Workflow Updates:

Update the **ci-pytest.yaml** workflow as follows:

- **Workflow Trigger:** The workflow should trigger on **push** events to the **main** branch, but only when the push event does not include a **tag**. This can be accomplished by using the **branches** filter and the **tags-ignore** filter. Replace **TODO-1** with `'main'` to match **push** events on the **main** branch and **TODO-2** with `'*'` to match any **tag**. The updated trigger should look like the following:

    ```yaml
    on:
      push:
        branches:
          - 'main'
        tags-ignore:
          - '*'
    ```

- **Matrix Strategy:** The workflow should use a matrix strategy to test the code against different versions of Python. The matrix should include the following Python versions: 3.10 and 3.11. Replace the placeholder **TODO-3** with `'3.10'` and **TODO-4** with `'3.11'`. The updated matrix configuration should look like the following:

    ```yaml
    strategy:
      matrix:
        python-version: ['3.10', '3.11']
    ```

- **Python Dependencies:** The workflow should install the Python packages specified in the **requirements.txt** file. Replace the placeholder **TODO-5** with the command `pip install -r requirements.txt`. The updated Python requirements should look like the following:

    ```yaml
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    ```

- **Run Pytest:** We want **pytest** to run tests from any module located in the **tests** directory. This can be accomplished by calling `pytest tests/`. Replace the placeholder **TODO-6** with the command `pytest tests/`. The YAML definition to run Pytest should look like the following:

    ```yaml
    - name: Test with pytest
      run: pytest tests/
    ```

## Task 5: GitHub Actions Sphinx Workflow

GitHub Actions workflows are located in the **.github/workflows** directory. The workflow defined by `ci-sphinx.yaml` runs Sphinx to automatically generate HTML documentation from the Python docstrings in the repository. The workflow requires modifications to ensure it runs the tests correctly.

### Sphinx Workflow Updates:

Update the `ci-sphinx.yaml` workflow as follows:

- **Workflow Trigger:** The workflow should trigger on **push** events that include a **tag**. This can be accomplished by using the **tags** filter. Replace **TODO-1** with `'v*.*.*'` to match any **tag** that follows the semantic versioning format (MAJOR.MINOR.PATCH). The updated trigger should look like the following:

    ```yaml
    on:
    push:
      tags:
        - 'v*.*.*'
    ```

- **Python Version:** For this workflow, we don't need to use a matrix strategy; we only need to run the workflow using Python 3.11. Replace the placeholder **TODO-2** with `'3.11'`. The updated Python version should look like the following:

    ```yaml
    - name: Setup Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.11'
    ```

- **Python Dependencies:** The workflow should install the Python packages specified in the **requirements.txt** file. Replace the placeholder **TODO-3** with the command `pip install -r requirements.txt`. The updated Python requirements should look like the following:

    ```yaml
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    ```

- **Make HTML:** We need to run the `make html` command to build the Sphinx documentation. Replace the placeholder **TODO-4** with the command `make html`. The YAML definition to run Sphinx should look like the following:

    ```yaml
    - name: Build HTML documentation
      run: |
        sphinx-apidoc -o docs/source/ sample/
        cd docs
        make html
        cd build
        zip ../../api_docs.zip -r html/
    ```

## Task 6: GitHub Actions Badge

You can use GitHub Actions badges to display the status (e.g., passing, failing) of any defined GitHub Actions workflows. This is done by adding a badge URL to the **README.md** file.

The badge consists of two parts: one part is the status image URL, and the second part is a link to the GitHub Actions workflow page. Clicking the badge will take you to the GitHub Actions workflow page where you can see the status of the workflow.

### README Badge Updates:

Update both badges in the **README.md** file as follows:

- **Pytest Badge:** Replace both instances of `TODO-1` with your GitHub username, and both instances of `TODO-2` with the repository name.
- **Sphinx Badge:** Replace both instances of `TODO-3` with your GitHub username, and both instances of `TODO-4` with the repository name.

## Task 7: Verify It Works

After completing the tasks, you should verify everything is working:

1. Both GitHub Actions workflows should run successfully and should display a green checkmark.
2. The Pytest badge should display `passing` and link to the GitHub Actions workflow page. This is what the badge should look like:

    ```{image} /images/github_badge_actions_pytest.svg
    :alt: unit test
    ```

3. The Sphinx badge should display `passing` and link to the GitHub Actions workflow page. This is what the badge should look like:

    ```{image} /images/github_badge_actions_sphinx.svg
    :alt: docs generation
    ```

4. The GitHub Actions workflow page should have an artifact named `api_docs.zip` that contains the HTML documentation generated by Sphinx.

## Task 8: Review the HTML Documentation

After the Sphinx workflow runs successfully, you should review the HTML documentation generated by Sphinx. Download the **api_docs.zip** artifact from the GitHub Actions workflow page. Extract the contents of the zip file and open the **index.html** file in a web browser. You should see documentation for the `course_grader` module and the `google_docstrings` module.

## Task 9: Submit Your Assignment to CANVAS

After completing the tasks, you should submit your assignment to CANVAS. The assignment should include the following:

- A link to your GitHub repository.
- The **api_docs.zip** artifact generated by the Sphinx workflow.