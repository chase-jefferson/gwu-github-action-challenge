# CI/CD Pipeline with GitHub Actions and Render Deployment

## Overview
This project integrates a CI/CD pipeline using GitHub Actions to ensure code quality and automate deployments. The pipeline consists of two workflows:

1. **Continuous Integration (CI):** Runs Cypress component tests on Pull Requests to the `develop` branch.
2. **Continuous Deployment (CD):** Deploys the application to Render when code is merged from `develop` to `main`.

## User Story

```md
AS A developer looking to integrate a pipeline in a codebase for continuous integration and deployment,
I WANT to create a GitHub Action that will follow best practices by running test cases when a Pull Request is made to the develop branch
SO THAT I can ensure that all code integrations are clean and pass the proper requirements.

AS A developer looking to deploy the application automatically to Render when code is merged from develop to main,
I WANT to create a GitHub Action that will run when the code is merged to main and automatically deploys to Render
SO THAT the application is constantly updated when major releases are made to the main branch.
```

## Acceptance Criteria

```md
GIVEN a fullstack application for a web developer,
WHEN I upload new features to the application
THEN I should be making Pull Requests to a develop branch first
WHEN I create a Pull Request to a develop branch
THEN I should be executing a GitHub Action that checks the Cypress component tests
WHEN I see that the tests pass on GitHub Action
THEN I should see those test results on GitHub Action and merge the code
WHEN I push the code from the develop branch to the main branch
THEN I should see that another GitHub Action triggers and should automatically deploy to Render
```

## Setup Instructions

### 1. Upload Code to GitHub
- Push your project files to a new GitHub repository.
- Create a `develop` branch: 
  ```sh
  git checkout -b develop
  git push origin develop
  ```

### 2. Deploy to Render
- Deploy the application via [Render](https://render.com/).
- Configure MongoDB Atlas if needed.
- Disable **Auto-Deploy** in Render Settings.
- Copy the **Deploy Hook URL** from Render.

### 3. Create GitHub Actions Workflows
#### **Workflow 1: Cypress Tests (`.github/workflows/test.yml`)
```yaml
name: Cypress Tests

on:
  pull_request:
    branches:
      - develop

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install Dependencies
        run: npm install

      - name: Run Cypress Tests
        run: npm run test
```

#### **Workflow 2: Deploy to Render (`.github/workflows/deploy.yml`)
```yaml
name: Deploy to Render

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Render Deploy Hook
        run: |
          curl -X POST "$RENDER_DEPLOY_HOOK"
        env:
          RENDER_DEPLOY_HOOK: ${{ secrets.RENDER_DEPLOY_HOOK }}
```

## Environment Variables
- **GitHub Secrets:**
  - `RENDER_DEPLOY_HOOK`: Set this in your GitHub repository settings under **Secrets and variables → Actions**.

## Branching Strategy
- **Feature Branches** → `develop` → `main`
- Pull Requests should be made to `develop` first.
- Merges from `develop` to `main` trigger deployment.

## Summary
This setup ensures that:
- Cypress tests run on every PR to `develop`.
- Code merging from `develop` to `main` automatically deploys to Render.

Happy coding! 🚀

