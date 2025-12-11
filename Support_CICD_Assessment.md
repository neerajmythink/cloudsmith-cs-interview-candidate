# Assessment Preface

After cloning the original repository to my GitHub namespace, the following changes were made to enable OIDC authentication and facilitate successful build, release, and promotion of the package.

## Changes in repository files

### 1. pyproject.toml
- Added `name` and `version` fields under the `[project]` section

### 2. .github/workflows/build_package.yml
- Added build command in the **Build Python Package** step to build the package using the build module

### 3. .github/workflows/release_package.yml
- Added `id-token: write` permission under the **permissions** section

### 4. .github/workflows/release_package.yml
- Added **Install Cloudsmith CLI** step before **Push package to Cloudsmith** using the cloudsmith-io/cloudsmith-cli-action@v1.0.3 action with necessary OIDC parameters

### 5. .github/workflows/promote_package.yml
- Replaced **workflow_dispatch** trigger with **repository_dispatch** trigger for **promote-package** event

### 6. .github/workflows/promote_package.yml
- Swapped **staging** and **production** repository names in environment variables
- Added `TAG_NAME` environment variable to specify the tag during promotion
- Updated `PACKAGE_QUERY` to filter packages by partial package name match
- Created a loop to iterate over matching packages and promote each by adding the specified tag and moving to production


## Cloudsmith Account Setup and Configuration

- Created `staging` and `production` repositories under the **interview-neeraj-srivastava** namespace
- Created a service account to allow `OIDC authentication` from GitHub Actions to Cloudsmith
- Configured provider settings under **settings** > **Organization** > **OpenID Connect** for GitHub Actions OIDC with the created service account
- Created a webhook in **staging** repository to trigger the promotion workflow on `promote-package` event in GitHub Actions whenever a package is pushed to the staging repository.

## GitHub Repository Configuration

- Under **Secrets and variables** > **Actions** > **Variables**, added the following environment variables to enable OIDC authentication with Cloudsmith:
- `CLOUDSMITH_NAMESPACE`: interview-neeraj-srivastava
- `CLOUDSMITH_SERVICE_SLUG`: github-actions-sa-3qx9