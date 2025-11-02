Here’s a **step-by-step guide** to install and run **Jenkins using Docker Compose** — a clean, reproducible setup that works on most systems (Linux, macOS, Windows with Docker Desktop).

----------

## 🧰 Prerequisites

Make sure you have:

-   Docker installed
    
-   [Docker Compose](https://docs.docker.com/compose/install/) installed (or use `docker compose` CLI built into Docker Desktop)
    

----------

## 🗂️ Step 1: Create a project directory

```bash
mkdir jenkins-docker
cd jenkins-docker
```

----------

## 📝 Step 2: Create a `docker-compose.yml` file

Create the file:

`nano docker-compose.yml` 

And paste this configuration:

```yaml
version: '3.8'

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    restart: unless-stopped
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock  # optional, for building Docker images inside Jenkins

volumes:
  jenkins_home:

``` 

### Explanation:

-   **Image**: `jenkins/jenkins:lts` – uses the Long-Term Support Jenkins version.
    
-   **Ports**:
    
    -   `8080` → Jenkins web UI.
        
    -   `50000` → Agent connection port.
        
-   **Volume `jenkins_home`** stores Jenkins data persistently.
    
-   Mounting `/var/run/docker.sock` allows Jenkins to use Docker inside its environment (optional but useful).
    

----------

## 🚀 Step 3: Start Jenkins

```bash
docker compose up -d
```

This will:

-   Pull the Jenkins image (if not already downloaded)
    
-   Start the Jenkins container in the background
    

Check container status:

`docker compose ps` 

----------

## 🔑 Step 4: Unlock Jenkins

1.  Open Jenkins in your browser:
    
    `http://localhost:8080` 
    
2.  Get the initial admin password:
    
    ```bash
    docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
    ``` 
    
3.  Copy and paste that into the web UI.
    
4.  Follow the setup wizard to:
    
    -   Install suggested plugins
        
    -   Create your admin user
        

----------

## 🧹 Step 5: Stop Jenkins (if needed)

`docker compose down` 

This stops the container but keeps your data (since it’s stored in a Docker volume).

If you want to **completely remove everything**, including data:

`docker compose down -v` 

----------

## ✅ Optional: Run Jenkins with Docker agent support

If you plan to run Docker builds inside Jenkins, make sure your Jenkins container can access the Docker daemon (the socket mount `/var/run/docker.sock` handles that).

----------

