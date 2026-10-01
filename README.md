# Splunk SPL Administrator Reference

A curated collection of reusable **Splunk Search Processing Language (SPL)** searches for Splunk Enterprise administration, troubleshooting, monitoring, validation, and security operations.

This repository grew from a personal collection of SPL accumulated through real-world Splunk administration and engineering work. The goal is to turn those searches into a structured, documented, and reusable reference that can be quickly searched, copied, adapted, and shared.

---

## Purpose

Splunk administrators frequently need quick answers to questions such as:

* Which indexes and sourcetypes are actively receiving data?
* Which forwarders are connected, and what versions are they running?
* Is data experiencing ingestion latency or timestamp issues?
* How much license capacity is being consumed?
* Which indexes are consuming the most storage?
* Which Deployment Server clients have recently phoned home?
* Which saved searches are being skipped?
* Which Enterprise Security correlation searches are enabled?
* Which data models are accelerated?
* Which users and roles exist?
* Which data sources have stopped sending events?

Instead of rebuilding these searches from scratch, this repository provides reusable SPL that can serve as a starting point.

---

# Repository Structure

The repository separates **executable SPL** from its supporting documentation.

```text
splunk-spl-reference/
├── README.md
├── CONTRIBUTING.md
├── LICENSE
│
├── searches/
│   ├── data-ingestion/
│   ├── forwarders/
│   ├── deployment-server/
│   ├── indexes/
│   ├── licensing/
│   ├── search-performance/
│   ├── data-models/
│   ├── enterprise-security/
│   ├── access-and-audit/
│   └── lookups/
│
├── docs/
│   ├── data-ingestion.md
│   ├── forwarders.md
│   ├── deployment-server.md
│   ├── indexes.md
│   ├── licensing.md
│   ├── search-performance.md
│   ├── data-models.md
│   ├── enterprise-security.md
│   ├── access-and-audit.md
│   └── lookups.md
│
├── templates/
│   └── search-entry-template.md
│
└── admin-commands/
    ├── splunk-cli.md
    ├── linux.md
    └── certificates.md
```

---

# Search Categories

## Data Ingestion

SPL for understanding the health, volume, latency, and characteristics of incoming data.

Examples include:

* Event counts by index
* Event counts by sourcetype
* Sourcetype volume over time
* Ingestion latency
* Positive and negative timestamp drift
* `_indextime` versus event time
* Sources associated with sourcetypes
* Hosts contributing to index/sourcetype combinations
* Duplicate event detection
* Silent or inactive data sources

Directory:

```text
searches/data-ingestion/
```

---

## Forwarders

Searches for Universal Forwarder and Heavy Forwarder visibility and troubleshooting.

Examples include:

* Connected forwarder inventory
* Forwarder hostname and IP
* Forwarder version
* Operating system and architecture
* Forwarder throughput
* Forwarders reaching `maxKBps`
* TCP output queue utilization
* Forwarder-to-indexer connections
* SSL connection validation
* Sourcetypes passing through a Heavy Forwarder

Directory:

```text
searches/forwarders/
```

---

## Deployment Server

Searches for Deployment Server administration and deployment client visibility.

Examples include:

* Deployment client inventory
* Last phone-home time
* Splunk version by deployment client
* Applications assigned to clients
* Server class assignments
* Unused server classes
* Deployment client handshake events
* Deployment history

Directory:

```text
searches/deployment-server/
```

---

## Indexes

Searches for index configuration, utilization, storage, and retention.

Examples include:

* Index inventory
* Event counts by index
* Index size
* Earliest event per index
* Configured retention
* Maximum index size
* Hot/warm/cold utilization
* Indexes that have stopped receiving data
* Hosts associated with indexes
* Sourcetypes associated with indexes

Directory:

```text
searches/indexes/
```

---

## Licensing

Searches for Splunk license utilization and ingest volume.

Examples include:

* Daily license usage
* License usage by index
* License usage by sourcetype
* Seven-day average usage
* Thirty-day utilization
* Host contribution
* Estimated ingestion volume
* License usage trends

Directory:

```text
searches/licensing/
```

---

## Search Performance

Searches for monitoring scheduled searches and search-head workload.

Examples include:

* Skipped scheduled searches
* Scheduler activity
* Search runtime
* CPU usage by search
* Memory usage by search
* Scheduled search inventory
* Search frequency
* Search activity by user

Directory:

```text
searches/search-performance/
```

---

## Data Models

Searches for Splunk data model inventory and acceleration health.

Examples include:

* Data model inventory
* Sourcetypes associated with data models
* Indexes associated with data models
* Acceleration status
* Acceleration completeness
* Summary size
* Acceleration retention
* Data model search dependencies

Directory:

```text
searches/data-models/
```

---

## Enterprise Security

Splunk Enterprise Security administration and correlation search visibility.

Examples include:

* Correlation search inventory
* Enabled correlation searches
* Correlation search SPL
* Correlation searches by framework
* MITRE ATT&CK annotations
* Alert counts
* Recently modified correlation searches
* Correlation search dependencies

Directory:

```text
searches/enterprise-security/
```

> Some searches in this section require **Splunk Enterprise Security** and will not function on a standard Splunk Enterprise installation.

---

## Access and Audit

Searches for Splunk authentication, user activity, and RBAC visibility.

Examples include:

* User inventory
* Role assignments
* Active web sessions
* Splunk Web login activity
* Failed logins
* Search activity by user
* Currently active users
* Authentication events

Directory:

```text
searches/access-and-audit/
```

---

## Lookups

Searches for discovering and troubleshooting lookup tables.

Examples include:

* Lookup file inventory
* Finding values across lookup tables
* Searching known entities
* Lookup dependency discovery
* Mapping lookup values into indexed data

Directory:

```text
searches/lookups/
```

---

# Canonical `.spl` Files

Each reusable search should have its own `.spl` file.

For example:

```text
searches/licensing/daily-license-usage.spl
```

The file should contain only executable SPL:

```spl
earliest=-30d@d latest=@d
index=_internal source=*license_usage.log type="RolloverSummary"
| timechart span=1d sum(b) AS license_usage_bytes
| eval license_usage_gb=round(license_usage_bytes/1024/1024/1024, 2)
| eventstats avg(license_usage_gb) AS average_30d_gb
| eval average_30d_gb=round(average_30d_gb, 2)
| fields _time license_usage_gb average_30d_gb
```

Documentation, requirements, and explanations should live in the corresponding Markdown documentation rather than inside the canonical SPL file.

This keeps each `.spl` file easy to:

* Copy
* Paste
* Review
* Diff
* Modify
* Version
* Reference directly from GitHub

---

# Placeholder Convention

Environment-specific values should be replaced with descriptive placeholders before searches are committed.

Use:

```text
<index>
<sourcetype>
<host>
<source>
<splunk_server>
<deployment_server>
<lookup_name>
<data_model>
<application_name>
<username>
<ip_address>
<email_address>
```

Example:

```spl
earliest=-24h
index=<index>
sourcetype=<sourcetype>
host=<host>
| stats count by source
```

Avoid committing customer-specific or internal values unless the repository is explicitly intended to contain them.

---

# Search Status

Searches may be assigned a status as the collection is reviewed.

| Status                      | Meaning                                                     |
| --------------------------- | ----------------------------------------------------------- |
| ✅ **Validated**             | Tested and documented                                       |
| 🟡 **Needs Review**         | Useful search that still requires validation or cleanup     |
| 📦 **Environment Specific** | Depends on custom indexes, macros, lookups, apps, or fields |
| ⚡ **Potentially Expensive** | May consume significant search resources                    |
| 🔴 **Use With Caution**     | Can modify data, configuration, or other state              |

The goal is eventually to have most searches categorized as **Validated**.

---

# Search Documentation Standard

Each documented search should contain the following information when applicable:

```text
Title
Purpose
Category
Requirements
Where to run the search
Required indexes
Required sourcetypes
Required macros
Required lookups
Required data models
Required Splunk apps
Placeholders
Default time range
Expected output
Performance considerations
Splunk version tested
Last validated date
Source or attribution
```

Example:

```markdown
## Daily License Usage

Displays daily Splunk license consumption over the previous 30 days.

**Category:** Licensing  
**Run on:** Search Head or Monitoring Console  
**Required index:** `_internal`  
**Required source:** `license_usage.log`  
**Default time range:** Previous 30 complete days  
**Risk:** Read-only  
**Status:** ✅ Validated
```

---

# SPL Formatting

Long searches should be formatted for readability.

Preferred:

```spl
earliest=-24h
index=_internal
sourcetype=splunkd
group=tcpin_connections
| eval source_host=coalesce(hostname, sourceHost)
| stats
    latest(version) AS version
    latest(sourceIp) AS source_ip
    latest(fwdType) AS forwarder_type
    by source_host
| sort source_host
```

Avoid compressing complex searches into a single line unless there is a specific reason to do so.

---

# Time Range Convention

Whenever practical, searches should explicitly define their intended time range.

Examples:

```spl
earliest=@d latest=now
```

```spl
earliest=-24h latest=now
```

```spl
earliest=-7d@d latest=@d
```

Avoid relying entirely on the Splunk Time Range Picker when the search logic depends on a specific time boundary.

---

# Performance Considerations

Some SPL commands and search patterns can be expensive in large environments.

Use particular care with:

```text
index=*
sourcetype=*
join
map
transaction
append
appendcols
rest
dbinspect
fieldsummary
```

Whenever possible:

* Use `tstats` for metadata-oriented searches.
* Narrow indexes and sourcetypes.
* Use explicit time ranges.
* Avoid unnecessary wildcards.
* Limit REST results when practical.
* Test searches over a small time range first.
* Review searches before running them against large production environments.

---

# Safety

Most searches in this repository are intended to be read-only.

Extra care should be taken with SPL commands that can modify data or create persistent content, including:

```text
delete
collect
outputlookup
sendemail
```

Commands that modify the operating system or Splunk configuration should be stored separately under:

```text
admin-commands/
```

Do not execute unfamiliar commands directly against a production environment.

---

# Sensitive Information

Before contributing or publishing a search, review it for:

* Passwords
* HEC tokens
* Session keys
* API tokens
* Private keys
* Internal FQDNs
* Customer names
* Email addresses
* Public or private IP addresses
* Proprietary lookup names
* Ticket identifiers
* Encrypted credential values
* Internal application names

Remember that removing a secret in a later Git commit does **not** automatically remove it from repository history.

---

# Splunk Version Compatibility

SPL behavior, REST endpoints, internal fields, and application-specific functionality can vary between Splunk releases.

Whenever possible, documentation should include:

```text
Tested with: Splunk Enterprise <version>
```

For example:

```text
Tested with: Splunk Enterprise 10.4
```

This repository should be treated as a **reference and starting point**, not as a guarantee that every search works unchanged in every Splunk environment.

---

# Enterprise-Specific Searches

Some searches may depend on products or applications such as:

* Splunk Enterprise Security
* Splunk Common Information Model
* Splunk Add-on for Microsoft Windows
* Unix and Linux Add-on
* CrowdStrike
* Okta
* AWS Add-ons
* Custom macros
* Custom lookups
* Custom data models

These dependencies should be clearly documented with the corresponding search.

---

# Naming Convention

Canonical SPL filenames should use lowercase kebab-case.

Examples:

```text
daily-license-usage.spl
forwarder-version-inventory.spl
ingestion-latency-by-sourcetype.spl
deployment-client-phone-home.spl
index-storage-and-retention.spl
skipped-scheduled-searches.spl
correlation-search-inventory.spl
```

Names should describe **what the search accomplishes**, rather than where it originally came from.

---

# Contributing

Contributions, improvements, and corrections are welcome.

When adding a search:

1. Place the `.spl` file in the appropriate category.
2. Use the repository placeholder convention.
3. Remove sensitive or environment-specific information.
4. Format the SPL for readability.
5. Document dependencies.
6. Include the expected result.
7. Identify potential performance concerns.
8. Test the search when possible.
9. Update the appropriate category documentation.
10. Update the root README if a new category is introduced.

---

# Planned Repository Cleanup

The initial search collection contains years of accumulated SPL, so migration into this repository will happen incrementally.

The cleanup process includes:

* Categorizing searches
* Removing duplicates
* Correcting formatting introduced by Word
* Replacing smart quotes and special characters
* Standardizing placeholders
* Separating Linux and Splunk CLI commands from SPL
* Identifying environment-specific searches
* Documenting macros and lookup dependencies
* Reviewing expensive searches
* Validating SPL against current Splunk versions
* Renaming searches consistently
* Adding source attribution where appropriate

---

# Disclaimer

These searches are provided as administrative and troubleshooting references.

Always review SPL before running it in a production environment. Search performance and results depend heavily on:

* Data volume
* Search head resources
* Indexer resources
* Search time range
* Index design
* Data model acceleration
* Search-time field extraction
* Installed applications
* Environment-specific configuration

Use appropriate caution with searches that interact with large datasets, REST endpoints, lookup files, summary indexes, or destructive commands.

---

# Roadmap

Planned work:

* [ ] Create repository structure
* [ ] Migrate data-ingestion searches
* [ ] Migrate forwarder searches
* [ ] Migrate Deployment Server searches
* [ ] Migrate index administration searches
* [ ] Migrate license monitoring searches
* [ ] Migrate search performance searches
* [ ] Migrate data model searches
* [ ] Migrate Enterprise Security searches
* [ ] Migrate access and audit searches
* [ ] Migrate lookup searches
* [ ] Separate Linux and Splunk CLI commands
* [ ] Remove duplicate SPL
* [ ] Standardize placeholders
* [ ] Validate searches
* [ ] Add documentation pages
* [ ] Add contribution guidelines
* [ ] Add repository license

---

## Why This Repository Exists

Good SPL tends to get reused.

A search written to solve one production incident often becomes useful again months or years later. Capturing those searches in a structured repository makes that operational knowledge easier to preserve, improve, and share.

The objective of this project is simple:

> **Turn years of useful Splunk searches into a maintainable administrator's toolbox.**
