# Configuration and Deployment Guide

## Local demonstration

1. Clone the application repository.
2. Copy `.env.example` to `.env`.
3. Set `MOCK_MODE=true` for local operation without Azure dependencies.
4. Set `AUDIT_HMAC_KEY` to a random value of at least 32 characters.
5. Run `docker compose up --build`.
6. Open the web application at `http://localhost:5173`.
7. Verify the API health endpoint at `http://localhost:3000/health`.
8. Verify the AI service health endpoint at `http://localhost:8000/health`.

## Required production configuration

### Microsoft Entra ID

Create separate app registrations or managed identities for:

- Web/API authentication
- Node.js API workload identity
- Python AI-service workload identity

Recommended application roles:

- `DesignAuthor`
- `DesignReviewer`
- `DesignApprover`
- `QualityAuditor`
- `SystemAdministrator`

Map Entra groups to application roles and enforce project-level membership in the API.

### Azure OpenAI

Configure approved chat and embedding deployments. Store only deployment names in application configuration; do not hard-code API keys. Use managed identity and private endpoints.

Required settings:

```text
AZURE_OPENAI_ENDPOINT=
AZURE_OPENAI_CHAT_DEPLOYMENT=
AZURE_OPENAI_EMBEDDING_DEPLOYMENT=
AZURE_OPENAI_API_VERSION=
```

### Azure AI Search

The search index should contain at least:

- `id`
- `projectId`
- `documentId`
- `documentVersion`
- `approvalStatus`
- `effectiveFrom`
- `effectiveTo`
- `recordType`
- `section`
- `page`
- `acl`
- `content`
- `contentVector`
- `contentHash`

All queries must apply project, approval-state, effective-version, and ACL filters before retrieval.

### Azure Blob Storage

Use separate containers for:

- `controlled-source`
- `verification-evidence`
- `validation-evidence`
- `generated-exports`
- `quarantine`

Enable versioning, soft delete, malware-scanning workflow, encryption, private endpoints, and retention policies appropriate to the quality system.

### Database

Use Azure SQL Database or PostgreSQL for structured records. Apply schema migration through CI/CD and prohibit direct production schema edits.

Core tables:

- `projects`
- `project_members`
- `design_records`
- `design_record_versions`
- `trace_links`
- `reviews`
- `approvals`
- `phase_gates`
- `change_requests`
- `audit_events`

### Key Vault

Store:

- Database credentials when managed identity cannot be used
- Audit-signing secrets or keys
- Third-party integration secrets
- Certificate material

Applications should access Key Vault through managed identity.

## Network controls

- Disable public network access for OpenAI, Search, Storage, Key Vault, and database services.
- Use private endpoints and private DNS zones.
- Integrate application hosting with the virtual network.
- Restrict outbound traffic through approved routes and firewall rules.
- Separate development, validation, and production subscriptions or resource groups.

## CI/CD promotion model

1. Build immutable container images.
2. Generate an SBOM.
3. Run dependency, container, secret, and infrastructure scans.
4. Execute unit and integration tests.
5. Sign images and publish to Azure Container Registry.
6. Deploy the same image digest to development.
7. Run regression and security tests.
8. Promote to validation and execute formal qualification.
9. Require release approval before production deployment.

## Production readiness checklist

- Entra authentication and role enforcement validated
- Private-network connectivity validated
- Managed identities and least-privilege roles validated
- Search security filters tested for cross-project leakage
- Audit-event integrity and retention tested
- Backup and restore tested
- Model golden-set evaluation approved
- Threat model and penetration test completed
- Computerized-system validation completed
- Operating procedures and support model approved
