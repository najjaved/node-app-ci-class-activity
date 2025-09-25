# Class Activity: CI/CD with GitHub Actions & Azure VM Deployment

In this exercise, you will create **two GitHub Actions pipelines** for a Node.js application.  
The first pipeline will handle **Build, Test & Docker image creation**, and the second pipeline will handle **Deployment to an Azure VM**.

---

## Task Description

### Part 1 – CI Pipeline
Fork the [Node.js App](https://github.com/saurabhd2106/sample-node-app-ci-lab-ih) and create a GitHub Actions workflow file (`.github/workflows/ci-pipeline.yml`) inside. The workflow:

1. **Triggers** on `push` to the `main` branch. 
2. **Installs Node.js dependencies** and runs `npm test`.
    - Hint: `npm install` → installs packages from package.json file of the node.js app (check also `npm ci` for clean install) & `npm run build` if required.
3. **Builds a Docker image** of the Node.js app.
4. **Pushes the Docker image** to a container registry (DockerHub or Azure Container Registry).
   - Use **GitHub Secrets** for credentials.
5. **Uploads build artifacts** for later inspection.

---

### Part 2 – CD Pipeline
Create a GitHub Actions workflow file (`.github/workflows/cd-pipeline.yml`) that:

1. **Triggers automatically** when the CI pipeline finishes successfully.
2. **Connects via SSH** to an Azure Virtual Machine.
   - Use VM credentials stored in **GitHub Secrets**.
3. **Pulls the Docker image** built in the CI pipeline.
4. **Runs the container** on the VM (exposed on port 80).
    - Hint: On VM in Azure, add inbound port rule for HTTP (80). Your Node.js app (inside Docker) usually serves traffic on port 3000. We map this to port 80 on the VM so that users can access it via a browser without typing :3000

---

## Secrets Required

Make sure the following secrets are stored in GitHub:

- `DOCKER_USERNAME`  
- `DOCKER_PASSWORD`
- `AZURE_CREDENTIALS`
   (for example: create one secret: AZURE_CREDENTIALS
value: 
{
  "clientId": "your_cleind_id_value",
  "clientSecret": "your_app_secret",
  "subscriptionId": "your_azure_subscription_id",
  "tenantId": "id_valeu"
})
- `AZURE_VM_NAME`
- `AZURE_VM_RESOURCE_GROUP`  
- `AZURE_VM_IP`  
- `AZURE_VM_USER` 
- `AZURE_VM_SSH_KEY` (private key for SSH authentication if using Github Action appleboy)
    - Hint: Copy your private key content (~/.ssh/vm_key.pem) and paste it as the secret value.

---

**Deliverables:**
- `ci-pipeline.yml` (build, test, docker push)  
- `cd-pipeline.yml` (deployment to Azure VM)  

Now after automated deployment, you can access your app in your browser via http://<your_VM_public_IP>:80