# rp_test_tasks
## WordPress / Apache2
file: wordpress/wordpress.conf

## Laravel / Nginx + PHP-FPM
file: example.com.conf

## Node.js Service Deploy
file: my-node-app.service

## Github Actions Pipeline
file: .github/workflows/dummy-node.yml

## Docker Compose
file: docker/docker-compose.yml
github-action: Deploy WordPress with LocalTunnel (Nginx + No Signup)
Wrote pipeline for deploying and running worpress mysql and nginx with docker compose public access is enabled via local tunnel

SSL Termination via Cloudflare
For production deployments, SSL termination should be handled by Cloudflare rather than LocalTunnel, providing better security, performance, and reliability.
Implementation Approach
1. Cloudflare Tunnel Setup:

Replace LocalTunnel with Cloudflare Tunnel (cloudflared)
Configure a custom domain managed by Cloudflare
SSL/TLS certificates handled automatically by Cloudflare
