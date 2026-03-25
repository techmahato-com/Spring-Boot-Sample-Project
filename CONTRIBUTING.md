# Contributing to Spring Boot Sample Project

Thank you for your interest in contributing! Please follow these guidelines.

## How to Contribute

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to your fork: `git push origin feature/your-feature-name`
5. Open a Pull Request against `main`

## Commit Message Convention

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add new endpoint
fix: resolve null pointer in HelloController
docs: update README setup steps
ci: add trivy scan step to pipeline
refactor: extract service layer
```

## Code Standards

- Java 17, Spring Boot 3.x conventions
- All new code must have unit tests
- Run `mvn verify` locally before pushing — all checks must pass
- No hardcoded secrets or credentials

## Pull Request Checklist

- [ ] Tests pass (`mvn test`)
- [ ] No new SonarQube issues introduced
- [ ] Dockerfile still builds successfully
- [ ] README updated if behaviour changed

## Reporting Issues

Use the GitHub Issues tab with the provided templates for bugs or feature requests.
