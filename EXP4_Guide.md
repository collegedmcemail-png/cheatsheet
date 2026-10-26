# Experiment 4: Containerizing Applications Using Docker

## 4.1 Objective

Create a simple Java application, place it in a Docker image, run it as a container, and verify the output and container status.

## 4.2 Prerequisites

- A computer with virtualization enabled
- Docker Desktop
- A JDK for compiling and testing the Java program locally
- Internet access to download the base image

---

## 4.3 Install Docker Desktop

### Download and Install

1. Download Docker Desktop from:

   ```text
   https://www.docker.com/products/docker-desktop/
   ```

2. Run the installer.
3. Accept the license agreement.
4. Keep the default installation options unless your laboratory requires a Linux container setup.
5. Complete the installation.
6. Restart the computer if requested.
7. Sign in to Docker Desktop.
8. Wait until Docker Desktop reports:

   ```text
   Engine running
   ```

### Verify Docker

Open PowerShell:

```powershell
docker --version
docker info
```

Check container status:

```powershell
docker ps
```

Expected output:

```text
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
```

There may be no running containers, but no error should appear.

---

## 4.4 Select Linux or Windows Containers

Docker Desktop can run either Linux containers or Windows containers.

### Check the Current Mode

```powershell
docker info --format '{{.OSType}}'
```

Expected for the Java example:

```text
linux
```

### Switch to Linux Containers

In Docker Desktop:

1. Open **Settings**.
2. Select **General**.
3. Under **Choose container backend to start**, select **Linux containers**.
4. Click **Apply & restart**.

A Windows-container version can also be created, but Linux containers are the usual choice for this experiment.

---

## 4.5 Create the Project Directory

Open PowerShell:

```powershell
cd C:\Users\Documents
mkdir DockerDemo
cd DockerDemo
```

Confirm the location:

```powershell
Get-Location
```

Expected path:

```text
C:\Users\Documents\DockerDemo
```

If `C:\Users\Documents` does not exist, use your actual user directory:

```powershell
cd $HOME
mkdir DockerDemo
cd DockerDemo
```

---

## 4.6 Write the Java Program

Create `HelloDocker.java`:

```powershell
@"
public class HelloDocker {
    public static void main(String[] args) {
        System.out.println("Hello Docker World");
    }
}
"@ | Set-Content -Encoding UTF8 HelloDocker.java
```

Verify the file:

```powershell
Get-Content HelloDocker.java
```

Expected content:

```java
public class HelloDocker {
    public static void main(String[] args) {
        System.out.println("Hello Docker World");
    }
}
```

---

## 4.7 Test the Java Program Locally

If Java is installed:

```powershell
javac HelloDocker.java
java HelloDocker
```

Expected output:

```text
Hello Docker World
```

Remove the local `.class` file if desired:

```powershell
Remove-Item HelloDocker.class
```

The Docker build can compile the source inside the image, so the local `.class` file is not required.

---

## 4.8 Create the Dockerfile

Create a file named exactly `Dockerfile` without `.txt` or another extension.

Using PowerShell:

```powershell
@"
FROM eclipse-temurin:21-jdk
WORKDIR /app
COPY HelloDocker.java .
RUN javac HelloDocker.java
CMD ["java", "HelloDocker"]
"@ | Set-Content -Encoding UTF8 Dockerfile
```

Verify the file:

```powershell
Get-Content Dockerfile
```

### Dockerfile Explanation

| Instruction | Purpose |
|---|---|
| `FROM eclipse-temurin:21-jdk` | Uses Java 21 with the JDK installed |
| `WORKDIR /app` | Sets the working directory inside the image |
| `COPY HelloDocker.java .` | Copies the source file into the image |
| `RUN javac HelloDocker.java` | Compiles the Java source during the build |
| `CMD ["java", "HelloDocker"]` | Sets the default container command |

Verify that the Dockerfile has the correct name:

```powershell
Get-ChildItem
```

Expected files:

```text
Dockerfile
HelloDocker.java
```

---

## 4.9 Build the Docker Image

Make sure you are in the project directory:

```powershell
cd C:\Users\Documents\DockerDemo
```

Build the image:

```powershell
docker build -t dockerdemo .
```

Expected output includes lines similar to:

```text
[+] Building 8.0s
 => [1/4] FROM eclipse-temurin:21-jdk
 => [2/4] WORKDIR /app
 => [3/4] COPY HelloDocker.java .
 => [4/4] RUN javac HelloDocker.java
 => exporting to image
```

### Verify the Image

```powershell
docker images
```

You should see an image named:

```text
dockerdemo
```

To inspect it directly:

```powershell
docker image inspect dockerdemo
```

---

## 4.10 Run the Container

Run the container:

```powershell
docker run dockerdemo
```

Expected output:

```text
Hello Docker World
```

The container may stop immediately after the Java program exits. This is expected for a one-time command container.

### Run and Keep the Container Identifiable

```powershell
docker run --name dockerdemo-container dockerdemo
```

Expected output:

```text
Hello Docker World
```

---

## 4.11 Verify the Container Status

List all containers:

```powershell
docker ps -a
```

Expected information should show:

- Container name: `dockerdemo-container`
- Image: `dockerdemo`
- Status: `Exited (0)`
- Command: `java HelloDocker`

Inspect a specific container:

```powershell
docker inspect dockerdemo-container
```

View its logs:

```powershell
docker logs dockerdemo-container
```

Expected output:

```text
Hello Docker World
```

Remove the container:

```powershell
docker rm dockerdemo-container
```

This is optional and demonstrates container cleanup.

---

## 4.12 Verify in Docker Desktop

1. Open Docker Desktop.
2. Select **Containers** in the left navigation.
3. Locate the container named `dockerdemo-container`.
4. If it is no longer listed, use **Images** and run the `dockerdemo` image again.
5. Select the container.
6. Open the **Logs** tab.
7. Confirm the following message:

   ```text
   Hello Docker World
   ```

8. In the container details, verify:
   - Image: `dockerdemo`
   - Exit code: `0`
   - State: Exited
   - Created timestamp
9. Take a screenshot showing:
   - The container in Docker Desktop
   - The `Hello Docker World` log
   - The successful exit status

---

## 4.13 Optional: Run the Container in Interactive Mode

Start a shell inside a Linux container:

```powershell
docker run -it --rm eclipse-temurin:21-jdk bash
```

Inside the container:

```bash
java -version
exit
```

This confirms that Java is available in the base image.

---

## 4.14 Optional: Tag and Push the Image to Docker Hub

### Log In

```powershell
docker login
```

Enter your Docker Hub username and access token.

### Tag the Image

```powershell
docker tag dockerdemo your-username/dockerdemo:latest
```

### Push It

```powershell
docker push your-username/dockerdemo:latest
```

### Pull It Back

```powershell
docker pull your-username/dockerdemo:latest
```

---

## 4.15 Troubleshooting

### `docker` Is Not Recognized

1. Restart the computer or reopen PowerShell.
2. Confirm that Docker Desktop is running.
3. Check whether Docker is included in `PATH`.
4. Try:

   ```powershell
   & "C:\Program Files\Docker\Docker\resources\bin\docker.exe" --version
   ```

### Docker Daemon Is Not Running

Open Docker Desktop and wait until the engine is running. Then run:

```powershell
docker info
```

### No Space Left on Device

Inspect disk usage:

```powershell
docker system df
```

Remove unused Docker data:

```powershell
docker system prune
```

Do not add the `-a` or `--volumes` options unless you understand which data will be permanently deleted.

### Port or Firewall Error

A port is not required for this basic application because it does not start a server. If a port error appears, use a different host port with `-p`, or review the container's command and application.

### Java Compiler Not Found

Confirm that the image tag includes the JDK, not only the Java Runtime Environment:

```dockerfile
FROM eclipse-temurin:21-jdk
```

Rebuild without using the cache:

```powershell
docker build --no-cache -t dockerdemo .
```

### Text Appears in the Wrong Encoding

Use:

```powershell
notepad Dockerfile
```

Save it with UTF-8 encoding, or create it in VS Code and ensure that the file is saved as `Dockerfile`.

---

## 4.16 Conclusion

The experiment demonstrated the complete containerization workflow for a Java application. The source code was compiled inside a Docker image, and the image was executed successfully as an isolated container. The expected `Hello Docker World` output and successful exit code confirmed that the containerized application worked as expected. Docker therefore provided a repeatable and portable way to package and execute the application.

---
