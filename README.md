🌍 TravelGo — Containerized Three-Tier Travel Booking Platform

TravelGo is a full-stack travel booking web application architected, containerized, and deployed on AWS EC2. Customers can browse travel destinations and submit trip reservations, while administrators can securely log in to view and manage customer bookings stored in a persistent MySQL 8.0 database.

📸 Application Screenshots
<img width="1907" height="1038" alt="Screenshot 2026-11-22 121133" src="https://github.com/user-attachments/assets/867896df-3fa1-44fc-8441-2a7f030a71fd" />
<img width="1901" height="1037" alt="Screenshot 2026-11-22 121203" src="https://github.com/user-attachments/assets/dd267681-28d4-4218-963e-4ceb4a077360" />
<img width="1892" height="1036" alt="Screenshot 2026-11-22 121229" src="https://github.com/user-attachments/assets/5154f0ec-406a-415d-8eba-5983cdb983a3" />

🛠️ Key Implementations & Project Highlights

Multi-Container Dockerization: Packaged the frontend and backend services into custom Docker images (alhadhshihab/travel-frontend:v1 and alhadhshihab/travel-backend:v1) and published them to Docker Hub for consistent, portable deployment across environments.

Three-Tier Service Architecture: Designed a declarative docker-compose.yml setup separating Presentation (travel-frontend), Application Logic (travel-backend), and Database Persistence (travel-db) into isolated containers.

Reverse Proxy & Traffic Routing: Configured Nginx (client/nginx.conf) on port 80 to serve static frontend assets and act as a reverse proxy forwarding /api/* requests internally to the Node.js/Express backend on port 5000.

Container Networking & Service Isolation: Built a custom user-defined Docker bridge network (travel-net) enabling container-to-container communication via internal DNS hostnames (travel-db, travel-backend) while keeping the MySQL database isolated from direct public access.

Persistent Database Storage: Configured a Linux host bind mount (/home/ubuntu/travel-backup/mysql-data:/var/lib/mysql) on the MySQL 8.0 container so customer booking records remain intact across container restarts, rebuilds, and updates.

Secret Management & Repository Security: Removed hardcoded database passwords, session keys, and admin credentials from docker-compose.yml by externalizing secrets into a server-local .env file excluded from version control via .gitignore.

Cloud Deployment & Firewall Configuration: Provisioned an AWS EC2 Ubuntu 24.04 LTS server and configured stateful AWS Security Groups to restrict inbound network traffic strictly to required ports (22 SSH, 80 HTTP, and 5000 API).

Service Reliability & Auto-Recovery: Implemented container startup dependencies (depends_on) and restart: always policies across all services to ensure automatic recovery after server reboots.

Custom Domain & DNS Integration: Configured custom domain routing via DNS A records (@ and www) pointing directly to the cloud server's public IPv4 endpoint.

## 🏗️ Three-Tier System Architecture

```mermaid
flowchart TD
    Client["🌐 Internet / Client Browser<br/>(Public Domain / EC2 IP)"] -->|HTTP :80| Frontend["🖥️ travel-frontend (Port 80:80)<br/>Nginx Reverse Proxy & Static UI"]
    Frontend -->|Internal DNS: travel-backend:5000| Backend["⚙️ travel-backend (Port 5000:5000)<br/>Node.js / Express REST API & Auth"]
    Backend -->|Internal DNS: travel-db:3306| DB[("🗄️ travel-db (MySQL 8.0)<br/>Database: travelbooking")]
    DB --- Volume["💾 Persistent Host Volume<br/>/home/ubuntu/travel-backup/mysql-data"]


🚀 Tech Stack

Cloud & OS: AWS EC2, Ubuntu 24.04 LTS, AWS Security Groups

Containerization & Registry: Docker Engine, Docker Compose v2, Docker Hub

Web Server & Reverse Proxy: Nginx

Frontend: HTML5, CSS3, JavaScript

Backend: Node.js, Express.js, mysql2, dotenv, express-session

Database: MySQL 8.0

DNS & Version Control: Custom DNS (A Records), Git, GitHub


📂 Project Structure

TravelBooking/
├── client/                  # Tier 1: Frontend UI & Nginx reverse proxy config
│   ├── Dockerfile
│   └── nginx.conf
├── server/                  # Tier 2: Node.js & Express REST API
│   └── Dockerfile
├── screenshots/             # Application UI screenshots
├── docker-compose.yml       # Multi-container orchestration & network definition
├── .gitignore               # Excludes .env and local artifacts from Git
└── README.md                # Project documentation
