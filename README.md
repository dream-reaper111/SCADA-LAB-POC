# SCADA-LAB-POC
LEGACY SCADA WEB LAB
MITRE ATT&CK Technical Service Assessment
2016-Era Web Practices · Industrial Dashboard Simulation
MITRE ATT&CK and ATT&CK are registered trademarks of The MITRE Corporation. This independent lab is not a MITRE-endorsed project.

# THIS WAS A POC THAT IVE ACTUALLY PERFORMED 



https://github.com/user-attachments/assets/59a0c44c-9cc2-46c9-be16-80d188e408ca




Overview
Legacy SCADA Web Lab is a proposed security research environment inspired by insecure web-development and deployment patterns found in older applications, including the 2016 era. These patterns are not unique to that period.
The concept pairs a simulated industrial monitoring dashboard with intentionally misplaced files, embedded credentials, weak access controls, and excessive server disclosure. A separate hardened configuration would show how each exposure can be prevented.
Shodan provides an optional exposure-review component for infrastructure owned by the operator or explicitly included in an authorized assessment. Local exercises use synthetic indexing records instead.
Current status: this README describes a project design. No application, Shodan integration, deployment configuration, or validation results are supplied with it. Examples below are planned lab scenarios, not confirmed vulnerabilities in a real system.
Scope
All industrial values, users, credentials, and operational events should be fictional. The dashboard represents an industrial interface; it does not communicate with PLCs, RTUs, safety systems, or production equipment.
The intentionally vulnerable configuration belongs on an isolated local network. Any public exposure exercise should use a separate sanitized demonstration service containing no vulnerable functionality, secrets, or operational data. Public indexing is optional and not required to complete the lab.
Proposed dashboard
The interface would resemble a small legacy monitoring portal, with:
- Simulated tank levels, pressure, temperature, and pump status.
- Timestamped fictional telemetry and alarm history.
- Operator and administrator views backed by dummy accounts.
- An application database containing only generated records.
- A visible distinction between the vulnerable and hardened editions.
Controls should affect simulated application state only. No industrial protocol connectivity is required.
Legacy practices under review
Planned scenario	Intended lesson	Hardened comparison
Database credentials embedded in application source	Source disclosure can also disclose secrets.	Keep secrets outside source control and the web root; restrict access.
SQLite database placed under a publicly served directory	A database file can be exposed independently of application authentication.	Store database files outside served paths and enforce filesystem permissions.
Configuration files and backups inside the web root	Deployment artifacts can reveal settings or historical data.	Exclude artifacts from deployments and deny access to sensitive file types.
Directory indexing enabled	Browsable directories reveal files that were never intended as pages.	Disable directory listing and explicitly define public content.
Verbose application errors	Error pages can disclose paths, queries, and implementation details.	Return generic errors and retain detailed diagnostics in protected logs.
Default accounts and missing route authorization	A login page alone does not protect every application endpoint.	Require unique credentials and server-side authorization on protected routes.
Excessive service and version disclosure	Public metadata helps identify the software stack.	Minimize unnecessary banners and maintain supported software.
Overprivileged application database access	A compromised application can have more access than it needs.	Apply least privilege and separate application roles.


Hardcoded credentials, SQL statements embedded in code, and database files stored in served directories are distinct issues. A SQL query in source is not inherently a vulnerability; unsafe query construction and exposed secrets require separate analysis. Server-side source is also not normally delivered to browsers unless configuration or another disclosure flaw exposes it.
Shodan exposure review
The optional Shodan workflow is for reviewing a known, authorized asset rather than locating unrelated industrial targets.
1. Record ownership, written authorization, and the exact approved public asset scope.
2. Review available indexing records for those assets through an authorized Shodan account or interface.
3. Document observation timestamps, reported ports, HTTP metadata, and visible service banners.
4. Compare indexed observations with the operator's current configuration and inventory.
5. Record unnecessary exposure and the relevant remediation, then verify changes within scope.
Shodan records may be stale or incomplete. A banner is evidence of an observed service, not proof of vulnerability, current reachability, source disclosure, or database access. Shodan does not index private loopback addresses or isolated local services, and indexing of a public service is not guaranteed.
For the local exercise, use clearly labeled synthetic records. Validate exposed files only against the isolated lab application; indexed third-party systems are outside this project's scope.
Proposed repository layout
The paths below describe a future implementation; these files are not included with this README.
Path	Planned purpose
app/	Simulated dashboard and application routes.
fixtures/	Fictional telemetry, dummy users, and synthetic exposure records.
lab/vulnerable/	Isolated configurations illustrating the documented mistakes.
lab/hardened/	Corrected configuration for comparison.
docs/architecture.md	Application boundaries, data flow, and isolation assumptions.
docs/exposure-review.md	Authorized asset inventory and Shodan observation notes.
docs/findings.md	Evidence, impact, remediation, and verification results.
tests/	Checks for isolation, protected routes, and sensitive-file handling.


Development roadmap
Phase 1 — Build the simulation
Create the fictional dashboard, generated telemetry, and dummy database. Define the application boundary and keep all behavior independent of real industrial devices.
Phase 2 — Model the historical mistakes
Implement a small, documented set of insecure configurations in the isolated edition. Make each scenario reproducible with fixture data and identify the specific condition that enables disclosure.
Phase 3 — Add exposure evidence
Provide synthetic Shodan-style observations for local study. Optionally document an authorized public sanitized service separately, with timestamps and a precise inventory mapping.
Phase 4 — Build the hardened edition
Move databases and secrets outside public paths, restrict file serving, remove default accounts, enforce authorization, and document supported dependencies. Explain why each change addresses its associated finding.
Phase 5 — Validate and document
Check that the lab has no real control-system connections, that vulnerable services remain isolated, and that the hardened edition blocks the documented disclosures. Publish only sanitized evidence and distinguish observed results from design expectations.
Findings format
Field	What to record
Asset and scope	Lab identifier or explicitly authorized asset.
Observation	What was directly seen and when.
Evidence source	Local response, configuration review, or indexed metadata.
Exposure condition	The precise file-placement, authorization, or server-setting mistake.
Demonstrated impact	What happened with fictional lab data; avoid unsupported claims.
Remediation	The configuration or application change that addresses the cause.
Verification	Expected result, actual result, and test date.


Limitations
This project models selected web-security mistakes. It does not reproduce a vendor SCADA product, establish a real-world compromise, or demonstrate control over industrial equipment. Availability of a web interface does not establish access to the underlying control network.
No implementation has been executed or tested as part of this README. Setup instructions and dependency versions should be added only after a working implementation exists.
Attribution and license
Document the provenance and licenses of any code, screenshots, or fixtures added later. This README assigns no license to a future implementation. Do not include third-party database contents, credentials, or copied industrial configuration files.
<div align="center">

Preserve the lesson. Simulate the process. Document the fix.
</div>


MITRE ATT&CK mapping methodology
This is a retrospective mapping of a 2016-inspired design to current ATT&CK terminology, reviewed October 17, 2026. It does not claim these technique identifiers existed in 2016. ATT&CK describes behavior; a configuration weakness alone is not an observed technique.
Evidence status for every scenario below: proposed / not executed. Promote an entry to demonstrated only after recording the relevant lab evidence. An indexed banner, a successful HTTP response, a login, and host execution are different observations.
Reconnaissance: where Shodan belongs
TA0043 — Reconnaissance → T1596.005 — Search Open Technical Databases: Scan Databases. Reviewing existing Shodan indexing records for an authorized asset fits this sub-technique. The analyst consults a third-party database rather than sending probes to the target through the search itself.
Shodan's underlying collection activity is distinct from an analyst searching its stored results. New direct probes of an authorized service should be assessed separately against T1595 — Active Scanning, with a sub-technique selected only when the actual behavior supports it. Reviewing indexed port metadata does not establish internal network discovery.
Record the authorized asset, service endpoint, observation timestamp, reported banner, analyst review time, and comparison with current inventory. Local web-server logs generally cannot show that someone searched a third-party index. Logs may show the indexer's earlier collection or subsequent direct requests; do not conflate these events.
Behavior-to-technique assessment
Tactic	Candidate technique	Required behavior and mapping boundary	Useful evidence
Reconnaissance	T1596.005 — Scan Databases	Consult existing Shodan records for the scoped service. This does not establish compromise.	Sanitized indexed record, scope record, collection and review timestamps.
Reconnaissance	T1595 — Active Scanning	Directly probe scoped infrastructure to gather targeting information. Map separately from index searches.	Authorized test record and corresponding network or HTTP logs.
Initial Access	T1190 — Exploit Public-Facing Application	Exploit an application weakness to obtain initial access. A visible login page, old banner, or publicly downloadable file alone is insufficient to establish this technique.	Application evidence connecting a demonstrated flaw to the access obtained.
Initial Access; other tactics only if demonstrated	T1078.001 — Default Accounts	Successfully use a built-in/default account in the simulation. A custom weak password is not automatically a default account.	Authentication event and account provenance; omit passwords.
Credential Access	T1552.001 — Credentials In Files	Search accessible configuration/source files for embedded dummy credentials. For pre-access HTTP disclosure, document the delivery context and assess fit rather than automatically treating it as local credential discovery.	Sanitized fixture, access context, and file review event.
Discovery	T1083 — File and Directory Discovery	Enumerate host files/directories after access in an appropriate context. Public web-path enumeration is not automatically host filesystem discovery.	Host audit records or controlled filesystem observations.
Collection	T1005 — Data from Local System	Collect fictional data from the accessed system's local files. An internet-accessible database download needs its own context assessment.	Local file access record and sanitized artifact identifier.
Collection	T1213.006 — Databases	Collect records from a database repository. The existence of a database or its credentials does not show collection.	Scoped database audit events and fictional record counts.


These mappings are assessment candidates, not an ordered intrusion recipe. Default-account access and application exploitation may represent alternative scenarios. Do not assume that one observation proves the next stage occurred.
Whole-lifecycle coverage and gaps
The following table accounts for the Enterprise tactics used for this assessment. It is not a claim to cover every ATT&CK technique. Recheck the current matrix when revising the project.
Tactic	Coverage in this project design
Reconnaissance	Proposed: indexed service review and separately authorized direct exposure checks.
Resource Development	No adversary resource acquisition or preparation scenario defined.
Initial Access	Conditional: demonstrate a qualifying application flaw or default-account use with fictional accounts.
Execution	Not demonstrated; HTTP access does not prove code execution.
Persistence	No scenario defined; an accessible service is not itself persistence.
Privilege Escalation	Not demonstrated; an administrator-labeled page does not establish elevation.
Defense Evasion	No scenario defined; missing logging is a defensive gap, not proof of evasion behavior.
Credential Access	Proposed: analysis of dummy credentials in lab files, subject to context.
Discovery	Conditional: host-level enumeration requires explicit evidence beyond public banners.
Lateral Movement	No scenario defined; no connection to real OT assets.
Collection	Proposed: fictional local files or database records, with source-specific evidence.
Command and Control	No scenario defined; ordinary dashboard HTTP traffic does not establish C2.
Exfiltration	Not demonstrated; collection alone does not establish an exfiltration channel or technique.
Impact	No process-impact scenario defined; disclosure does not prove physical consequences.


Enterprise and ICS boundaries
Use Enterprise ATT&CK for the web host, application accounts, configuration files, and database behavior described here. Use ATT&CK for ICS only when a scenario establishes a specific industrial behavior and its relevant asset context.
An industrial-looking page is not evidence of an HMI connection, PLC access, process manipulation, impaired safety, or loss of control. This project has no real controller connectivity. If a later simulator adds process behavior, document that simulation and assess it against the ICS matrix separately. Do not transfer Enterprise technique IDs into an ICS coverage claim.
Service observability and defensive validation
Service or boundary	Evidence to retain	Defensive validation goal
Public inventory / indexing	Authorized assets, indexed timestamps, sanitized banners.	Expose only intended services; compare historical indexing with current configuration.
Reverse proxy / web server	Timestamp, request ID, method, normalized path, status, byte count, source context.	Sensitive files remain inaccessible; directory listings are disabled.
Application authentication	Login outcome, account identifier, session lifecycle, authorization decisions.	Default accounts are removed and protected routes enforce server-side checks.
Filesystem	Sensitive-file permissions and appropriately configured file audit events.	Databases, backups, and secrets reside outside public serving paths.
Database	Connection identity, access denials, scoped query audit events where supported.	The application uses only its required privileges and fictional records.
Lab network	Listener inventory, firewall rules, relevant connection logs.	Vulnerable components stay isolated and cannot reach production OT.


Do not place passwords, session tokens, full configuration secrets, or sensitive database contents in logs. HTTP logs usually establish requests and responses, not that a person read or used the returned content. Correlate evidence rather than overstating what one event proves.
Technical report template
Each finding should contain:
1. Finding ID and title: a precise configuration or behavior description.
2. Asset and scope: exact lab service and authorization boundary.
3. Status: proposed, observed, demonstrated, remediated, or verified.
4. Preconditions: application state and permissions required.
5. Evidence: timestamped, sanitized observations with artifact identifiers.
6. ATT&CK mapping: domain, tactic, technique link, rationale, and uncertainty.
7. Impact: demonstrated effect on fictional data and the separate hypothetical business implication.
8. Remediation: the corrective application, deployment, or access-control change.
9. Verification: expected behavior, observed behavior, date, and remaining gaps.
Keep configuration weaknesses separate from behavior mappings. For example, a database file inside the web root is a deployment finding; map an ATT&CK technique only when the documented behavior and access context support it.
Official mapping references
- Enterprise ATT&CK matrix
- Reconnaissance — TA0043
- Scan Databases — T1596.005
- Active Scanning — T1595
- Exploit Public-Facing Application — T1190
- Default Accounts — T1078.001
- Credentials In Files — T1552.001
- File and Directory Discovery — T1083
- Data from Local System — T1005
- Databases — T1213.006
- ATT&CK for ICS
MITRE ATT&CK and ATT&CK are registered trademarks of The MITRE Corporation. This independent lab is not a MITRE-endorsed project.
