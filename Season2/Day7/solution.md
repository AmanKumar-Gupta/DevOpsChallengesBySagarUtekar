# Solution: Setting Up a Self-Hosted GitHub Runner

## Step 1: Create a Virtual Machine

- Launch an Ubuntu 24.04 virtual machine (e.g., on Azure or AWS EC2).
- Allow inbound traffic on ports **22 (SSH)** and **80 (HTTP)**.

![Runner setup](Runner.png)

## Step 2: Configure GitHub Self-Hosted Runner

- Navigate to your **GitHub repository** → **Settings** → **Actions** → **Runners**.
- Click **"New self-hosted runner"**, select **Linux**, and follow the instructions to set it up.
- SSH into the VM and install the runner using the provided commands.
- Start the runner: `./run.sh`

![GitHub Runner](Runner.png)

## Step 3: Install Docker on the VM

Before setting up the workflow, install Docker on your VM:

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
```

Log out and log back in for the changes to take effect.

## Step 4: Set Up Repository & Secrets

- Fork the repository from [Sagar2366/season2-snake_game](https://github.com/Sagar2366/season2-snake_game)
- Add secrets in GitHub under **Settings → Secrets and variables → Actions**:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_PASSWORD`
  - `GMAIL_USERNAME`
  - `GMAIL_PASSWORD`
  - `MAIL_TO`

![secrets](secrets.png)

## Step 5: Create GitHub Actions Workflow

Create a workflow file that does the following:

1. **Builds the application** when code is pushed to the main branch
2. **Builds and pushes a Docker image** to Docker Hub

![Docker Hub](dockerhub.png)

3. **Deploys the application on the self-hosted runner** by pulling and running the Docker container
4. **Validates that the application is running correctly** by checking if it responds on port 80

![Validation](validate.png)

5. **Sends an email notification** about the deployment status

![Email notification](email.png)

6. **Maps the container port to port 80** so the application is accessible via HTTP

## Step 6: Test the Deployment

- The workflow will automatically build, push, and deploy your application.
- Confirm the deployment by accessing `http://<vm-public-ip>` in a browser.

![Browser access](browser.png)

- If the workflow succeeds:

![Workflow success](success.png)

## Key Points to Remember

- Make sure your Dockerfile exposes the correct port.
- The workflow stops any existing container before deploying the new one.
- Email notifications will tell you if the deployment succeeded or failed.
- You can check container logs with `docker logs <container-name>` if something goes wrong.

That's it! Your application will now automatically deploy whenever you push changes to your repository.