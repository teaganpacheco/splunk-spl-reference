# Search Title

> **Status:** 🟡 Needs Review
> **Category:** `<category>`
> **Canonical SPL:** [`<search-name>.spl`](../searches/<category>/<search-name>.spl)

## Purpose

Briefly describe what the search returns or what administrative question it answers.

Example:

> Lists connected Splunk forwarders and reports their hostname, source IP, forwarder type, and Splunk version.

---

## Use Cases

* `<use case 1>`
* `<use case 2>`
* `<use case 3>`

Example:

* Inventory connected Universal and Heavy Forwarders
* Identify outdated Splunk Forwarder versions
* Validate forwarder connectivity after maintenance or migration

---

## Requirements

| Requirement          | Value                                                                     |
| -------------------- | ------------------------------------------------------------------------- |
| Run From             | `<Search Head / Monitoring Console / Deployment Server / ES Search Head>` |
| Required Indexes     | `<index or N/A>`                                                          |
| Required Sourcetypes | `<sourcetype or N/A>`                                                     |
| Required Apps        | `<app or None>`                                                           |
| Required Macros      | `<macro or None>`                                                         |
| Required Lookups     | `<lookup or None>`                                                        |
| Required Data Models | `<data model or None>`                                                    |

Remove rows that are not relevant to the search.

---

## Search

Canonical file:

```text
searches/<category>/<search-name>.spl
```

```spl
<canonical SPL>
```

---

## Environment-Specific Dependencies

List any dependencies that may require modification before the search can be used elsewhere.

Examples:

* Custom indexes
* Custom sourcetypes
* Custom macros
* Custom lookups
* Enterprise Security
* CIM-compliant fields
* Accelerated data models
* Vendor-specific add-ons
* Organization-specific tags

If none:

> None. Uses standard Splunk fields, commands, or internal data.

---

## Status

| Status                      | Meaning                                                         |
| --------------------------- | --------------------------------------------------------------- |
| ✅ **Validated**             | Tested and ready for general use                                |
| 🟡 **Needs Review**         | Useful search that still requires cleanup or validation         |
| 📦 **Environment Specific** | Depends on custom content or environment-specific configuration |
| ⚡ **Potentially Expensive** | May require additional resources in large environments          |
| 🔴 **Use With Caution**     | Performs or supports potentially disruptive operations          |

Multiple statuses may apply.

Example:

> ✅ Validated · 📦 Environment Specific
