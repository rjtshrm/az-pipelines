# README: Shared Azure Pipeline Templates

## Overview
This repository demonstrates a structured approach to manage CI/CD pipelines for Go microservices using Azure DevOps. The pipeline templates and deployment configurations are designed for reusability, scalability, and ease of integration across multiple repositories and environments.

---

## Directory Structure

### **ci**
- **Purpose:**
  - Contains CI pipeline templates for specific tasks in Azure DevOps.
- **Files:**
  - `docker-build.yaml`: Handles Docker image building.
  - `snyk-code-security-analysis.yaml`: Manages security scanning using Snyk.
  - `sonar-code-analysis.yaml`: Includes tasks for SonarQube code quality analysis.

### **testing**
- **Purpose:**
  - Unit test-related files for validating application functionality.

### **deployment**
- **Purpose:**
  - Contains deployment-related configuration files for multi-environment setups.
- **Files:**
  - `deploy.yaml`: Parameterized template for deploying to different environments (e.g., staging and production).


---

## Using Shared Templates

### **Step 1: Reference the Shared Template Repository**
In your Azure DevOps pipeline, include the shared templates repository as a resource:

```yaml
resources:
  repositories:
  - repository: templates
    type: git
    name: pipeline-templates  # Replace with the name of your shared repository
    ref: refs/tags/v1.0.0  # Optional: Specify a version tag
```

### **Step 2: Use the Templates in Your Pipeline**
You can now reference the templates in your pipeline YAML:

#### Example: Build and Analyze
```yaml
- stage: BuildAndAnalyze
  jobs:
  - template: azure-pipelines/ci/docker-build.yaml@templates
    parameters:
      repository: my-service
      tag: $(Build.BuildId)
```

#### Example: Multi-Environment Deployment
```yaml
- stage: Deploy
  jobs:
  - template: deployment/deploy.yaml@templates
    parameters:
      environment: staging
      namespace: staging-namespace
      releaseName: staging-microservice
```

### **Step 3: Customize Parameters**
Each template is parameterized for flexibility. Pass environment-specific values (e.g., `namespace`, `releaseName`) to adapt the pipeline to your needs.

---

## Benefits of This Approach
- **Centralized Templates:** Simplifies CI/CD management by maintaining a single source of truth.
- **Reusability:** Enables easy integration across multiple microservices.
- **Scalability:** Quickly extend pipelines to new environments or services.
- **Consistency:** Ensures uniform CI/CD practices across repositories.

---