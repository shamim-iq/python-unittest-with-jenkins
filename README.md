# Project Title
Python unit testing with jenkins pipeline

## Introduction

This project includes a Python unit test to verify the successful loading of the "atg.world" website. The unit test is integrated into Jenkins, hosted on a custom subdomain (e.g., jenkins.domainname.com). The Jenkins pipeline is triggered automatically through a webhook when changes are pushed to the associated GitHub repository.

## Project Structure

- **test_website_loader.py**: Python unit test script to check the successful loading of the "atg.world" website.
- **Jenkinsfile**: Jenkins pipeline configuration to automate the installation of dependencies, run the unit test, and handle post-test actions.
- **requirements.txt**: List of Python dependencies, including the required version of the `requests` library.
- **nginx.conf**: Nginx configuration file to map the Jenkins subdomain to the Jenkins server.

## Jenkins Setup

### 1. Jenkins Subdomain Configuration

- Set up a custom subdomain (e.g., jenkins.domainname.com) for Jenkins.
- Configure Nginx to map this subdomain to your Jenkins server.

    Example Nginx Configuration (`/etc/nginx/nginx.conf`):
    ```nginx
    server {
        listen 80;
        server_name jenkins.domainname.com;

        location / {
            proxy_pass http://<your_ip>:8080;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```

    Test Nginx:
    ```bash
    sudo nginx -t
    ```

    Reload Nginx:
    ```bash
    sudo nginx -s reload
    ```

### 2. Webhook Integration

- Set up a webhook in your GitHub repository to trigger the Jenkins pipeline on each push.
  
    - In your GitHub repository, go to `Settings` > `Webhooks` > `Add webhook`.
    - Set the Payload URL to `http://jenkins.domainname.com/github-webhook/`.
    - Choose the events that trigger the webhook (e.g., `Just the push event`).
    - Save the webhook.

## Instructions

### Running the Unit Test Locally

1. Clone the repository:

    ```bash
    git clone https://github.com/shamim-iq/python-unittest-with-jenkins.git
    cd python-unittest-with-jenkins
    ```

2. Install dependencies:

    ```bash
    pip install -r requirements.txt
    ```

3. Run the unit test:

    ```bash
    python3 -m unittest test_website_loader.py
    ```

### Jenkins Integration

1. Configure Jenkins:
    - Install Jenkins on your server.
    - Access Jenkins at `http://jenkins.domainname.com` and follow the setup wizard.
    - Install necessary plugins (e.g., GitHub plugin).
    - Set up GitHub credentials in Jenkins.(Needed only in case of private repository)

2. Update Jenkins Pipeline:
    - Open the Jenkinsfile and replace the repository URL with your repository URL.
    - Adjust any other pipeline configurations as needed.

3. Run the Jenkins Pipeline:
    - Push changes to your GitHub repository to trigger the Jenkins pipeline.
    - Jenkins will automatically install dependencies, run the unit test, and mark the build as unstable if the test fails.

## Notes

- Ensure Jenkins has the necessary permissions to access the GitHub repository.
- Customize Nginx configurations and Jenkins pipeline as needed for your specific setup.
- Monitor Jenkins build status for automatic feedback on website loading test results.
- Replace "jenkins.domainname.com" and use your custom domain.

