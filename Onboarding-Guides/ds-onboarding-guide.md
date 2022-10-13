# Underdog Devs | Data Science Onboarding Guide
Last Updated: Oct 10, 2022

## Contents
- [Introduction](#introduction)
- [Setup](#setup)
  - [IDE](#ide)
  - [Cloning the Repo](#cloning-the-repo) 
  - [Environment Vars](#environment-vars)
- [Repo Walkthrough](#repo-walkthrough)

## Introduction
Welcome to the DS section of the Underdog Devs project in Bloomtech Labs!

While using this guide, you should also view the 
[official README](https://github.com/BloomTech-Labs/underdog-devs-ds-a/blob/main/README.md) for additional information.

The purpose of the DS section of the UD project is to handle the storage, analytics, and modeling of all data related
to the project (except authentication data for FE/BE).

It consists of two main components:
- **MongoDB**
  - The Mongo database is used by all, and should be handled with some amount of care
  - PURPOSE: Store all information not associated with user authentication (profiles, meetings, feedback, etc)
- **DS Codebase (API)**
  - The DS codebase itself is an API created in Python with FastAPI, pydantic, and pymongo
  - PURPOSE: Be the interface for web (BE) to interact with MongoDB using a 
  [REST API](https://medium.com/@mwaysolutions/10-best-practices-for-better-restful-api-cbe81b06f291)

You will be setting up a local instance of the codebase/API on your machine in the next section.

## Setup
This section will guide you through setting up your local environment for working on the UD project.

There are also videos available covering various parts of this section:
- ["Official" Video by DS Manager Robert Sharp](https://www.youtube.com/watch?v=R6nxaN0HegA)

### IDE
While you are free to use any IDE of your choice, it is recommended that you use PyCharm. It is what Robert, the
DS Manager, will be using in Workshops and is ready 'out-of-the-box' for working on python projects.

There is a free version, the [Community Edition](https://www.jetbrains.com/pycharm/download/), that is more than 
suitable for our purposes in this project.

### Cloning the Repo
For this project, you want to directly clone the repo from the Bloomtech-Labs page on GitHub. ***DO NOT FORK***

```bash
git clone git@github.com:BloomTech-Labs/underdog-devs-ds-a.git
cd underdog-devs-ds-a
```

Now that we have our local repo, we can set up our virtual Python environment:

```bash
# Initialize virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate

# Install all dependencies for project into virtual environment
pip install -r requirements.txt
```

Finally, we need to add 'execute' permissions to our `run.sh` script. The `run.sh` script allows us to run a local
instance of the API with any changes to the code that we have made locally.

```bash
# Grant execute permissions for all users on file 'run.sh'
chmod +x run.sh

# Run the 'run.sh' script
./run.sh
```


### Environment Vars
Create a `.env` file in your local repository with the following contents:
```bash
CONTEXT=local
```
The `CONTEXT` variable is what is needed to get the 'secrete password' for the Module 1 quiz in Canvas.

```bash
MONGO_URL=mongodb+srv://*******:********@*******.mongodb.net
```
`MONGO_URL` is the connection string to the mongodb server that we all use and share. See the Environment Variables
link on the Product Resources page for the full connection string.


## Repo Walkthrough
Each directory and file of note is described here. This should be periodically updated as changes occur, and should
only be considered a high-level view of the codebase.

### `/app/`
This is where the code for the API (DS's end product) lives!
  - `routers/` Where the code for each API endpoint is kept
  - `app.py` The 'core' of our API lives here, where all the routers from `routers/` is imported
  - `data.py` A wrapper for pymongo that abstracts our interactions with MongoDB across the rest of the app
  - `graphs.py` Code for generating graphs from data in the database
  - `model.py` Code for the Mentor/Mentee matching model
  - `schema.py` Data validation schema for pydantic. This is what FastAPI and pydantic uses to validate data sent 
  - to the API endpoints
  - `sentiment.py` Code for text sentiment analysis

### `/data_generators/`
Generates mock data to seed the database with while we are developing and testing the application.

### `/notebooks/`
Exploratory notebooks

### `/tests/`
Unit and Integration tests

### `/run.sh`
Run an instance of the API locally. You can access it at `127.0.0.1:8000`

### `/seed.sh`
Reseed the MongoDB database. This is the same database for everybody, so don't abuse it (too much)!!!