# JDEV 2026 Python Workshop



# RSA Encryption & Breaking Demo (Educational)

This project is an **educational Python workshop** designed to illustrate how RSA encryption works and why **small or poorly implemented RSA keys are insecure**.

The prime numbers are extracted from https://t5k.org/lists/small/millions/

-> Fork and clone the repository. Create a new virtual environment and install the package to test it.

```bash
python -m venv rsa_demo_env
source rsa_demo_env/bin/activate  # On Windows: rsa_demo_env\Scripts\activate
pip install numpy
decrypt_rsa.py
```

# 1st step: Create the architecture:

To create the architecture of the package, the python files are inside the folder <package_name> and the README.md file is outside of it. __init__.py file is needed to make the folder a package and to be able to import automaticaly the modules inside it.

-> From the branch workshop_starting_point, move the scripts to the folder <package_name> with your name, add __init__.py files and change the path of the data to be able to load it from the package in decrypt_rsa.py. You can use the following code to load the data from the package:

```python
data_path = files("jdev2026_python_wheel_XXXXXX.data") / "primes.npz"
data = np.load(data_path)
```

# 2nd step: Create the pyproject.toml file:

pyproject.toml is a configuration file used to create wheels. It is used to specify the build system and the dependencies of the package. It is a standard file that is used by all Python packages. It is also used to specify the metadata of the package, such as the name, version, description, etc.

-> From the model, complete the pyproject.toml file, change the name of the package and add the dependencies

-> Fist try to build the package (see step 5 and 6).

# 3rd step: Add the data files to the package:

For the moment, the data files are not included in the package, we need to add them to the package to be able to load them from the package.
To add the data files to the package, we need to specify the data files in the MANIFEST.in (for the archive) and pyproject.toml (for the wheel) files. 

-> Create a MANIFEST.in file, complete it to include the data files in the archive. Uncomment and complete the lines in the pyproject.toml related to package-data:

# 4th step: Add the entry point to the package:

For the moment, we can only run the scripts by importing them in a python file as a librairy or in the terminal. We cannot run them directly from the terminal.
To be able to run the scripts directly from the terminal, we need to add an entry point to the package. An entry point is a command that is executed when we run a specific command in the terminal. For example, if we want to run the script decrypt_rsa.py by running the command decrypt_rsa in the terminal, we need to add an entry point for this command.
To add the entry point to the package, we need to specify the entry point in the pyproject.toml file. We will use the entry_points section to specify the entry point of the package.

-> Uncomment and complete the lines in the pyproject.toml related to scripts:

# 5th step: Build the package locally:

To build the package locally, we need to run the following command in the terminal:

```bash
pip install build
python -m build
ls dist
```

This will create a dist folder with the built package. We can then install the package locally using pip:

```bash
pip install dist/<package_name>-<version>.whl
```

It's better to have a recent version of pip and setuptools to avoid any issues when building the package. You can upgrade them using the following command:

```bash
pip install --upgrade pip setuptools
```

# 6th step: Test the package:

To test the package, it's better to create a new virtual environment and install the package in it. Then we can run the entry point of the package to see if it works correctly.

```bash
python -m venv test_env
source test_env/bin/activate  # On Windows: test_env\Scripts\activate
pip install dist/<package_name>-<version>.whl
<entry_point_command>
```

# 7th step: .gitignore file:

There is a lot of files that are generated when we build the package, we don't want to commit them to the repository. We can create a .gitignore file to ignore these files.

-> Create a .gitignore file and add the files and folders that you want to ignore. Do not forget to ignore .gitignore file itself.

# 8th step: Publish the package on PyPI:

To publish the package on PyPI (test), you need to create an account on test PyPI (https://test.pypi.org/). In "Account Settings", you can find your API token. Then tag the version of your package in the pyproject.toml and with git:

```bash
git tag -a 0.1.0 -m "tag 0.1.0"
git push --tags
python -m build
```

Finally run the following commands to upload the wheel on pypi:

```bash
pip install twine
twine upload --repository testpypi dist/*
```

# 9th step: Automatization with Github Actions:

To automatize the build and publish process, we can use Github Actions. We can create a workflow that will be triggered when we push a new commit or a new tag to the repository. The workflow will build the package and publish it on test PyPI. You will need to create a secret in the Github repository settings (settings/secrets/actions) to store your API token for test PyPI.

-> From the model, complete the file .github/workflows/publish.yml and modify it to fit your package. You will need to add your API token as a secret in the Github repository settings called TEST_PYPI_API_TOKEN. Push everything on Github and check if the workflow is triggered and if the package is published on test PyPI.

# Last step: Test everything:

To test the automatization, you can create a new tag and push it to the repository (see step 8). This will trigger the workflow and publish the package on test PyPI. Then you can create a new virtual environment and install the package from test PyPI to see if it works correctly.

```bash
python -m venv test_env
source test_env/bin/activate  # On Windows: test_env\Scripts\activate
pip install --index-url https://test.pypi.org/simple/ <package_name>
<entry_point_command>
```
