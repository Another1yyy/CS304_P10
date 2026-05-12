# Practice10 - CI/CD with Jenkins and Docker

## Task

1. Build a Docker image by Jenkins.
2. Push the image to Docker Hub by Jenkins.
3. Run three containers by Jenkins on ports `8082`, `8083`, and `8084`.

## Implementation

The project contains:

- `index.html`: nginx static web page.
- `Dockerfile`: Docker image definition.
- `Jenkinsfile`: Jenkins Pipeline that builds, pushes, and runs the containers.

## Jenkins Configuration

Create a Jenkins Pipeline job from this repository.

Add a Jenkins credential:

- Type: `Username with password`
- ID: `dockerhub-credentials`
- Username: Docker Hub username
- Password: Docker Hub access token or password

The pipeline uses the username stored in:

- Jenkins credential ID: `dockerhub-credentials`

## Expected Jenkins Result

The Jenkins pipeline has these stages:

1. `Checkout`
2. `Build Docker Image`
3. `Push to Docker Hub`
4. `Run Three Containers`

## Docker Hub Repository

Repository:

```text
https://hub.docker.com/r/yyyyyysaqqqq/cs304-practice10
```

## Running Containers

After a successful run, the following containers should be available:

```text
http://localhost:8082
http://localhost:8083
http://localhost:8084
```

Verification command:

```bash
docker ps --filter "name=cs304-practice10"
```
