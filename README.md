# Azure 3-Tier Architecture with NSG Rules
This project demonstrates how to deploy and secure a **3-tier architecture** in Azure using **Virtual Network (VNet)**, **Subnets**, and **Network Security Groups (NSGs)**.  
The setup includes a **Web Server**, **App Server**, and **Database Server**, each hosted in separate subnets within a single VNet.

## Project Overview
- **Resource Group**: `3tierproject`  
- **Location**: South India  
- **Operating System**: Linux  
- **VM Size**: Standard_B1s  

## Virtual Network & Subnets
- **VNet CIDR**: `10.0.0.0/16`
- **Subnets**:
  - **Web Subnet** → `10.0.1.0/24`
  - **App Subnet** → `10.0.2.0/24`
  - **DB Subnet** → `10.0.3.0/24`

## Virtual Machines

| Name        | Subnet     | Public IP    | Status   | OS    | Size         |
|-------------|-----------|--------------|----------|-------|--------------|
| Web-server  | Web Subnet | 4.247.27.79  | Running  | Linux | Standard_B1s |
| App-server  | App Subnet | 20.44.57.67  | Running  | Linux | Standard_B1s |
| DB-server   | DB Subnet  | 20.235.182.252 | Running | Linux | Standard_B1s |

---

## NSG Rules

### Web Server NSG
| Rule                | Source | Destination | Port   | Action |
|---------------------|--------|-------------|--------|--------|
| Allow HTTP/HTTPS    | Any    | Web Server  | 80/443 | Allow  |
| Allow SSH (optional)| Any    | Web Server  | 22     | Allow  |

### App Server NSG
| Rule                  | Source                | Destination | Port  | Action |
|-----------------------|-----------------------|-------------|-------|--------|
| Allow App Traffic     | Web Subnet (10.0.1.0/24) | App Server | 8080  | Allow  |

### DB Server NSG
| Rule                  | Source                | Destination | Port  | Action |
|-----------------------|-----------------------|-------------|-------|--------|
| Allow DB Traffic      | App Subnet (10.0.2.0/24) | DB Server  | 3306  | Allow  |
| Deny Web to DB        | Web Subnet (10.0.1.0/24) | DB Server  | Any   | Deny   |

---

## Traffic Flow
- Internet → Web Server (80/443) → **Allowed**  
- Web Server → App Server (8080) → **Allowed**  
- App Server → DB Server (3306) → **Allowed**  
- Web Server → DB Server (Any port) → **Denied**  

