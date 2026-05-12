# CS304 Practice 10 - CI/CD with Jenkins and Docker

This project completes the Practice 10 requirements:

1. Jenkins builds a Docker image.
2. Jenkins pushes the image to Docker Hub.
3. Jenkins starts three containers on ports `8082`, `8083`, and `8084`.

## Files

- `index.html`: Demo web page served by nginx.
- `Dockerfile`: Builds the Docker image.
- `Jenkinsfile`: Windows Jenkins Pipeline for build, push, and deployment.

## Jenkins Setup

1. Push this folder to a Git repository.
2. In Jenkins, create a Pipeline job and point it to the repository.
3. Add Docker Hub credentials in Jenkins:
   - Kind: `Username with password`
   - ID: `dockerhub-credentials`
   - Username: your Docker Hub username
   - Password: your Docker Hub access token or password
4. Run the Jenkins job. The image repository username is read from the `dockerhub-credentials` Jenkins credential.

## Expected Result

After the pipeline succeeds:

- Docker Hub contains the image repository: `yyyyyysaqqqq/cs304-practice10`
- Three containers are running:
  - `http://localhost:8082`
  - `http://localhost:8083`
  - `http://localhost:8084`

Useful verification commands:

```bash
docker images | grep cs304-practice10
docker ps --filter "name=cs304-practice10"
curl http://localhost:8082
curl http://localhost:8083
curl http://localhost:8084
```
