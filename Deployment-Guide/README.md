# UDRM Deployment Guide

*Last Updated: Oct 7, 2022*

## Contents
- [What is this document?](#what-is-this-document)
- [How to use this guide](#how-to-use-this-guide)
- [Further Information](#further-information)

## What is this document?
This is a collection of living documents describing how to deploy the Resource Management Application
developed for Underdog Devs by Bloomtech Labs.

It covers the deployment process step-by-step from beginning to end, and should be followed in the order described.

## How to use this guide

This guide is broken into 3 sub-documents that should be followed in order:

1. Data Science: [1-ds-deployment.md](./1-ds-deployment.md)
2. Backend: [2-be-deployment.md](./2-be-deployment.md)
3. Frontend: [3-fe-deployment.md](./3-fe-deployment.md)

The order is determined by the dependencies required by each section. The frontend is dependent on the
backend, and both frontend and backend are dependent on the data science section of the app. The data science section is 
not dependent on any other section, and is therefore the best place to start deployment.

## Further Information

If the need for further information or context arises, you may find the documents in 
[Project-READMEs](../Project-READMEs) helpful.
