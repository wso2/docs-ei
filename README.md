# WSO2 Enterprise Integrator Documentation

This is the source documentation for the latest release of the WSO2 Enterprise Integrator 7 family (i.e., 7.1.0).

To see the **latest released documentation** for the WSO2 Enterprise Integrator 7 family, go to: https://ei.docs.wso2.com/en/latest/

## Contributing to WSO2 EI documentation

As an open source project, WSO2 EI welcomes contributions from the community. To start contributing, read these contribution guidelines for information on how you should go about contributing to our project.

1. Accept the Contributor License Agreement (CLA)
    
    You need to Accept the Contributor License Agreement (CLA) when prompted by a GitHub email notification after sending your first Pull Request (PR). Subsequent PRs will not require CLA acceptance.

    If the CLA gets changed for some (unlikely) reason, you will be presented with the new CLA text after sending your first PR after the change.

2. Fork this repository, make your changes, and send in a pull request. Make sure you are contributing to the correct branch (for example, if your change is relevant to WSO2 EI 7.1.0 documentation, you should commit your changes to the 7.1.0 branch).

Check the issue tracker for open issues that interest you. We look forward to receiving your contributions.

## Building the documentation locally

### 1. Install Python. 

If you are using MacOS, you probably already have a version of Python installed on your machine. You can verify this by running the following command.

`python --version`
`Python 2.7.2`


If your version of Python is Python 2.x.x, you also need to install Python3. This is because the PDF plugin only supports Python3. Follow the instructions on [this guide](https://docs.python-guide.org/starting/install3/osx/) to install Python3 properly. 

Once you are done, you will have two versions of Python on your machine; a version of python2 and a version of python3. 


### 2. Install Pip. 

Pip is most likely installed by default. However, you may need to upgrade pip to the latest version:

`pip install --upgrade pip`

If pip is not already installed on your machine, download get-pip.py to install pip for the first time. Then run the following command to install it:

`python get-pip.py`

### 3. Install pip packages

`pip install mkdocs`

`pip install mkdocs-material`

`pip install markdown-include`

`pip install pymdown-extensions`

### 4. Run mkdocs 

Clone this GitHub repository.

Navigate to the branch you want and the directory you want. For example, `cd docs-ei/en/micro-integrator/` in the `7.1.0` branch. Then run the following command:

`mkdocs serve --dirtyreload`
  
Open the URL that is generated in a new browser window to view the sample site. 

