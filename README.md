# 🚀 Docker-PNXM (PHP-Nginx-MySQL) Guide  

Welcome to the **Docker-PNXM** repository! This guide will help you set up and use a fully containerized PHP, Nginx, and MySQL environment seamlessly. 🐳🔥  

## ✅ Prerequisites  
&nbsp;  
Before you begin, ensure your system meets the following requirements:  
- 🐳 [Docker Desktop](https://www.docker.com/products/docker-desktop) - Required to manage and run containers.  
- 🖥️ [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install) - Ensures a smooth development experience.  
- ⚙️ Enable WSL integration in Docker Desktop - Allows Docker to work efficiently with WSL.  
&nbsp;  

For a step-by-step guide on setting up **Docker Desktop and WSL**, refer to the [setup guide](./setup-docker-wsl.md). 📖  
## 🚀 Getting Started  
No need to clone this repository! Instead, you can **download the ZIP file** from GitHub and extract it, or use Git in a way that won’t affect the original repository.  
&nbsp;  
### 📥 Download and Use the Repository  
#### Option 1: Download ZIP (Recommended)  
1. Navigate to the GitHub repository.  
2. Click **Code** > **Download ZIP**.  
3. Extract the ZIP file to your preferred location.  
&nbsp;  
#### Option 2: Use Git Without Affecting the Main Repo  
If you prefer using Git but want to keep your changes independent:  
```sh  
git clone https://github.com/your-username/docker-pnxm.git  
cd docker-pnxm  
rm -rf .git  # Remove Git tracking to ensure changes remain local  
```  
This allows you to work freely without affecting the original repository.  
&nbsp;  
## 🪛 Environment Setup
### 1️⃣ Update `docker-compose.yaml`  
Customize the `appname` in `docker-compose.yaml` to match your project name.  
```sh  
services:
	app:
		container_name: appname_php
		networks:
		- appname_internal_network
		...

	nginx:
		container_name: appname_nginx
		networks:
		- appname_internal_network
		...

	mysql:
		container_name: appname_mysql
		volumes:
		- appname_mysql_data:/var/lib/mysql
		networks:
		- appname_internal_network
		...

networks:
  appname_internal_network:

volumes:
  appname_mysql_data:
```  
Using a unique `appname` ensures multiple Docker environments can coexist within WSL without conflicts.  
&nbsp;  

---   
#### 🔖 Running Multiple Docker Projects  
If you have multiple Docker-based projects, ensure unique port assignments to avoid conflicts:  
```sh  
services:
  app:
	...

  nginx:
    ...
    ports:
      - "8081:80" # Map host port 8081 to container port 80
	...

  mysql:
	...
    ports:
      - "3307:3306" # Map host port 3307 to container port 3306
	...
...
```  
&nbsp;  
### 2️⃣ Create a `src` Folder (if not already present)  
```sh  
mkdir src  
```  
This directory will hold your application source code.  
&nbsp;  
### 3️⃣ Update `Dockerfile`  
The `Dockerfile` is located in `./docker/php`. Update the default `myusername` to match your UNIX `WSL` username.  
```sh  
Ubuntu 20.04 LTS is already installed.
...

Please create a default UNIX user account.  
Enter new UNIX username: mynewusername  
New password:
...
```  
```sh  
RUN groupadd -g ${GID} mynewusername && useradd -m -u ${UID} -g mynewusername mynewusername
...
RUN chown -R mynewusername:mynewusername /var/www
...
USER mynewusername
```  
Here, `myusername` is updated to `mynewusername`, matching the UNIX user created during WSL setup.  
&nbsp;  
### 4️⃣ Build and Start Containers  
```sh  
docker-compose build  
docker-compose up -d  
```  
Or simply:  
```sh  
docker-compose up --build -d  
```  
This builds and starts all services in detached mode.  
&nbsp;  
### 5️⃣ Verify Running Containers  
```sh  
docker ps  
```  
&nbsp;  
### 6️⃣ Configure Project Environment  
For `MySQL` to work properly, update your `.env` file with the correct values:  
```sh
DB_CONNECTION=mysql
DB_HOST=mysql # Matches service name in docker-compose.yaml
DB_PORT=3306
DB_DATABASE=mydatabase
DB_USERNAME=root
DB_PASSWORD=secret # Matches MYSQL_ROOT_PASSWORD in docker-compose.yaml
```  
&nbsp;  
In `docker-compose.yaml`:  
```sh
services:
  mysql: # Matches DB_HOST in .env
    image: mysql:latest
    container_name: appname_mysql
	...
    environment:
      MYSQL_ROOT_PASSWORD: secret # Matches DB_PASSWORD in .env
...
```  
&nbsp;  
### 7️⃣ Access the Application 🌐  
- 🖥 Web App: `http://localhost:PORT`  
- 🗄 Database: `localhost:DB_PORT`  
&nbsp;  
## 🛠 Managing the Environment  
### ⏹ Stop Containers  
```sh  
docker-compose down  
```  
&nbsp;  
### 🛠 Execute Commands in a Running Container  
```sh  
docker-compose exec [container_name] [command]  
```  
Example:  
```sh  
docker-compose exec app_container php --version  
```  
---  
Or, enter the container’s shell:  
```sh  
docker-compose exec [container_name] bash  
```  
Example:  
```sh  
docker-compose exec app_container bash  # Enter container
/var/www> php --version # Check PHP version
/var/www> exit # Exit container
```  
&nbsp;  
## 📜 License  
This project is licensed under the MIT License.  
&nbsp;  

---  
&nbsp;  
**Happy Dockering! 🚢✨**
