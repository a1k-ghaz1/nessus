To install Nessus on your Kali Linux using Docker, you can follow the steps outlined in the GitHub repository you found. Here's a step-by-step guide:

### Step 1: Install Docker on Kali Linux
First, make sure you have Docker installed. If not, you can install it using the following commands:

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### Step 2: Pull the Nessus Docker Image
Pull the Nessus Docker image from Docker Hub:

```bash
docker pull ramisec/nessus
```

### Step 3: Run the Nessus Container
Run the Nessus container with the necessary port mappings:

```bash
docker run -itd --name=ramisec_nessus -p 8834:8834 ramisec/nessus
```

### Step 4: Update Nessus Plugins
To update the Nessus plugins, you need to execute the update script inside the Docker container. First, obtain the `UPDATE_URL_YOU_GOT` from the Nessus website:

```bash
docker exec -it ramisec_nessus /nessus/update.sh "UPDATE_URL_YOU_GOT"
```

**Note:** Replace `UPDATE_URL_YOU_GOT` with the actual update URL you received from the Nessus website.

### Step 5: Access Nessus Web Interface
Open your web browser and navigate to `https://localhost:8834`. You should see the Nessus login page. The default account and password are mentioned in the repository, but you might need to solve for the password given in the updates.

### Step 6: Migration (if needed)
If you need to migrate from an old version to a new version, follow these commands:

1. **Create a directory for Nessus data:**
   ```bash
   mkdir ~/nessus_data
   ```

2. **Stop the existing container:**
   ```bash
   docker stop ramisec_nessus
   ```

3. **Copy Nessus data from the container to the host:**
   ```bash
   docker cp ramisec_nessus:/opt/nessus/var/nessus/ ~/nessus_data
   ```

4. **Delete the old container:**
   ```bash
   docker rm ramisec_nessus
   ```

5. **Run the new container with volume mapping:**
   ```bash
   docker run -itd --name=ramisec_nessus -v ~/nessus_data/nessus/:/opt/nessus/var/nessus/ -p 8834:8834 ramisec/nessus
   ```

6. **Update plugins again:**
   ```bash
   docker exec -it ramisec_nessus /bin/bash /nessus/update.sh
   ```

### Additional Notes
- Ensure your network connection is stable to avoid issues during the update.
- The default credentials might need to be figured out as mentioned in the repository update logs.

By following these steps, you should be able to install and run Nessus on your Kali Linux using Docker. If you encounter any issues, refer to the repository documentation or seek help from the community.
