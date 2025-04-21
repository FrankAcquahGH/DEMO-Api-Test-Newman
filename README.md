# Api-Test-Newman

This repository contains a setup for automating API tests using Postman, Newman, and GitHub Actions. The Postman collection is executed automatically on pushes or pull requests to the main branch, or manually via a workflow dispatch.
Overview

Postman Collection: The API tests are defined in a Postman collection (Regression_testing.postman_collection.json).
Newman: A command-line tool to run Postman collections.
GitHub Actions: A CI/CD workflow that runs the collection, generates an HTML report, and uploads it as an artifact.

Prerequisites

A GitHub account and repository.
Postman installed to create/export collections.
Node.js installed locally (optional, for testing Newman locally).
A Postman collection file (e.g., Regression_testing.postman_collection.json).

Setup Instructions
1. Export Your Postman Collection

Open Postman.
Export your collection:
Click the three dots (…) next to the collection.
Select Export, choose Collection v2.1, and save as Regression_testing.postman_collection.json.


(Optional) Export any environment file as my-environment.postman_environment.json.

2. Upload the Collection to the Repository

Upload Regression_testing.postman_collection.json to the root of this repository using the GitHub GUI or CLI (see previous conversation for detailed steps).
If you have an environment file, upload it as well.

3. Verify the GitHub Actions Workflow

The workflow file is located at .github/workflows/Newman.yml.
It triggers on:
Pushes or pull requests to the main branch.
Manual trigger via workflow_dispatch.


The workflow:
Checks out the repository.
Sets up Node.js and installs Newman.
Runs the Postman collection using Newman.
Generates an HTML report using newman-reporter-htmlextra.
Uploads the report as an artifact.



4. Run the Workflow

Push a change to the main branch to trigger the workflow.
Alternatively, go to the Actions tab, select the "Run Postman Collection" workflow, and click Run workflow.
After the workflow completes, download the Postman-Test-Reports artifact to view the HTML report.

Workflow Details

File: .github/workflows/Newman.yml
Steps:
Checkout the repository.
Set up Node.js (version 20).
Install Newman and newman-reporter-htmlextra.
Create a directory for test results.
Run the Postman collection with Newman.
Upload the test report as an artifact.


Report: The HTML report is generated at testResults/htmReport.html.

Troubleshooting

File Not Found: Ensure the collection file name in Newman.yml matches the file in the repository (e.g., Regression_testing.postman_collection.json).
Spaces in File Names: Avoid spaces in file names to prevent issues. Use underscores instead (e.g., Regression_testing.postman_collection.json).
Environment Variables: If your collection requires an environment file, upload it to the repository and update the newman run command in Newman.yml to include -e my-environment.postman_environment.json.
Logs: Check the workflow logs in the Actions tab for detailed error messages.

Additional Notes

To run the collection locally with Newman:npm install -g newman newman-reporter-htmlextra
newman run Regression_testing.postman_collection.json -r htmlextra,cli --reporter-htmlextra-export testResults/htmReport.html


To schedule the workflow (e.g., daily at 2 AM UTC), add a schedule event in Newman.yml:on:
  schedule:
    - cron: '0 2 * * *'



