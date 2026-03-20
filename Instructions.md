# Docker + Jenkins CI/CD Setup Instructions

## Local Development

### Build Docker Image
```bash
docker build -t docker-jenkins-app .
```

### Run Container
```bash
docker run -p 3000:3000 docker-jenkins-app
```

### Test Application
Open http://localhost:3000 in your browser. You should see "Hello from Docker + Jenkins"

## Jenkins Pipeline Setup

### 1. Configure Jenkins
- Install Docker Pipeline plugin
- Install Docker plugin
- Ensure Docker is installed on Jenkins server

### 2. Add Docker Hub Credentials
1. Go to Jenkins Dashboard → Manage Jenkins → Manage Credentials
2. Click (global) → Add Credentials
3. Select "Username with password"
4. **Username**: Your Docker Hub username
5. **Password**: Your Docker Hub password or access token
6. **ID**: `docker-hub-credentials`
7. Click "OK"

### 3. Update Jenkinsfile
Edit the `Jenkinsfile` and replace:
- `your-dockerhub-username` with your actual Docker Hub username
- `https://github.com/your-username/docker-jenkins-app.git` with your GitHub repository URL

### 4. Create Jenkins Job
1. Go to Jenkins Dashboard → New Item
2. Enter job name (e.g., "docker-jenkins-app")
3. Select "Pipeline" → Click "OK"
4. In Pipeline section:
   - Definition: "Pipeline script from SCM"
   - SCM: Git
   - Repository URL: Your GitHub repository URL
   - Credentials: Add your GitHub credentials if needed
   - Script Path: `Jenkinsfile`
5. Click "Save"

### 5. Trigger Build
- Click "Build Now" to manually trigger
- Or configure webhooks for automatic builds on Git push

## Environment Variables in Jenkinsfile

Update these variables in the Jenkinsfile:
- `DOCKER_IMAGE`: Change `your-dockerhub-username` to your Docker Hub username
- Ensure credentials ID matches what you configured (`docker-hub-credentials`)

## Troubleshooting

### Docker Build Issues
- Ensure Docker daemon is running
- Check Dockerfile syntax
- Verify all files are in the correct directory

### Jenkins Pipeline Issues
- Check Jenkins logs for specific error messages
- Verify Docker Hub credentials are correct
- Ensure Jenkins has Docker permissions
- Check if Docker plugins are properly installed

### Permission Issues
```bash
# Add jenkins user to docker group (Linux)
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

## Production Deployment

### Pull and Run on Production Server
```bash
# Pull the image from Docker Hub
docker pull your-dockerhub-username/docker-jenkins-app:latest

# Run the container
docker run -d -p 3000:3000 --name app your-dockerhub-username/docker-jenkins-app:latest
```
