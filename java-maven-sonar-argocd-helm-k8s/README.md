<img width="300" height="140" alt="jenkins_cicd_whiteboard_diagram" src="https://github.com/user-attachments/assets/d20876a3-dd68-46b1-803d-09dc49c87a67" /># End-to-End CI/CD for Spring Boot with Jenkins, SonarQube, Docker, Kubernetes, and Argo CD

This project demonstrates a complete DevOps CI/CD workflow for a Java Spring Boot application.

The Jenkins pipeline builds the application with Maven, runs SonarQube analysis, builds and pushes a Docker image to Docker Hub, updates the Kubernetes deployment manifest with the new image tag, and lets Argo CD deploy the updated application to Kubernetes.

## Project Structure

```text
java-maven-sonar-argocd-helm-k8s/
+-- Argo CD/
|   +-- argocd-basic.yaml
+-- spring-boot-app/
|   +-- Dockerfile
|   +-- JenkinsAgent.Dockerfile
|   +-- JenkinsFile
|   +-- pom.xml
|   +-- src/
+-- spring-boot-app-manifests/
    +-- deployment.yml
    +-- service.yml
```

## Tools Used

- **Jenkins**: CI/CD automation
- **Maven**: Java build and test tool
- **SonarQube**: Static code analysis
- **Docker**: Container image build and push
- **Docker Hub**: Container registry
- **Kubernetes**: Application runtime platform
- **Argo CD**: GitOps deployment controller

## Current Images

Application image:

```text
naveenreddy9/spring-boot-app:<BUILD_NUMBER>
```

Jenkins agent image:

```text
naveenreddy9/maven-docker-java17-agent:v1
```

The custom Jenkins agent image includes:

- Java 17
- Maven 3.9.9
- Docker CLI 29.5.3

This avoids repeated installation of Java and Docker CLI during every pipeline run.

## Jenkins Pipeline Flow

The pipeline is defined in:

```text
spring-boot-app/JenkinsFile
```

Pipeline stages:

1. **Checkout**
   - Clones the GitHub repository from the `main` branch.

2. **Build and Test**
   - Runs Maven package:

   ```bash
   mvn clean package
   ```

3. **Static Code Analysis**
   - Runs SonarQube analysis with the pinned Maven Sonar plugin:

   ```text
   org.sonarsource.scanner.maven:sonar-maven-plugin:4.0.0.4121
   ```

4. **Build and Push Docker Image**
   - Builds the Spring Boot Docker image.
   - Tags it with the Jenkins build number.
   - Pushes it to Docker Hub.

5. **Update Deployment File**
   - Updates `spring-boot-app-manifests/deployment.yml`.
   - Replaces the image tag with the latest Jenkins build number.
   - Commits and pushes the updated manifest back to GitHub.

6. **Post Actions**
   - Fixes workspace ownership after the Docker-based Jenkins agent exits.

## Jenkins Credentials

Create these credentials in Jenkins under:

```text
Manage Jenkins -> Credentials -> System -> Global credentials
```

### SonarQube Token

```text
ID: sonarqube
Kind: Secret text
Secret: SonarQube user token
```

Used by:

```groovy
withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')])
```

### Docker Hub Credentials

```text
ID: docker-cred
Kind: Username with password
Username: naveenreddy9
Password: Docker Hub password or access token
```

Used by:

```groovy
docker.withRegistry('https://index.docker.io/v1/', "docker-cred")
```

### GitHub Token

```text
ID: github
Kind: Secret text
Secret: GitHub personal access token
```

The token must have permission to push to:

```text
VenkataNaveenReddyYaparla/Jenkins_E2E_CICD
```

For a classic GitHub token, enable the `repo` permission.

## Custom Jenkins Agent Image

The Jenkins pipeline uses this custom agent:

```text
naveenreddy9/maven-docker-java17-agent:v1
```

Its Dockerfile is:

```text
spring-boot-app/JenkinsAgent.Dockerfile
```

Build the agent image:

```bash
cd spring-boot-app
docker build -f JenkinsAgent.Dockerfile -t naveenreddy9/maven-docker-java17-agent:v1 .
```

Push the agent image:

```bash
docker login
docker push naveenreddy9/maven-docker-java17-agent:v1
```

Verify the image:

```bash
docker run --rm naveenreddy9/maven-docker-java17-agent:v1 sh -lc "java -version && mvn -v && docker --version"
```

## Run the Application Locally

### Using Maven

```bash
cd spring-boot-app
mvn clean package
java -jar target/spring-boot-web.jar
```

Open:

```text
http://localhost:8080
```

### Using Docker

```bash
cd spring-boot-app
docker build -t spring-boot-app .
docker run --rm -p 8080:8080 spring-boot-app
```

Open:

```text
http://localhost:8080
```

## Kubernetes Deployment

Kubernetes manifests are stored in:

```text
spring-boot-app-manifests/
```

Deployment:

```text
spring-boot-app-manifests/deployment.yml
```

Service:

```text
spring-boot-app-manifests/service.yml
```

The deployment uses the Docker image:

```text
naveenreddy9/spring-boot-app:<BUILD_NUMBER>
```

The service is a `NodePort` service that exposes the app on port `80` and forwards traffic to container port `8080`.

Apply manually:

```bash
kubectl apply -f spring-boot-app-manifests/deployment.yml
kubectl apply -f spring-boot-app-manifests/service.yml
```

Check resources:

```bash
kubectl get pods
kubectl get svc
```

For local testing:

```bash
kubectl port-forward svc/spring-boot-app-service 8080:80
```

Open:

```text
http://localhost:8080
```

## Argo CD Setup

Argo CD should track the Git repository that contains:

```text
spring-boot-app-manifests/
```

When Jenkins pushes a new image tag to `deployment.yml`, Argo CD detects the Git change and syncs the new deployment to Kubernetes.

Example Argo CD application manifest:

```text
Argo CD/argocd-basic.yaml
```

Apply it:

```bash
kubectl apply -f "Argo CD/argocd-basic.yaml"
```

Check Argo CD application status:

```bash
kubectl get applications -n argocd
```

## SonarQube Notes

The SonarQube scanner must run with Java 17.

The custom Jenkins agent image already includes Java 17. This fixes errors like:

```text
UnsupportedClassVersionError
class file version 61.0
this version of the Java Runtime only recognizes class file versions up to 55.0
```

Meaning:

```text
class file version 61 = Java 17
class file version 55 = Java 11
```

## Common Issues and Fixes

### `ERROR: Could not find credentials entry with ID 'github'`

Create a Jenkins credential:

```text
ID: github
Kind: Secret text
```

Use a GitHub token with push permission.

### `ERROR: docker-cred`

Create Docker Hub credentials in Jenkins:

```text
ID: docker-cred
Kind: Username with password
```

Make sure it is under **System -> Global credentials**.

### `client version 1.41 is too old`

The Docker CLI inside the Jenkins agent is too old for the Docker daemon.

Fix: use the custom Jenkins agent image:

```text
naveenreddy9/maven-docker-java17-agent:v1
```

### `fatal: not in a git directory`

The workspace is not a real Git checkout.

Fix: make sure the Jenkins `Checkout` stage performs a real Git checkout:

```groovy
git branch: 'main', url: 'https://github.com/VenkataNaveenReddyYaparla/Jenkins_E2E_CICD.git'
```

### `detected dubious ownership`

Add the Jenkins workspace as a safe Git directory:

```bash
git config --global --add safe.directory "$WORKSPACE"
```

This is already handled in the current Jenkinsfile.

## Useful Commands

Check Docker image:

```bash
docker images | grep spring-boot-app
```

Check pushed image:

```bash
docker pull naveenreddy9/spring-boot-app:<BUILD_NUMBER>
```

Check Kubernetes rollout:

```bash
kubectl rollout status deployment/spring-boot-app
```

Check pod logs:

```bash
kubectl logs -l app=spring-boot-app
```

Check Argo CD apps:

```bash
kubectl get applications -n argocd
```

## Final Workflow

1. Developer pushes code to GitHub.
2. Jenkins pipeline starts.
3. Jenkins builds the Spring Boot application.
4. Jenkins runs SonarQube analysis.
5. Jenkins builds and pushes Docker image.
6. Jenkins updates `deployment.yml` with the new image tag.
7. Jenkins commits and pushes the manifest change.
8. Argo CD detects the Git change.
9. Argo CD syncs the new version to Kubernetes.
10. Kubernetes runs the updated Spring Boot application.
