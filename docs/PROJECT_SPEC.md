# Project 03 — Agentic Migration Factory

```yaml
project_id: P03
project_name: Agentic Migration Factory
suggested_repo: agentic-migration-factory
primary_language: Python 3.12
iac: Bicep
cloud: Microsoft Azure
target_duration: 4 weeks
estimated_effort: 35-45 hours
estimated_cloud_cost: USD 5-20 for short-lived model/API usage
primary_roles:
  - Domain AI Azure Architect
  - Agentic AI Solutions Architect
  - AI Enterprise Solutions Architect
certification_alignment:
  AI-103: agents, structured extraction, tools, orchestration, evaluation
  SC-500: governance, security classification, network and data controls
deployment_requires_human_approval: true
```

## 1. Mission for the implementing agent

Build a portfolio rationalization and migration planning system that ingests a synthetic application/server inventory and produces evidence-backed recommendations for the 7Rs, dependency groups, migration waves, target Azure patterns, risks, TCO assumptions and cutover readiness.

The system must encode migration expertise without pretending that an LLM can authorize a production move. All final dispositions, wave assignments and destructive steps require human approval.

This project should be the most domain-differentiated artifact in the portfolio and should explicitly leverage Mario's experience with large-scale server migration while using only synthetic data.

## 2. Business problem

Migration programs spend substantial effort cleaning inventories, classifying applications, discovering dependencies, reconciling contradictory stakeholder answers and generating wave plans. Decisions often lack traceability and must be revisited as evidence changes.

The product creates a repeatable decision workflow:

```text
inventory -> quality checks -> dependency graph -> 7R candidates -> target patterns
          -> risks/TCO -> wave constraints -> human decision -> migration plan
```

## 3. Target users

- Migration and modernization architect.
- Cloud solutions architect.
- Program manager.
- Application owner.
- Security/network/database workstream leads.
- FinOps analyst.

## 4. Success metrics

- 100% of application records receive a data-quality state.
- At least 90% of seeded dependency conflicts are detected.
- 7R recommendation agreement >= 85% against the approved golden decisions.
- Every recommendation includes evidence, assumptions and confidence.
- No wave violates a declared hard dependency or blackout constraint.
- No application marked “retire” or “retain” becomes final without approval.
- TCO output clearly separates observed inputs, assumptions and calculated values.
- Generated target IaC examples pass Bicep build/lint for supported patterns.
- Local full run completes on a 50-application fixture without paid services.

## 5. Scope

### MVP in scope

- Synthetic portfolio of 40-60 applications and 120-200 servers.
- CSV/JSON import inspired by RVTools, CMDB and stakeholder questionnaire exports.
- Data quality and conflict detection.
- Dependency graph and application grouping.
- Candidate 7R recommendations: rehost, replatform, refactor, repurchase, relocate, retain, retire.
- Candidate Azure target patterns for a bounded catalog.
- Wave planning constrained by dependencies, criticality, blackout dates, capacity and shared services.
- Risk register and discovery questions.
- TCO calculator with explicit formulas and editable assumptions.
- Approval workflow and decision history.
- Generation of non-production Bicep skeletons for approved target patterns.
- JSON, CSV and Markdown outputs.
- API/CLI, tests, evaluation and dev deployment.

### Out of scope

- Executing migrations.
- Connecting to production vCenter, CMDB or customer tenant.
- Azure Migrate project creation.
- Database schema conversion.
- Guaranteed Azure pricing quote.
- License compliance decisions.
- Autonomous decommissioning.

### Stretch goals

- RVTools `.xlsx` parser using synthetic export.
- Read-only Azure Migrate assessment import.
- What-if comparison between IaaS and a replatform target.
- Diagram of dependency groups/waves.
- Cutover runbook generator.
- English/Spanish executive report.

## 6. Synthetic scenario

Create a fictional company with:

- Three business units.
- Production, non-production and shared-services environments.
- Finance, healthcare-like and manufacturing-like applications without real regulated data.
- Windows and Linux workloads.
- SQL Server, PostgreSQL and file dependencies.
- At least two unsupported operating systems.
- One licensing constraint.
- One latency-sensitive dependency.
- One application with incomplete ownership.
- One “retire” candidate with unresolved usage evidence.
- Blackout periods and business deadlines.
- Conflicting CMDB and stakeholder records.

Every source must declare provenance and synthetic status.

## 7. Input model

### `Application`

- `application_id`
- `name`
- `business_unit`
- `owner_role`
- `environment`
- `criticality`
- `data_classification`
- `rto_hours`
- `rpo_hours`
- `maintenance_window`
- `business_deadline`
- `modernization_appetite`
- `compliance_tags`
- `source_confidence`

### `Server`

- `server_id`
- `application_id`
- `os_family/version`
- `cpu/memory/storage`
- `utilization_percentiles`
- `database_role`
- `network_zone`
- `backup_state`
- `support_state`
- `current_location`

### `Dependency`

- `source_id`
- `target_id`
- `type`
- `direction`
- `latency_sensitivity`
- `port_protocol_alias`
- `confidence`
- `discovery_source`

### `Constraint`

- `constraint_id`
- `scope`
- `type`
- `hard_or_soft`
- `start/end`
- `description`
- `source`

## 8. Decision output model

### `MigrationDecisionCandidate`

```json
{
  "application_id": "APP-014",
  "recommended_r": "replatform",
  "alternate_r": "rehost",
  "target_pattern": "app-service-plus-azure-sql",
  "confidence": 0.82,
  "evidence": ["modernization_appetite=medium", "os_support=valid", "database_compatibility=partial"],
  "assumptions": ["application can externalize session state"],
  "blocking_questions": ["Can vendor certify Azure SQL compatibility?"],
  "risks": ["vendor_support"],
  "approval_state": "pending",
  "approved_by": null
}
```

### Outputs

- `portfolio-quality-report.md`
- `application-decisions.csv`
- `dependency-groups.json`
- `migration-waves.csv`
- `risk-register.csv`
- `target-architecture-options.md`
- `tco-assumptions.json`
- `tco-summary.md`
- `discovery-questions.md`
- `decision-log.json`
- `run-manifest.json`
- Bicep pattern skeletons for approved sample applications.

## 9. Agent design

### Orchestrator

- Creates bounded plan and run context.
- Invokes deterministic validators before agents.
- Prevents final wave generation while blocking data issues remain.
- Routes conflicts and low-confidence decisions to humans.
- Records decision provenance and versions.

### Inventory/data-quality agent

- Normalizes records.
- Identifies missing ownership, invalid capacity and contradictory sources.
- Proposes questions, not fabricated values.

### Dependency agent

- Builds graph from declared records.
- Detects cycles, single points of dependency and uncertain edges.
- Suggests application groups.
- Cannot invent network dependencies.

### Rationalization agent

- Produces candidate 7R and alternate.
- Uses a versioned decision rubric.
- Must explain evidence, assumptions and confidence.
- Retire/retain/repurchase decisions always require approval.

### Target architecture agent

- Selects only from an approved target-pattern catalog.
- Produces trade-offs rather than unrestricted designs.
- Marks missing security/network/database prerequisites.

### Wave planning agent

- Consumes approved or provisional decisions plus hard constraints.
- Uses deterministic graph/constraint solver for validity.
- Agent may explain/optimize soft priorities but cannot override hard rules.

### TCO agent

- Uses deterministic calculator.
- May explain cost drivers and sensitivity.
- Must never claim a quote or guaranteed saving.

## 10. Approved target-pattern catalog

Minimum patterns:

- Azure VM rehost.
- Availability-zone VM set.
- Azure VMware Solution placeholder/decision option, not deployed.
- App Service + managed database.
- Container Apps + managed database.
- Azure SQL Database/Managed Instance option.
- Retain on-premises with Azure Arc management option.
- Retire/decommission plan placeholder.

Each pattern includes:

- Eligibility criteria.
- Exclusions.
- Security/network prerequisites.
- Availability model.
- Operational model.
- Cost inputs.
- Bicep skeleton where deployable at low cost.
- Required human specialists.

## 11. 7R rubric

Implement a versioned decision table. Inputs include:

- Business value and deadline.
- Technical fitness/support status.
- Dependency complexity.
- Data and compliance constraints.
- Vendor/licensing limitations.
- Modernization appetite.
- Availability and performance needs.
- Current utilization.
- Retirement evidence.

The deterministic rubric produces candidate scores; the agent explains trade-offs. The LLM must not be the sole decision engine.

## 12. Wave-planning rules

Hard constraints:

- Dependencies with `must_move_together` remain in the same wave.
- Foundation/shared services precede dependents unless grouped.
- Blackout windows are honored.
- Wave capacity limit is not exceeded.
- Production cannot precede its approved non-production validation unless an exception is approved.
- Unresolved critical blockers prevent scheduling.

Soft objectives:

- Group by business unit or target platform.
- Balance risk and effort.
- Prefer early low-risk learning waves.
- Meet business deadlines.

Produce a validation report proving hard constraints are satisfied.

## 13. TCO model

Required categories:

- Compute, storage and estimated data transfer.
- Licensing assumptions.
- Backup, monitoring and security services.
- Migration one-time effort assumption.
- Support/operations assumption.
- Retirement avoidance.
- Three-year horizon with configurable growth/discount factors.

Rules:

- Formulas live in code and are unit tested.
- Pricing data source/date/region are recorded.
- If live Azure pricing cannot be retrieved, use clearly labeled sample rates.
- Sensitivity table for utilization, growth and commitment discount.
- No unsupported savings percentage.

## 14. Tools

- `load_portfolio(path)` — allowlisted input directory.
- `validate_portfolio(portfolio)`.
- `build_dependency_graph(records)`.
- `score_7r(application, rubric_version)`.
- `select_target_pattern(application, catalog_version)`.
- `validate_wave_plan(plan, constraints)`.
- `calculate_tco(inputs, rates_version)`.
- `render_bicep_skeleton(pattern_id, parameters)`.
- `request_decision_approval(decision_ids)`.

No arbitrary Azure CLI, PowerShell, shell, vCenter or network tool is permitted in MVP.

## 15. API and CLI

Endpoints:

- `POST /v1/portfolios`
- `POST /v1/portfolios/{id}/analyze`
- `GET /v1/portfolios/{id}/quality`
- `GET /v1/portfolios/{id}/decisions`
- `POST /v1/portfolios/{id}/approvals`
- `POST /v1/portfolios/{id}/waves/generate`
- `GET /v1/portfolios/{id}/reports/{type}`
- `GET /healthz`

CLI:

```text
migration validate --input sample-data/portfolio-medium
migration analyze --input <path> --provider mock
migration approve --portfolio <id> --decision APP-014 --choice replatform
migration waves --portfolio <id> --max-apps 8
migration tco --portfolio <id> --horizon-years 3
migration export --portfolio <id> --output artifacts/<id>
migration eval --dataset evals/datasets/golden.jsonl
```

## 16. Security and governance

- Synthetic data only.
- Treat imported inventory fields as untrusted.
- Prevent spreadsheet formula injection in CSV exports.
- Sanitize filenames and paths.
- No direct resource actions.
- Separate recommender identity from approver identity.
- Immutable append-only decision history in the application model.
- Redact resource names in public artifacts.
- Validate generated Bicep and prohibit arbitrary embedded scripts.
- Tool budgets and timeouts.
- All decisions record model/rubric/catalog versions.

Threat cases:

1. Resource description contains prompt injection.
2. Malicious CSV formula.
3. Agent recommends retire without usage evidence.
4. Wave plan ignores hidden dependency.
5. Model invents cost rate.
6. Generated IaC creates a public endpoint.
7. User requests deployment despite advisory-only scope.

## 17. Evaluation

Golden portfolio contains approved labels for:

- Data quality issues.
- Dependency conflicts/groups.
- Candidate and alternate 7R.
- Mandatory questions.
- Target pattern eligibility.
- Wave constraints.
- TCO formula outputs.

Metrics:

- Data-quality recall/precision.
- 7R top-1 and top-2 agreement.
- Evidence coverage.
- Unapproved irreversible decision count.
- Dependency/wave constraint violation count.
- Cost-calculation error.
- Bicep build success.
- Unsupported claim rate.
- Latency/tokens/cost.

Blocking thresholds:

- Hard wave constraint violations = 0.
- Unapproved retire/retain decisions = 0.
- TCO deterministic tests = 100%.
- Generated Bicep build success = 100% for supported patterns.
- Evidence coverage >= 95%.

## 18. Architecture and deployment

Local architecture:

```text
CLI/API -> portfolio store -> deterministic engines -> specialized agents
        -> approval/decision log -> report/IaC generators -> eval artifacts
```

Azure dev target:

- App Service or Container Apps.
- Microsoft Foundry project/model.
- Storage account for synthetic portfolios/artifacts.
- Optional Cosmos DB or low-cost data store only if justified; local SQLite is sufficient for MVP.
- Key Vault.
- Application Insights.
- Managed Identity.

Do not deploy target workload patterns automatically. Generated Bicep is validated offline and kept under `artifacts/` or tests.

## 19. Repository-specific structure

```text
app/portfolio/
app/dependencies/
app/rationalization/
app/target_patterns/
app/waves/
app/tco/
app/approvals/
rubrics/7r-v1.yaml
target-patterns/catalog-v1/
sample-data/portfolio-small/
sample-data/portfolio-medium/
evals/datasets/golden-portfolio.json
```

## 20. Implementation backlog

### Milestone 0 — Domain contract

- Define synthetic scenario and schemas.
- Encode 7R rubric, risk taxonomy and approval matrix.
- Define target patterns and TCO formulas.
- Architecture/threat model/ADRs.

### Milestone 1 — Local deterministic engines

- Loaders and validators.
- Dependency graph.
- 7R scorer.
- Wave constraint validator.
- TCO calculator.
- Report output.

### Milestone 2 — Agents and approvals

- Provider abstraction and mock.
- Specialized agents and orchestrator.
- Decision log and approval API.
- Failure, conflict and low-confidence handling.

### Milestone 3 — Evaluation and IaC patterns

- Golden labels.
- Full metrics.
- Bicep skeleton generator and security checks.
- Adversarial fixtures.

### Milestone 4 — Azure dev

- Bicep/OIDC.
- Deploy API/model adapter.
- Smoke test synthetic portfolio.
- Capture trace/cost and destroy after approval.

### Milestone 5 — Portfolio package

- Dependency/wave diagrams.
- Before/after process metric.
- Five-minute demo.
- Interview brief emphasizing 5,000+ migration experience without exposing customer data.

## 21. CI/CD

- Ruff, mypy, pytest.
- Schema/rubric/catalog validation.
- Dependency and wave invariant tests.
- TCO formula tests.
- Generated Bicep build/lint.
- Mocked golden evaluation subset.
- Injection/formula/secret scans.
- Manual Azure deployment with OIDC and approval.

## 22. Definition of done

- Synthetic portfolio and provenance complete.
- Deterministic engines and agents produce traceable decisions.
- All hard constraints pass.
- Human approvals cannot be bypassed.
- TCO assumptions and calculations are reproducible.
- Bicep pattern skeletons validate.
- Evaluation thresholds pass.
- Local run and Azure dev adapter are documented and tested.
- Demo, diagrams, ADRs, threat model, cost and interview brief are complete.

## 23. Required decisions before repository creation/deployment

- GitHub owner, visibility and license.
- Azure subscription/tenant/region and budget.
- Whether to use a synthetic RVTools-like spreadsheet in MVP.
- Which target patterns should be emphasized: Azure VM, App Service, Container Apps, Azure SQL, AVS decision analysis.
- Whether the demo/report should be English, Spanish or bilingual.
- Whether any sanitized personal migration template may be used; default is no until explicitly approved.

