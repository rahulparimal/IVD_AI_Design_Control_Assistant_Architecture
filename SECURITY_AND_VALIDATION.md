# Security and Validation Design

## Security objectives

- Preserve confidentiality of design records and source evidence.
- Prevent cross-project and cross-tenant data exposure.
- Ensure integrity and version traceability of controlled records.
- Enforce segregation of duties for authoring, review, and approval.
- Prevent AI output from bypassing design-control workflows.
- Maintain attributable, contemporaneous, and reviewable audit records.

## Access-control model

Authorization is enforced at two levels:

1. **Application role** — Author, Reviewer, Approver, Auditor, or Administrator.
2. **Project membership** — the user must be assigned to the specific design project.

The API, not the frontend, is the authorization decision point. Every read, write, generation, review, approval, and export request is checked server-side.

## Segregation of duties

- Authors cannot approve their own controlled records.
- Review and approval identities are recorded independently.
- Administrators cannot silently alter approved design content.
- Approved records are changed only through versioned change control.
- Emergency exceptions require reason, authorization, and audit evidence.

## Audit-event model

Each audit event records:

- Event identifier
- UTC timestamp
- Actor and effective role
- Action
- Resource type and identifier
- Project identifier
- Correlation identifier
- Before and after content hashes
- Workflow transition
- Reason or comment when required
- Model and prompt version for AI-assisted changes
- Integrity signature

Audit events are append-only and retained according to the applicable quality-system policy.

## AI-specific risk controls

| Risk | Control |
|---|---|
| Hallucinated requirement | Evidence-only prompt, citation validation, reviewer approval |
| Unsupported trace link | Link recommendation marked as proposed until accepted |
| Prompt injection in source evidence | Source content isolated as untrusted data and never treated as instructions |
| Stale document used | Effective-version and approval-state filters |
| Cross-project leakage | Project and ACL filters applied before retrieval |
| Over-reliance on AI confidence | Confidence displayed as advisory only; human decision remains mandatory |
| Uncontrolled model change | Model deployment and prompt version controlled and regression-tested |
| Sensitive telemetry | No design-record content in standard logs; structured identifiers only |

## Validation strategy

The platform should be validated using a risk-based computerized-system validation approach aligned to the organization's quality system.

### Required specifications

- User Requirements Specification
- Functional Requirements Specification
- Software/System Design Specification
- Configuration Specification
- Data and interface specification
- Security and access-control specification
- Traceability matrix
- Risk assessment
- Test protocols and reports
- Release and validation summary

### Qualification activities

#### Installation Qualification

- Cloud resources match approved configuration.
- Container image digests and dependencies are recorded.
- Network, identity, certificates, and secrets are configured correctly.
- Monitoring, backup, and retention controls are active.

#### Operational Qualification

- Role-based access and segregation of duties
- Record creation, versioning, review, and approval
- Trace-link enforcement
- Audit-event generation and integrity
- Evidence retrieval security filters
- AI structured-output validation
- Failure handling and recovery

#### Performance Qualification

- Representative IVD design-control workflows
- Production-like user roles and data volumes
- Phase-gate preparation and traceability reporting
- Change-impact analysis
- Controlled export and archival

## AI evaluation framework

Maintain an approved golden dataset covering:

- User needs
- Design inputs
- Risk controls
- Design outputs
- Verification criteria
- Validation criteria
- Trace links
- Conflicting and incomplete evidence

Measure:

- Requirement atomicity
- Testability
- Evidence entailment
- Citation correctness
- Trace-link precision and recall
- Unsupported-claim rate
- Critical-error rate
- Reviewer acceptance rate

A model or prompt version cannot be promoted when critical-error thresholds are exceeded.

## Testing layers

1. Unit tests for validation, authorization, mapping, and output parsing.
2. Contract tests between frontend, API, and AI service.
3. Integration tests with Search, Blob Storage, database, and OpenAI.
4. Security tests for access-control bypass and data leakage.
5. Model regression tests against the golden dataset.
6. Load and resilience tests.
7. Backup, restore, and disaster-recovery tests.
8. Formal qualification tests in the validation environment.

## Release evidence

Each production release should retain:

- Source commit and pull request
- Build and test results
- SBOM and vulnerability scan results
- Container image digest and signature
- Infrastructure deployment record
- Configuration baseline
- AI model and prompt versions
- Regression-evaluation results
- Release approval
- Rollback plan
