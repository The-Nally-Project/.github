# Security Policy

Security and user autonomy are foundational requirements for The Nally Project.

## Reporting a vulnerability

Please do not publish details of an unpatched vulnerability in a public issue.

Use GitHub's **Report a vulnerability** feature in the affected repository when private vulnerability reporting is enabled. If it is unavailable, contact the project maintainers through the organisation's published contact channel and provide a concise description and safe reproduction details.

Do not include real user data, credentials, access tokens, or information belonging to other people in a report.

## What to include

- Affected repository, component, and version or commit.
- Impact and conditions required.
- A minimal reproduction or proof of concept that does not access other people's data.
- Any suggested mitigation.

## Security priorities

- Least-privilege access and explicit user approval.
- Short-lived credentials and revocation.
- Secure storage and careful handling of secrets.
- Input validation and safe tool execution.
- Isolation between users, agents, and devices.
- Auditable actions and clear failure behaviour.
- Safe dependency management and timely updates.
- Reliable deletion and retention controls for personal memory.

## Scope and disclosure

The project will assess reports, coordinate a fix, and communicate relevant mitigations. Do not test against systems or accounts you do not own or have explicit permission to assess.

A response timeline and supported-version policy will be published when the project has maintainers and operational capacity to meet them consistently.
