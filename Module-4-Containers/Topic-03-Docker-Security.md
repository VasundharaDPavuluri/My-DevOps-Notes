# 🔐 Docker Security: Run Containers as Non-Root User
By default, Docker containers run as the root user (UID 0).
While convenient, this increases the security risk in production environments.
________________________________________
## 🚨 The Problem

When a container runs as root:

* it has elevated privileges inside the container
* a compromised application gains full control
* container breakout risks increase
* host and other containers may be exposed
* security audits often flag this configuration
  
Containers share the host kernel, so minimizing privileges is critical.
________________________________________
🛡️ Why This Matters
If an attacker exploits your application:
Running as root
•	install malicious tools
•	scan internal network
•	attempt host escape
•	pivot to other services
Running as non-root
•	limited permissions
•	restricted system access
•	reduced damage scope
This follows the Principle of Least Privilege.
________________________________________
📁 Default Behavior (Runs as Root)
FROM node:18-alpine

WORKDIR /app
COPY . .

RUN npm install

CMD ["node", "server.js"]
Verify:
docker exec -it <container> whoami
Output:
root
________________________________________
	Secure Approach: Run as Non-Root User
---
FROM node:18-alpine

# Create group and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app
COPY . .

RUN npm install

# switch to non-root user
USER appuser

CMD ["node", "server.js"]
---
Verify:
docker exec -it <container> whoami
Output:
appuser
________________________________________
	Best Practice
•	Use root only during build/setup steps
•	Switch to non-root before runtime
•	Follow least-privilege principle
________________________________________
	Additional Hardening Options
•	Use read-only filesystem
•	Drop unnecessary Linux capabilities
•	Avoid privileged mode
•	Use minimal/distroless images
•	Scan images for vulnerabilities
________________________________________
	Benefits
•	Reduced attack surface
•	Lower container breakout risk
•	Improved compliance & audit readiness
•	Production-grade security posture
________________________________________
	When Root May Still Be Required
Some applications may require root for:
•	binding ports below 1024
•	system-level utilities
•	legacy workloads
Use only when necessary.
________________________________________
•	Running containers as non-root users reduces privilege levels and limits potential damage if the application is compromised.
