# Homelab Portfolio Demo Script

This script is a short presentation path for the private AI homelab. It is
designed for a recruiter screen, portfolio walkthrough, or interview follow-up.

## One-Minute Pitch

I built a private production-style homelab on an Apple Silicon Mac Mini using
Docker Desktop, Tailscale, Traefik, Authentik, AdGuard DNS, Wiki.js, Open WebUI,
n8n, monitoring, media services, and an on-demand IGA lab.

The stack has no public ingress. Access flows through Tailscale, private DNS,
Traefik, Authentik, and app-native OIDC where appropriate. I also built a local
AI operations assistant in Open WebUI that uses runbook RAG and custom read-only
OpenAPI tools to answer live infrastructure questions.

The newest part is an IGA simulation: midPoint models users, roles,
joiner/mover/leaver states, access review evidence, and a guarded connector
provisions dummy lab users/groups into Authentik.

## Demo Flow

### 1. Private Entry Point

Open:

```text
https://homepage.home.arpa:8443
```

Say:

```text
This is the private launchpad. It is reachable only from my tailnet through
home.arpa DNS. The page organizes infrastructure, applications, monitoring,
lab services, and on-demand power controls.
```

Screenshot proof:

```text
docs/assets/screenshots/homepage-dashboard.png
```

Proves:

- private service organization
- tailnet-only app access
- clean operational dashboard

### 2. Identity And SSO

Open:

```text
https://auth.home.arpa:8443
```

Say:

```text
Authentik is the central identity layer. Browser/admin apps use forward-auth,
while API-heavy or mobile-heavy apps use app-native OIDC to avoid breaking
clients and webhooks.
```

Screenshot proof:

```text
docs/assets/screenshots/authentik-apps.png
```

Proves:

- centralized identity
- application-level access policy
- SSO architecture

### 3. AI Operations Assistant

Open:

```text
https://openwebui.home.arpa:8443
```

Ask:

```text
Use docker_summary and tell me which containers need attention.
```

Say:

```text
Open WebUI is not just chat. It has runbook knowledge and an internal OpenAPI
tool server. The assistant can answer operational questions from live read-only
system state.
```

Screenshot proof:

```text
docs/assets/screenshots/openwebui-operator.png
docs/assets/screenshots/openwebui-tool-call.png
```

Proves:

- local AI/RAG integration
- custom internal tools
- read-only operations automation

### 4. Monitoring And Logs

Open:

```text
https://uptime.home.arpa:8443
https://dozzle.home.arpa:8443
```

Say:

```text
Uptime Kuma tracks service availability and expected Authentik redirect
behavior. Dozzle gives log visibility without exposing public ports.
```

Screenshot proof:

```text
docs/assets/screenshots/uptime-kuma-status.png
```

Proves:

- service monitoring
- operational visibility
- private observability stack

### 5. Documentation Layer

Open:

```text
https://wiki.home.arpa:8443
```

Say:

```text
Wiki.js is the operator-facing documentation layer. Runbooks are versioned in
Git and pulled into the wiki for easier browsing.
```

Screenshot proof:

```text
docs/assets/screenshots/wikijs-runbooks.png
```

Proves:

- runbook discipline
- Git-backed documentation
- operational maintainability

### 6. IGA Simulation

Open:

```text
https://midpoint.home.arpa:8443/midpoint
```

Say:

```text
midPoint runs as an on-demand IGA lab. It models HR-driven users, business
roles, joiner/mover/leaver states, separation-of-duties checks, and access
review evidence.
```

Screenshots:

```text
docs/assets/screenshots/midpoint-users.png
docs/assets/screenshots/midpoint-role-catalog.png
docs/assets/screenshots/midpoint-access-review.png
```

Proves:

- IGA concepts
- lifecycle state modeling
- role catalog
- access review evidence

### 7. IGA To Authentik Provisioning

Open:

```text
https://auth.home.arpa:8443
```

Show:

```text
Directory -> Users
Directory -> Groups
```

Say:

```text
The guarded connector turns desired IGA state into Authentik lab users and
lab-* groups. It has dry-run mode, audit logs, domain restrictions, group-prefix
restrictions, and no user deletion.
```

Screenshots:

```text
docs/assets/screenshots/authentik-lab-provisioning-users.png
docs/assets/screenshots/authentik-lab-provisioning-groups.png
```

Proves:

- IGA-to-enforcement bridge
- provisioning guardrails
- lab-only blast-radius control

### 8. IGA Report

Open:

```text
https://wiki.home.arpa:8443/runbooks/iga-lab-report
```

Say:

```text
The report generator creates executive-friendly evidence from desired state and
the latest provisioning audit. It summarizes users, groups, guardrails, SoD
results, and provisioning actions.
```

Screenshots:

```text
docs/assets/screenshots/wikijs-iga-provisioning-runbook.png
docs/assets/screenshots/wikijs-iga-report.png
```

Proves:

- auditability
- evidence generation
- readable governance reporting

## Screenshot Map

| Screenshot | What To Say |
|---|---|
| `homepage-dashboard.png` | Private service launchpad with organized app groups. |
| `authentik-apps.png` | Centralized SSO and app access policy. |
| `openwebui-operator.png` | Local AI operator with runbook knowledge. |
| `openwebui-tool-call.png` | Live read-only infrastructure tool call. |
| `uptime-kuma-status.png` | Monitoring and availability coverage. |
| `wikijs-runbooks.png` | Git-backed operational documentation. |
| `traefik-dashboard.png` | Reverse proxy routing and middleware. |
| `immich-oidc.png` | App-native OIDC for mobile/API-safe SSO. |
| `midpoint-users.png` | IGA demo users and lifecycle states. |
| `midpoint-role-catalog.png` | Business role catalog. |
| `midpoint-access-review.png` | Access review and SoD evidence. |
| `authentik-lab-provisioning-users.png` | Dummy users provisioned into Authentik. |
| `authentik-lab-provisioning-groups.png` | Lab-only Authentik groups. |
| `wikijs-iga-provisioning-runbook.png` | Finished IGA provisioning runbook. |
| `wikijs-iga-report.png` | Executive-friendly IGA report. |

## Strongest Talking Points

- Zero-public-ingress homelab with private DNS and tailnet access.
- Layered auth model: Authentik forward-auth, app-native OIDC, and local
  break-glass auth where needed.
- Local AI operations assistant with RAG and live read-only OpenAPI tools.
- On-demand IGA lab that demonstrates governance concepts without keeping a
  heavy Java stack always running.
- Guarded provisioning bridge with dry-run, audit logs, dummy-user restrictions,
  and `lab-*` blast-radius controls.
- Git-backed runbooks and screenshots that turn the build into presentable
  evidence, not just a pile of containers.

## Questions To Be Ready For

| Question | Short Answer |
|---|---|
| Why Tailscale instead of public ports? | It removes public ingress risk while keeping remote access simple. |
| Why both forward-auth and OIDC? | Forward-auth is good for browser apps; OIDC is better for mobile/API-heavy apps. |
| Why on-demand midPoint? | It is realistic for IGA demos but too heavy to keep running all the time on 16GB RAM. |
| What prevents the provisioner from touching real users? | It only accepts `@home.arpa` emails and `lab-*` groups, refuses privileged-looking names, and never deletes users. |
| What would you improve next? | Add approval workflow, backup execution on external SSD, and a production-grade connector pattern. |

