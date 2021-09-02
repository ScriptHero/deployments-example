# GitHub Deployments using Actions

A project to build and demonstrate the use of GitHub Deployments from within GitHub Actions.

Deployments are controlled by the creation of a GitHub deployment, which causes another workflow job to run.

The deployment object is created by the GitHub Actions workflow.
GitHub specifically disallows the result of an Actions workflow run auth token from starting another workflow run.
To work around this, the API calls that create the Deployment must use the credentials of a GitHub App (which we'll call a "bot").

Create a GitHub App and create a private key for it.
Add the app ID and the private key as secrets to the repository.
And install the app on the repository.

## Test environments

These are deployed from a pull request, where each pull request gets a unique deployment environment.

- "Create" job: When a pull request is opened with the `deploy to spicy-proton` label, or it is added later
  - creates a Deployment for the current commit
- "Destroy" job: When a pull request is closed or the `deploy to spicy-proton` label is removed
  - any previous run of this workflow is canceled so that no deployment process completes
  - the Deployment is set to "inactive" state
- "Deploy" job: When a Deployment is created
  - deploys!
  - updates the status of the deployment to indicate success or failure
