# 🏠 HomeLab — Self-Hosted Infrastructure & Monitoring

A personal **self-hosted homelab** built to practice Linux administration, Docker, networking, reverse proxies, DNS, monitoring, and infrastructure management.

The goal of this project is to build and maintain a small production-like environment where different services communicate through a controlled Docker network and are exposed through a reverse proxy.

> 🚧 **Project status:** Ongoing — additional services and improvements will be added over time.

---

## 📌 Overview

This homelab is currently running on an **Ubuntu Server virtual machine** and uses **Docker** to deploy and manage multiple infrastructure and monitoring services.

The environment includes:

* 🐳 Docker
* 🛠️ Portainer
* 🌐 Caddy Reverse Proxy
* 🛡️ AdGuard Home
* 🏠 Homepage Dashboard
* 📈 Prometheus
* 📊 Grafana
* 🟢 Uptime Kuma
* 🖥️ Node Exporter

The project is designed to simulate a small infrastructure environment while providing hands-on experience with:

* Linux system administration
* Containerization
* Docker networking
* Reverse proxy configuration
* DNS and local service discovery
* Infrastructure monitoring
* Metrics collection
* Dashboard creation
* Service availability monitoring
* Troubleshooting distributed services

---

# 🏗️ Architecture

The current environment follows a simple architecture:

```text
                         ┌─────────────────────┐
                         │     Ubuntu Server   │
                         │        VM           │
                         └──────────┬──────────┘
                                    │
                                    ▼
                              ┌───────────┐
                              │   Docker  │
                              └─────┬─────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
       ┌─────────────┐       ┌─────────────┐       ┌─────────────┐
       │   Caddy     │       │  Portainer  │       │  AdGuard    │
       │ Reverse     │       │   Docker    │       │    Home     │
       │   Proxy     │       │ Management  │       │    DNS      │
       └──────┬──────┘       └─────────────┘       └─────────────┘
              │
              │
      ┌───────┴──────────────────────────────────┐
      │                                          │
      ▼                                          ▼
┌─────────────┐                          ┌─────────────┐
│  Homepage   │                          │ Uptime Kuma │
│  Dashboard  │                          │ Monitoring  │
└─────────────┘                          └─────────────┘

                    Monitoring Stack
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
      ┌─────────────┐         ┌─────────────┐
      │ Prometheus  │◄────────│Node Exporter│
      │   Metrics   │         │ Host Metrics│
      └──────┬──────┘         └─────────────┘
             │
             ▼
      ┌─────────────┐
      │   Grafana   │
      │ Dashboards  │
      └─────────────┘
```

---

# 🧰 Technologies

| Technology        | Purpose                              |
| ----------------- | ------------------------------------ |
| **Ubuntu Server** | Host operating system                |
| **Docker**        | Containerization platform            |
| **Portainer**     | Docker/container management          |
| **Caddy**         | Reverse proxy                        |
| **AdGuard Home**  | DNS and network-wide ad blocking     |
| **Homepage**      | Centralized service dashboard        |
| **Uptime Kuma**   | Service availability monitoring      |
| **Prometheus**    | Metrics collection and storage       |
| **Grafana**       | Metrics visualization and dashboards |
| **Node Exporter** | Linux host metrics                   |

---

# 🐳 Docker Infrastructure

Docker is used as the primary containerization platform.

Each service runs independently inside its own container, allowing services to be:

* Started and stopped independently
* Updated without affecting the entire environment
* Connected through Docker networks
* Exposed through controlled ports
* Monitored individually

Docker networking is also used to allow containers to communicate with each other using Docker's internal DNS and container names.

---

# 🌐 Reverse Proxy — Caddy

**Caddy** acts as the central reverse proxy for the homelab.

Instead of accessing services through different ports, services can be accessed through dedicated hostnames.

Example:

```text
homepage.home
portainer.home
adguardhome.home
kuma.home
prometheus.home
grafana.home
```

The reverse proxy routes incoming requests to the appropriate Docker container.

Example architecture:

```text
Browser
   │
   ▼
Caddy
   │
   ├── homepage.home ──────► Homepage
   │
   ├── portainer.home ─────► Portainer
   │
   ├── adguardhome.home ───► AdGuard Home
   │
   ├── kuma.home ──────────► Uptime Kuma
   │
   ├── prometheus.home ────► Prometheus
   │
   └── grafana.home ───────► Grafana
```

This provides a cleaner and more realistic way of exposing internal services.

---

# 🛡️ DNS — AdGuard Home

AdGuard Home is used as the DNS server for the homelab.

Its main purposes are:

* Local DNS resolution
* Network-wide ad blocking
* DNS request monitoring
* Centralized DNS configuration

It also provides a practical introduction to how DNS infrastructure works inside a local network.

---

# 📊 Monitoring Stack

The monitoring infrastructure is based on:

**Prometheus + Node Exporter + Grafana + Uptime Kuma**

### Prometheus

Prometheus collects and stores time-series metrics from monitored services.

The current configuration includes Prometheus itself and a Node Exporter target.

Example target:

```text
node-exporter:9100
```

Prometheus uses a **15-second scrape interval**.

### Node Exporter

Node Exporter exposes Linux host-level metrics such as:

* CPU usage
* Memory usage
* Disk statistics
* Network statistics
* System load
* Filesystem information

### Grafana

Grafana is used to visualize the metrics collected by Prometheus.

The goal is to create dashboards showing the health and performance of the homelab.

Example metrics include:

```text
CPU utilization
Memory utilization
Disk usage
Network traffic
System load
Container/service health
```

### Uptime Kuma

Uptime Kuma is used to monitor service availability.

It allows the homelab to detect when a service becomes unavailable and provides an easy-to-read monitoring dashboard.

---

# 🖼️ Screenshots

Screenshots will be added here to document the actual environment.

## 🏠 Homelab Dashboard

![Home](images/home.png)

---

## 🐳 Portainer

![Portainer](images/portainer.png)

---

## 🛡️ AdGuard Home

![AdGuard](images/adguard.png)

---

## 🌐 Caddy / Reverse Proxy

![Caddy](images/caddy.png)

---

## 📈 Prometheus

![Prometheus](images/prometheus.png)

---

## 📊 Grafana

![Grafana](images/grafana.png)

---

## 🟢 Uptime Kuma

![Kuma](images/kuma.png)

---

# 🔧 Networking

The homelab uses Docker networks to control communication between services.

Services that need to communicate with Caddy are connected to a shared Docker network.

This allows Caddy to communicate with containers using their Docker DNS names instead of relying exclusively on host IP addresses.

Example:

```text
Caddy
  │
  ├── homepage:3000
  ├── adguardhome:80
  ├── kuma:3001
  ├── prometheus:9090
  └── grafana:3000
```

This setup provided practical experience troubleshooting:

* Docker DNS resolution
* Container-to-container communication
* Network membership
* Port conflicts
* Reverse proxy connectivity
* Service availability

---

# 🔍 Troubleshooting & Lessons Learned

One of the main objectives of this homelab is learning how to troubleshoot infrastructure problems instead of simply deploying applications.

Some issues encountered during the project included:

### Docker Networking

Containers were initially distributed across different Docker networks, which caused connectivity problems between services.

This required investigating container network membership and ensuring that services that needed to communicate were connected to the appropriate network.

### Reverse Proxy Issues

Caddy initially returned errors when it could not resolve certain Docker container names.

This demonstrated the importance of understanding Docker's internal DNS and network isolation.

### Port Conflicts

Some services attempted to use ports that were already occupied by other applications.

For example, Prometheus and other services required checking which ports were already in use before deployment.

### Monitoring Troubleshooting

Prometheus successfully connected to Node Exporter, but some PromQL queries initially returned no data.

This led to investigating:

* Prometheus targets
* Scrape configuration
* Exporter availability
* Labels
* Job names
* Docker networking
* PromQL queries

These problems provided practical experience debugging a monitoring stack rather than simply following a deployment tutorial.

---

# 📚 Skills Demonstrated

Through this project, I developed practical experience with:

### Linux

* Ubuntu Server administration
* Services and processes
* Networking
* System troubleshooting
* File and configuration management

### Docker

* Container deployment
* Docker Compose / stacks
* Container networking
* Port mapping
* Docker DNS
* Container troubleshooting

### Networking

* DNS
* Reverse proxies
* HTTP/HTTPS
* Local service discovery
* Port management
* Network isolation

### Monitoring

* Prometheus
* PromQL
* Grafana
* Node Exporter
* Uptime monitoring
* Metrics troubleshooting

### Infrastructure

* Self-hosted services
* Service management
* Infrastructure organization
* Monitoring and observability
* Troubleshooting distributed services

---

# 🚀 Future Improvements

The homelab is an ongoing project.

Planned improvements include:

* [ ] Add additional DNS/networking services
* [ ] Improve Grafana dashboards
* [ ] Add more Prometheus exporters
* [ ] Monitor Docker containers
* [ ] Add centralized logging
* [ ] Implement automated backups
* [ ] Improve Docker network architecture
* [ ] Add alerting
* [ ] Add HTTPS certificates where appropriate
* [ ] Automate deployments with Docker Compose
* [ ] Add a dedicated NAS/storage service
* [ ] Add a Git-based CI/CD workflow
* [ ] Document infrastructure configuration
* [ ] Expand the environment with additional virtual machines

---

# 🎯 Project Goals

The main purpose of this homelab is to gain practical experience with technologies commonly used in modern IT infrastructure and DevOps environments.

Rather than only learning these technologies theoretically, the project provides hands-on experience with:

```text
Deploy
   ↓
Configure
   ↓
Connect
   ↓
Monitor
   ↓
Troubleshoot
   ↓
Improve
```

The environment is continuously evolving as new technologies and services are tested.

---

# 📁 Repository Structure

A possible repository structure:

```text
homelab/
│
├── README.md
│
├── images/
│   ├── homepage.png
│   ├── portainer.png
│   ├── adguard.png
│   ├── caddy.png
│   ├── prometheus.png
│   ├── grafana.png
│   └── uptime-kuma.png
│
├── docker/
│   ├── compose/
│   └── configs/
│
├── caddy/
│   └── Caddyfile
│
├── prometheus/
│   └── prometheus.yml
│
└── documentation/
    └── notes.md
```

---

# 📈 Status

| Component     | Status     |
| ------------- | ---------- |
| Ubuntu Server | 🟢 Running |
| Docker        | 🟢 Running |
| Portainer     | 🟢 Running |
| Caddy         | 🟢 Running |
| AdGuard Home  | 🟢 Running |
| Homepage      | 🟢 Running |
| Uptime Kuma   | 🟢 Running |
| Prometheus    | 🟢 Running |
| Grafana       | 🟢 Running |
| Node Exporter | 🟢 Running |

---

# 👨‍💻 Author

**Abdellah El Berdai**

Software Engineer
Morocco

This homelab is a personal infrastructure project created to develop practical skills in **Linux, Docker, networking, monitoring, and infrastructure administration**.

---

⭐ If you find this project useful, feel free to explore the repository and follow its development.
