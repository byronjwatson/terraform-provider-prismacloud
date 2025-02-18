---
page_title: "Prisma Cloud: prismacloud_policy"
---

# prismacloud_policy

Manage a specific policy.

## Example Usage

```hcl
resource "prismacloud_policy" "example" {
    name = "My Policy"
    policy_type = "network"
    rule {
        name = "my rule"
        criteria = "savedSearchId"
        parameters = {
            "savedSearch": "true",
            "withIac": "false",
        }
        rule_type = "Network"
    }
}
```

## Argument Reference

* `name` - (Required) Policy name
* `policy_type` - (Required) Policy type. Valid values are `config`, `audit_event`, `iam`, `network`, `data`, or `anomaly`
* `description` - Description
* `severity` - Severity. Valid values are `low` (default), `medium`, or `high`.
* `recommendation` - Remediation recommendation
* `cloud_type` - Cloud type (Optional for policies having RQL query with multiway joins, otherwise required) - valid values are `aws`,`azure`,`gcp`,`alibaba_cloud` and `all`
* `labels` - List of labels
* `enabled` - (bool) Enabled
* `overridden` - (bool) Overridden
* `deleted` - (bool) Deleted
* `restrict_alert_dismissal` - (bool) Restrict alert dismissal
* `rule` - (Required) Model for the rule, as defined [below](#rule)
* `remediation` - Model for remediation, as defined [below](#remediation)
* `compliance_metadata` - List of compliance data. Each item has compliance standard, requirement, and/or section information, as defined [below](#compliance-metadata)

### Rule

One and only one of these must be present:

* `name` - (Required) Name
* `cloud_type` - Cloud type
* `cloud_account` - Cloud account
* `resource_type` - Resource type
* `api_name` - API name
* `resource_id_path` - Resource ID path
* `criteria` - (Required for Config, Audit Event, IAM and Network policies) Saved search ID that defines the rule criteria
* `data_criteria` - (Required for Data policy) Criteria for DLP Rule, as defined [below](#data-criteria)
* `children` - (Required for Config build policy) Children description for build policy, as defined [below](#children)
* `parameters` - (Required for Config, Audit Event, IAM and Network policies, map of strings) Parameters. Valid keys are `withIac` and `savedSearch` and value is `"true"`or `"false"` (`SavedSearch` is true when we are using savedsearch and it is false when we directly give search query and `withIac` is true for build policies otherwise false)
* `rule_type` - (Required) Type of rule or RQL query. Valid values are `Config`, `AuditEvent`, `IAM`, `Network`, `DLP`, or `Anomaly`

### Remediation

This section may be present or may be omitted:

* `template_type` - Template type
* `description` - Description
* `cli_script_template` - CLI script template
* `cli_script_json_schema_string` - CLI script JSON schema

### Compliance Metadata

* `compliance_id` - (Required) Compliance Section UUID

#### Data Criteria

* `classification_result` - (Required) Data Profile name required for DLP rule criteria
* `exposure` - File exposure. Valid values are `private`, `public`, or `conditional`
* `extension` - List of file extensions

#### Children

* `criteria` - (Required for custom build policy) Criteria for build policy.
* `metadata` - (Required for custom code build policy, map of string) YAML string for code build policy. Valid key is `code`. 
* `recommendation` - (Optional, string) Recommendation.
* `type` - (Required) Type of policy. Valid values are: `tf`, `cft`, `k8s` or `build`.

## Attribute Reference

* `policy_id` - Policy ID
* `policy_subtypes` - Policy subtypes
* `created_on` - (int) Created on
* `created_by` - Created by
* `last_modified_on` - (int) Last modified on
* `last_modified_by` - Last modified by
* `rule_last_modified_on` - (int) Rule last modified on
* `open_alerts_count` - (int) Open alerts count
* `owner` - Owner
* `policy_mode` - Policy mode
* `policy_category` - Policy category
* `policy_class` - Policy class
* `system_default` - (bool) If policy is a system default policy or not
* `remediable` - (bool) Is remediable or not

In each `Compliance Metadata` section, the following attributes are available:

* `standard_name` - Compliance standard name
* `standard_description` - Compliance standard description
* `requirement_name` - Requirement name
* `requirement_id` - Requirement ID
* `requirement_description` - Requirement description
* `section_description` - Section description
* `section_id` - Section ID
* `section_label` - Section label
* `policy_id` - Policy ID
* `custom_assigned` - (bool) Custom assigned

## Import

Resources can be imported using the policy ID:

```
$ terraform import prismacloud_policy.example 11111111-2222-3333-4444-555555555555
```