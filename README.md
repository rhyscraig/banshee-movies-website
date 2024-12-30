# Banshee Movies Website
The Banshee Movies Website is a dynamic platform that showcases a curated collection of films, providing users with detailed information, trailers, and reviews. Built using modern web technologies, the site offers an engaging and user-friendly experience.

## Features
- Comprehensive Movie Database: Access an extensive list of movies across various genres, complete with synopses, cast details, and release dates.
- Interactive User Interface: Navigate through the site effortlessly with an intuitive design and responsive layout.
- Search and Filter Options: Easily find movies by title, genre, or release year using the advanced search functionality.
- User Reviews and Ratings: Engage with the community by reading and submitting reviews, and view aggregated ratings for each film.

## Deployment Overview
The Banshee Movies Website is deployed to Amazon Web Services (AWS) using a Continuous Integration and Continuous Deployment (CI/CD) pipeline powered by GitHub Actions. This setup ensures that the website is automatically updated with the latest changes from the GitHub repository.

### Refactored Code [Dec 24]
Banshee has been split into movies and tv shows with their own repos in GitHub. 
Banshee movies - banshee-movies-website
Banshee tv - banshee-tv-website
This is so GitHub actions can update the HTML files inside each bucket.
New Banshee buckets are managed in aws-prod-account

## Deployment Process
Code Commit: Developers push updates to the master branch of the GitHub repository.

GitHub Actions Workflow: A GitHub Actions workflow is triggered upon each push to the master branch.

Build and Test: The workflow installs necessary dependencies, runs tests to ensure code quality, and builds the production-ready assets.

AWS Deployment: The workflow utilizes the aws-actions/configure-aws-credentials action to authenticate using OpenID Connect (OIDC) and deploys the built assets to an Amazon S3 bucket configured for static website hosting.

This approach leverages AWS's support for OIDC, allowing GitHub Actions to authenticate without the need for long-lived AWS credentials. 
GITHUB DOCS

Website Update: Once the deployment is successful, the website reflects the latest changes, providing users with an up-to-date experience.

## Setup Instructions
To set up the deployment pipeline, follow these steps:

### Configure AWS:

- Create an S3 Bucket: Set up an S3 bucket to host the static website.
- Enable Static Website Hosting: Configure the S3 bucket for static website hosting.
- Set Up an OIDC Identity Provider: Create an OIDC provider in AWS to allow GitHub Actions to authenticate.
- Create an IAM Role: Establish an IAM role with a trust policy that allows GitHub Actions to assume the role using OIDC.

### Configure GitHub Actions:

Create a Workflow File: Add a .github/workflows/deploy.yml file to your repository with the following content:

yaml
Copy code
name: Deploy to S3

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: arn:aws:iam::YOUR_ACCOUNT_ID:role/YOUR_ROLE_NAME
          aws-region: YOUR_AWS_REGION

      - name: Build and deploy
        run: |
          # Build commands here
          aws s3 cp . s3://YOUR_BUCKET_NAME/ --recursive
Replace YOUR_ACCOUNT_ID, YOUR_ROLE_NAME, YOUR_AWS_REGION, and YOUR_BUCKET_NAME with your actual AWS account ID, IAM role name, AWS region, and S3 bucket name, respectively.

Push Changes: Push changes to the master branch to trigger the deployment workflow.

For detailed instructions, refer to GitHub's official documentation on configuring OpenID Connect in Amazon Web Services. 
GITHUB DOCS

Contributing
Contributions are welcome! To contribute:

Fork the Repository: Create a personal fork of the repository.
Clone Your Fork: Clone the fork to your local machine.
Create a Branch: Create a new branch for your feature or fix.
Make Changes: Implement your changes and commit them with descriptive messages.
Push Changes: Push your changes to your fork on GitHub.
Create a Pull Request: Open a pull request to the master branch of the original repository.
Please ensure that your code adheres to the project's coding standards and includes appropriate tests.