This repository contains the application source code and the GitHub Actions workflow that drives the entire deployment pipeline.
##

🚀 Pipeline Overview
This repo implements a multi‑stage GitHub Actions workflow:

Build & Test

Static Analysis & SonarQube Scan

Docker Build & Push to ECR

GitOps Helm Update

The workflow is triggered on:

Pull Requests → CI checks + SonarQube

Push to main → Build image + Push to ECR + Update Helm repo

.
├── src/                     # Application source code
├── Docker-files/            # Multi-stage Dockerfile for app build
│   └── app/multistage/
├── .github/workflows/
│   └── ci-cd.yaml           # Main GitHub Actions pipeline
├── pom.xml                  # Maven project file
└── README.md                # This file

🐳 CD Pipeline (Push to main)
1. Build & Push Docker Image to AWS ECR
Creates ECR repo if missing

Builds multi‑stage Docker image

Tags with:

latest

Short SHA (e.g., abc1234)

Pushes both tags to ECR

2. Update Helm GitOps Repository
The workflow:

Clones your separate Helm repo using a secure PAT

Updates:
Code
app.image: <ECR repo>
app.tag:   <image tag>

Commits & pushes changes to main

ArgoCD (external) detects the change and deploys automatically

Required GitHub Secrets & Variables
Secrets
Name	Purpose
AWS_ACCESS_KEY_ID	AWS auth
AWS_SECRET_ACCESS_KEY	AWS auth
GITOPS_PAT	PAT for Helm repo updates
HELM_REPO_USER	GitHub username for Helm repo
SONAR_TOKEN	SonarQube authentication
SONAR_HOST_URL	SonarQube server URL


Variables
Name	Purpose
AWS_REGION	AWS region (e.g., us-east-1)
ECR_REPOSITORY	ECR repo name
HELM_REPO_NAME	Name of GitOps Helm repo


📦 Deployment Flow Diagram





