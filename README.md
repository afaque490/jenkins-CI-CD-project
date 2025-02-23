# CI/CD Pipeline with Jenkins, GitHub, and Nginx

## Project Overview

This project demonstrates a CI/CD pipeline for automating the deployment of a web application using Jenkins, GitHub, and Nginx. The pipeline is designed to fetch code from a GitHub repository, build and test the application, and then deploy it to a web server.

## Technologies Used

- **GitHub** Version control system to manage source code.
- **Jenkins**: CI/CD tool to automate build and deployment.
- **Nginx**: Web server for hosting the application.
- **Ngrok**: Used for exposing local servers to the internet.
- **Linux (Ubuntu)**: Operating system for running Jenkins and Nginx.

## Workflow

1. **Developer Pushes Code**: A developer makes changes and pushes code to the GitHub repository.
2. **GitHub Webhook Triggers Jenkins**: A webhook notifies Jenkins about the new changes.
3. **Jenkins Builds the Project**:
   - Fetches the latest code from GitHub.
   - Runs tests and builds the application.
   - Deploys the updated code to the server.
4. **Nginx Serves the Updated Application**: Nginx ensures that the latest version of the application is accessible to users.

## Setup Instructions

### 1. Install Jenkins

```sh
sudo apt update && sudo apt install -y openjdk-11-jdk
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
echo "deb http://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt update && sudo apt install -y jenkins
sudo systemctl enable --now jenkins
```

### 2. Install and Configure Nginx

```sh
sudo apt install -y nginx
sudo systemctl enable --now nginx
```

Configure Nginx to serve your web application by editing `/etc/nginx/sites-available/default`.

### 3. Install and Configure Git

```sh
sudo apt install -y git
```

Set up user details:

```sh
git config --global user.name "your_username"
git config --global user.email "your_email@example.com"
```

### 4. Install and Configure Ngrok

```sh
wget -qO- https://ngrok.com/download | sudo tar xvz -C /usr/local/bin
ngrok config add-authtoken <your-ngrok-token>
ngrok http 80
```

### 5. Set Up GitHub Webhook

- Go to your GitHub repository.
- Navigate to **Settings > Webhooks**.
- Add a webhook with the payload URL as `http://<your-ngrok-url>/github-webhook/`.
- Set content type to `application/json` and select **Just the push event**.

### 6. Configure Jenkins Pipeline

1. Install required plugins (`Git Plugin`, `GitHub Plugin`).
2. Create a new **Freestyle project** in Jenkins.
3. Set **GitHub project URL** and configure **Source Code Management** to pull from the repository.
4. Under **Build Triggers**, select **GitHub hook trigger for GITScm polling**.
5. Add a **Build Step** to execute deployment commands.

## Project Structure

```
jenkins-CI-CD-project/
│── .github/                 # GitHub Actions workflow (if needed in the future
│── nginx/                   # Nginx configuration files
│── src/                     # Main source code directory
│   ├── css/                 # Stylesheets
│   ├── fonts/               # Font files
│   ├── images/              # Images used in the project
│   ├── js/                  # JavaScript files
│   ├── videos/              # Video files if required
│   ├── views/               # HTML files
│   │   ├── index.html       # Main homepage
│   │   ├── contact.html     # Contact page
│   │   ├── login.html       # Login page
│   │   ├── register.html    # Registration page
│   │   ├── password-reset.html  # Password reset page
│   │   ├── 404.html         # Error page
│── docs/                    # Documentation, including README and project screenshots
│   ├── screenshots/         # Project screenshots
│   ├── README.md            # Project documentation
│── scripts/                 # Automation scripts (if needed)
│── .gitignore               # Git ignore 

```

## Lessons Learned

- Setting up a fully automated CI/CD pipeline using Jenkins and GitHub.
- Configuring Nginx as a web server for deployment.
- Using Ngrok for webhook testing and exposing local services.
- Automating code updates through GitHub webhooks.
- Understanding the integration of Jenkins with GitHub for continuous deployment.

