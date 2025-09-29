# How to Configure VS Code to Easily Develop Airflow DAGs with Docker
By default VS Code does not have access to Airflow running inside docker container. This setup will help you overcome import errors in the code editor and enable auto completion for your code for smooth DAG development.

## Step 0: Create a Python virtual environment
In the same directory where you have your docker compose file, create a Python virtual environment with these commands:

```
python3 -m venv <your virtual env name>
```

## Step 1: Activate the environment
Run this command to activate the virtual environment

For Linux/Mac OS:  
```
source <virtualenvname>/bin/activate
```

For Windows OS: 

```
<virtualenvname>\Scripts\activate
```

## Step 2: Install Apache Airflow
Run the following command to install Airflow:

```
pip install apache-airflow
```

## Step 3: Setup Intellisense
If you don't have Python intellisense setup on your vscode, then do this, else skip.

Install the Python microsoft extension for vscode:

Go to the extensions tab, search for Python, click on it and click on install.

![intellisense](images/intellisense.png)

---

**Well done, you have successfully setup your vscode for airflow DAGs development with docker**