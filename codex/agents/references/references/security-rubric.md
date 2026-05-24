# Security Rubric

Focus on task-touched code and obvious adjacent security boundaries.

Check:
- secret/token/password hardcoding
- unsafe logging of sensitive values
- injection risks
- XSS or unsafe HTML rendering
- auth/authz bypass
- unsafe file path handling
- risky CORS/cookie/header config
- dependency audit when tooling is available

Do not print secrets. Mask any sensitive value if it must be referenced.

Security blocking issues cannot be waived through DISAGREED unless equivalent mitigation is documented.
