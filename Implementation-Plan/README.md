# Underdog Devs Resource Management Application Deployment Guide

*Last Updated: Oct 6, 2022*

## Contents
- [What is this document?](#purpose)
- [How to use this guide](#how-to-use-this-guide)
- [Further Information](#further-information)

## Purpose
This is a living document describing how to deploy the Resource Management Application
developed for Underdog Devs by Bloomtech Labs.

It covers the deployment process step-by-step from beginning to end, and should be followed in the order described.

## How to use this guide

This guide is broken into 3 sub-documents that should be followed in order:

1. Data Science: [1-ds-deployment.md](./1-ds-deployment.md)
2. Backend: [2-be-deployment.md](./2-be-deployment.md)
3. Frontend: [3-fe-deployment.md](./3-fe-deployment.md)

The order was determined by the dependencies required by each section. The frontend is dependent on the
backend, and both frontend and backend are dependent on the data science section of the app. The data science section is 
not dependent on any other section, and is therefore the best place to start deployment.

## Further Information

If the need for further information or context arises, reference the documents in the 
[Project-READMEs](../Project-READMEs) directory. 
