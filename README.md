## 🔄 CI/CD Pipeline (GitOps)

The CI/CD pipeline is fully automated and triggered on pushes to the `main` branch. 

**Workflow Steps (`deploy.yml`):**
1. **CI Phase - Build & Push**:
   - Checks out the code.
   - Logs into DockerHub using secrets.
   - Builds the Docker image tagged with the unique GitHub commit SHA (`${{ github.sha }}`).
   - Pushes the image to DockerHub.
   
2. **CD Phase - Update Manifest**:
   - Uses `sed` to dynamically update `k8s/deployment.yaml` with the new image tag.
   - Commits the updated manifest back to the `main` branch with a `[skip ci]` flag to prevent infinite loops.

3. **GitOps Phase - ArgoCD Sync**:
   - ArgoCD detects the new commit in the `k8s/` directory.
   - It automatically pulls the changes and updates the Kubernetes cluster with the new Docker image.

### 🔑 Required GitHub Secrets
To make the deployment pipeline work, you must add the following secrets in your GitHub repository settings (**Settings > Secrets and variables > Actions**):

| Secret Name | Description |
| :--- | :--- |
| `DOCKERHUB_USERNAME` | Your DockerHub account username. |
| `DOCKERHUB_TOKEN` | A Personal Access Token (PAT) from DockerHub. |

*(Note: The EC2 secrets previously required are no longer needed since we migrated to Kubernetes/ArgoCD).*

## 📝 License
This project is open-source and available for educational purposes.

*happy_to_be_devops_guy*
