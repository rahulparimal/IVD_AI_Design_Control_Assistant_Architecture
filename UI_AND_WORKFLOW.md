# UI and Workflow Guide

## Primary screens

### 1. Project dashboard

Displays:

- Current lifecycle phase
- Open requirements and traceability gaps
- Pending reviews and approvals
- Verification and validation readiness
- Phase-gate status
- Recent controlled changes

### 2. Design-record workspace

Used for user needs, design inputs, risk controls, design outputs, verification, validation, and transfer records.

Key controls:

- Create and edit draft
- Generate AI-assisted candidate
- Critique requirement quality
- View supporting evidence
- Propose trace links
- Submit for review
- Compare versions
- View audit history

### 3. Traceability matrix

Shows the chain:

```text
User Need → Design Input → Risk Control → Design Output → Verification → Validation → Design Transfer
```

Filters should support:

- Record type
- Lifecycle phase
- Status
- Owner
- Risk classification
- Missing required links
- Changed since the last design review

### 4. Phase-gate workspace

Each gate contains:

- Required deliverables
- Completion rules
- Open critical gaps
- Required approvers
- Design-review minutes
- Exceptions and rationale
- Final gate decision

AI can summarize readiness and identify candidate gaps. Only authorized users can record a gate decision.

### 5. Change-impact workspace

The user selects a proposed change and the system identifies candidate impacts across:

- User needs
- Design inputs
- Risk analysis and controls
- Design outputs
- Verification and validation
- Labeling and regulatory documentation
- Manufacturing and design transfer

All AI-identified impacts remain proposed until assessed and accepted by qualified personnel.

## Standard usage sequence

1. Select the IVD project.
2. Open the current lifecycle phase.
3. Select a controlled upstream source or record.
4. Generate a candidate design record or critique an existing one.
5. Review supporting evidence and AI-identified gaps.
6. Accept, edit, or reject proposed content.
7. Establish required trace links.
8. Submit the record for independent review.
9. Complete approval through an authorized approver.
10. Use the phase-gate dashboard to assess readiness.

## UI wireframe

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ IVD Design Control Assistant              Project: Aurora Flu A+B           │
├──────────────────┬───────────────────────────────────┬──────────────────────┤
│ Lifecycle        │ Design Input DI-014               │ Evidence & Trace     │
│                  │                                   │                      │
│ ✓ User Needs     │ The system shall detect...        │ UN-003 Intended use  │
│ ● Design Inputs  │                                   │ RM-017 Risk control  │
│ ○ Risk Controls  │ [Critique] [Generate criteria]    │ VER-042 Verification │
│ ○ Design Outputs │                                   │                      │
│ ○ Verification   │ Quality checks                    │ Gaps                 │
│ ○ Validation     │ • Atomic: Pass                    │ • Validation link    │
│ ○ Transfer       │ • Testable: Pass                  │   missing            │
│                  │ • Ambiguous terms: None           │                      │
├──────────────────┴───────────────────────────────────┴──────────────────────┤
│ [Save draft] [Submit for review]                         Audit history       │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Review rules

- Generated records always begin in draft status.
- AI-generated text is visually identified.
- Evidence references are shown beside the associated claim.
- Missing or conflicting evidence is shown as a blocking or advisory gap.
- Reviewers can request changes with record-specific comments.
- Approvers cannot approve records they authored.
- Approved records become immutable; changes create a new version.

## Accessibility and usability

- Keyboard-accessible navigation and controls
- WCAG-aligned contrast and focus states
- Clear distinction between draft, reviewed, and approved states
- No reliance on color alone for status
- Plain-language explanations for AI confidence and evidence gaps
- Consistent record identifiers and breadcrumb navigation
