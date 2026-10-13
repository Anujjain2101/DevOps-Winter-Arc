# DevOps Winter Arc — Day 7

## Overview

This project covers Git and GitHub workflows, Nginx reverse proxy configuration, and hands-on troubleshooting of an application outage.

## Git & GitHub Concepts

* Repository initialization and commits
* Feature branches and merging
* `.gitignore` for excluding files such as `.env`, logs, and `node_modules/`
* Pushing branches to GitHub
* Pull Requests, code review, and merging
* Synchronizing local branches using `git pull`

## Nginx Reverse Proxy Lab

Configured Nginx to forward requests for `day7.local` to a Python HTTP server running on `127.0.0.1:3000`.

**Request flow:**

`Client → Nginx (Port 80) → Python HTTP Server (Port 3000)`

### Key Commands

```bash
# Check Nginx configuration
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx

# Check listening ports
sudo ss -lntp

# Test the application directly
curl -I http://127.0.0.1:3000

# Test through Nginx
curl -i http://day7.local

# Inspect Nginx error logs
sudo tail -n 20 /var/log/nginx/error.log
```

## Troubleshooting Exercise: 502 Bad Gateway

**Problem:** Nginx returned `502 Bad Gateway` after the Python HTTP server was stopped.

**Investigation:**

* Verified that no process was listening on port `3000`.
* Tested the upstream directly using `curl`.
* Inspected Nginx error logs and found `Connection refused`.

**Root cause:** The upstream Python application was unavailable.

**Resolution:** Restarted the Python HTTP server on port `3000` and verified that requests through Nginx returned `200 OK`.

## Key Learnings

* Use feature branches to isolate changes before merging.
* Validate Nginx configuration before reloading.
* Troubleshoot systematically using port checks, direct connectivity tests, and application logs.
* Verify service recovery end to end.

## Environment

* Ubuntu on AWS EC2
* Git and GitHub
* Nginx
* Python HTTP server
