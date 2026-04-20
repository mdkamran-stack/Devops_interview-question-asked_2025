# what is Gitops?
## Gitops uses Git as a single source of truth to deliver applications and infrastructure. 

It basically keeps the state between Git & Argocd.    

# Advantages of Gitops
- Security
- Versioning (track of chages)
- Auto upgrades
- Auto Healing of any unwanted changes
- Continous Reconciliation  

# Declarative : A system managed by Gitops must have its desired state expressed declaratively  

# Versioned and immutable : Desired state is stored in a way that enforces immutability,versioning and retains a complete version history. 

# Pulled Automatically: Software agents automatically pull the desired state declarations from the source.  

# Contionous Reconciled: Software continously observe actual system state and attempt to apply the desired state.  

# why are we using githubaction

## we are on ecosystem already on github project is lreaady on github we manage everything on github 

# How do you secure sensitive information on Github  
## We use Secretes and variables to secure informations  

# how do you create a ci file in github  
## In the repository itself we create .github/workflows folder and after that steps 

# CI/CD Pipeline using GitHub Actions

This pipeline builds a Java application, creates a Docker image, pushes it to Docker Hub, and deploys it to Kubernetes.

---

## 📌 Workflow Definition

```yaml
name: CI-CD Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Setup Java
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: Build Application
      run: mvn clean package

    - name: Build Docker Image
      run: docker build -t my-app:${{ github.sha }} .

    - name: Login to Docker Hub
      run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

    - name: Push Image
      run: docker push my-app:${{ github.sha }}

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Deploy to Kubernetes
      run: kubectl apply -f k8s/deployment.yaml
1️⃣ Scenario: GitHub Actions Workflow Fails Due to Missing Secrets

Problem:
 Your team created a CI/CD pipeline using GitHub Actions to deploy an application to AWS. Suddenly the workflow started failing during deployment with the error:

Error: AWS credentials not found

The pipeline was working previously.

Root Cause:
 The workflow was trying to access AWS credentials stored in GitHub Secrets, but the secret name was either deleted, renamed, or incorrectly referenced in the workflow file.

Solution:
1. Go to Repository → Settings → Secrets and variables → Actions
2. Verify the required secrets exist:
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
3. Ensure workflow references them correctly:

env:
 AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
 AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }

4. Re-run the workflow.
Best Practice:
 Use environment-specific secrets for staging and production to avoid accidental exposure.

2️⃣ Scenario: Workflow Job Stalls Without Errors
Problem: Nightly build suddenly stalls at a step (e.g., long npm install), hits timeout, no logs. Worked fine yesterday! 😩

Quick Diagnosis: Check runner logs for hung processes; network flakes or resource exhaustion.

Solution:
Add timeout-minutes: 30 to jobs/steps.
Use continue-on-error: true for non-critical steps.
Pin runner: runs-on: ubuntu-latest → ubuntu-22.04.

Example YAML:
job:
 timeout-minutes: 60
 steps:
 - run: npm ci
 timeout-minutes: 20

3️⃣ Scenario: GitHub Actions Pipeline Taking Too Long
Problem:
Your CI pipeline using GitHub Actions takes 20 minutes to complete, delaying developer feedback.

Root Cause:
 Every run installs dependencies from scratch.
Example:
npm install
runs every time.

Solution: Use Dependency Caching
- name: Cache Node Modules
 uses: actions/cache@v3
 with:
 path: ~/.npm

Result:
 Pipeline time reduced from 20 minutes → 6 minutes.
Best Practice: Cache dependencies like: npm, Maven, Gradle, Docker layers

4️⃣ Scenario: Deployment Happening Even When Tests Fail
Problem:
 Your CI/CD workflow in GitHub Actions deploys to production even when unit tests fail.

Root Cause:
 Deployment job was not dependent on the test job.

Solution: Use Job Dependencies
jobs:
 test:
 runs-on: ubuntu-latest
 deploy:
 needs: test
 runs-on: ubuntu-latest
Now deployment runs only if tests pass.

5️⃣ Scenario: Multiple Developers Triggering Deployment Simultaneously
Problem:
 In a busy repository using GitHub Actions, multiple developers triggered workflows simultaneously, causing multiple deployments to production.

Risk:
 Production instability and version conflicts.

Solution: Use Concurrency Control
concurrency:
 group: production-deploy
 cancel-in-progress: true
Result:
Only one deployment runs at a time
Previous deployments automatically cancel


      - name: Verify Deployment
        run: kubectl rollout status deployment/my-app
