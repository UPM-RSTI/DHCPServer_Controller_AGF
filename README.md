# DHCPServer_Controller_AGF

A Go-based controller for dynamic AGF pool management, combining:

- a **DHCP server** to reassign UE IP addresses according to the target AGF,
- a **REST API** to register AGFs/users and trigger mobility workflows,
- a **web frontend** to visualize registered AGFs and users and trigger handovers.

## What this repository contains

The project is structured into three main parts:

- `api/`  
  REST API routes for:
  - AGF registration
  - user registration
  - AGF/user listing
  - DHCP trigger
  - handover trigger
  - target AGF user registration

- `dhcpserver/`  
  Custom DHCP server logic and DHCP-trigger helper functions.

- `frontend/`  
  React + Vite web UI that shows:
  - registered AGFs
  - registered users
  - a handover action per user

- `docs/`  
  Swagger/OpenAPI generated files.

- `main.go`  
  Entry point that starts:
  - the DHCP server
  - the frontend dev server
  - the REST API server

---

## Architecture overview

The controller acts as the central orchestrator between UEs and AGFs:

1. **AGFs register themselves** in the controller.
2. **Users (UEs) are registered** and associated with their current AGF.
3. When mobility is needed, the controller can:
   - trigger a DHCP procedure on a UE,
   - trigger a handover on the source AGF,
   - instruct the target AGF to register the user,
   - instruct the source AGF to deregister the user.

The frontend provides a simple operational dashboard for these flows.

---

## Main features

- Register AGFs
- Register users
- List registered AGFs
- List registered users
- Trigger DHCP on a UE
- Trigger handover to a target AGF
- Notify a target AGF to register a moved UE
- Swagger UI for API inspection

---

## Requirements

### Backend
- Go `1.23.x`

### Frontend
- Node.js
- npm

### Network/runtime assumptions
This project currently assumes:

- the DHCP server is attached to a specific local interface,
- UEs expose an HTTP endpoint on port `8081`,
- AGFs expose an HTTP API on port `8082`,
- some AGF IDs and IP addresses are hardcoded.

You will likely need to adapt these values for your environment before using the project in production or in a different lab.

---

## Current hardcoded values and limitations

At the moment, the repository is not fully configurable. The following values are hardcoded in the source code:

### Network interface
The DHCP server is started on:

```go
enp1s0np0np0
