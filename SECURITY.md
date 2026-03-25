# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.0.x   | ✅ Yes    |

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

Please report security issues by emailing: `security@<your-domain>.com`

Include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact

You will receive a response within 48 hours. We aim to release a patch within 7 days of confirmation.

## Security Measures in This Project

- OWASP Dependency-Check scans all Maven dependencies for known CVEs on every build
- Trivy scans both the filesystem and Docker image — pipeline fails on CRITICAL or HIGH findings
- Docker container runs as a non-root user (`appuser`, UID 1001)
- Multi-stage Docker build ensures no build tools or source code are present in the runtime image
- SonarQube enforces a Quality Gate that blocks merges with security hotspots
