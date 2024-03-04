<!-- markdownlint-disable -->
# terraform-aws-rds <a href="https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content="><img align="right" src="https://cloudposse.com/logo-300x69.svg" width="150" /></a>
<a href="https://github.com/cloudposse/terraform-aws-rds/releases/latest"><img src="https://img.shields.io/github/release/cloudposse/terraform-aws-rds.svg" alt="Latest Release"/></a><a href="https://slack.cloudposse.com"><img src="https://slack.cloudposse.com/badge.svg" alt="Slack Community"/></a>
<!-- markdownlint-restore -->

<!--




  ** DO NOT EDIT THIS FILE
  **
  ** This file was automatically generated by the `cloudposse/build-harness`.
  ** 1) Make all changes to `README.yaml`
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file.
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **





-->

Terraform module to provision AWS [`RDS`](https://aws.amazon.com/rds/) instances


---
> [!NOTE]
> This project is part of Cloud Posse's comprehensive ["SweetOps"](https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=) approach towards DevOps.
> <details><summary><strong>Learn More</strong></summary>
> <a href="https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=terraform_modules">
>   <picture>
>     <source media="(prefers-color-scheme: dark)" srcset="https://docs.cloudposse.com/images/terraform-open-source-modules-light.svg">
>     <source media="(prefers-color-scheme: light)" srcset="https://docs.cloudposse.com/images/terraform-open-source-modules-dark.svg">
>     <img alt="Terraform Open Source Modules" src="https://docs.cloudposse.com/images/terraform-open-source-modules.svg" align="right">
>   </picture>
> </a>
>
>
> It's 100% Open Source and licensed under the [APACHE2](LICENSE).
>
> We literally have [*hundreds of terraform modules*](https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=terraform_modules) that are Open Source and well-maintained. Check them out!
> </details>

<a href="https://cloudposse.com/readme/header/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=readme_header_link"><img src="https://cloudposse.com/readme/header/img"/></a>


## Introduction

The module will create:

* DB instance (MySQL, Postgres, SQL Server, Oracle)
* DB Option Group (will create a new one or you may use an existing)
* DB Parameter Group
* DB Subnet Group
* DB Security Group
* DNS Record in Route53 for the DB endpoint



## Usage



> [!IMPORTANT]
> In Cloud Posse's examples, we avoid pinning modules to specific versions to prevent discrepancies between the documentation 
> and the latest released versions. However, for your own projects, we strongly advise pinning each module to the exact version
> you're using. This practice ensures the stability of your infrastructure. Additionally, we recommend implementing a systematic 
> approach for updating versions to avoid unexpected changes.



```hcl
module "rds_instance" {
    source = "cloudposse/rds/aws"
    # Cloud Posse recommends pinning every module to a specific version
    # version = "x.x.x"
    namespace                   = "eg"
    stage                       = "prod"
    name                        = "app"
    dns_zone_id                 = "Z89FN1IW975KPE"
    host_name                   = "db"
    security_group_ids          = ["sg-xxxxxxxx"]
    ca_cert_identifier          = "rds-ca-2019"
    allowed_cidr_blocks         = ["XXX.XXX.XXX.XXX/32"]
    database_name               = "wordpress"
    database_user               = "admin"
    database_password           = "xxxxxxxxxxxx"
    database_port               = 3306
    multi_az                    = true
    storage_type                = "gp2"
    allocated_storage           = 100
    storage_encrypted           = true
    engine                      = "mysql"
    engine_version              = "5.7.17"
    major_engine_version        = "5.7"
    instance_class              = "db.t2.medium"
    db_parameter_group          = "mysql5.7"
    option_group_name           = "mysql-options"
    publicly_accessible         = false
    subnet_ids                  = ["sb-xxxxxxxxx", "sb-xxxxxxxxx"]
    vpc_id                      = "vpc-xxxxxxxx"
    snapshot_identifier         = "rds:production-2015-06-26-06-05"
    auto_minor_version_upgrade  = true
    allow_major_version_upgrade = false
    apply_immediately           = false
    maintenance_window          = "Mon:03:00-Mon:04:00"
    skip_final_snapshot         = false
    copy_tags_to_snapshot       = true
    backup_retention_period     = 7
    backup_window               = "22:00-03:00"

    db_parameter = [
      { name  = "myisam_sort_buffer_size"   value = "1048576" },
      { name  = "sort_buffer_size"          value = "2097152" }
    ]

    db_options = [
      { option_name = "MARIADB_AUDIT_PLUGIN"
          option_settings = [
            { name = "SERVER_AUDIT_EVENTS"           value = "CONNECT" },
            { name = "SERVER_AUDIT_FILE_ROTATIONS"   value = "37" }
          ]
      }
    ]
}
```
### Character Sets

If you wish to create the database in a specific character set you can use one of the following options depending
on your database engine of choice.

For Oracle and Microsoft SQL you can specify charset name as an input variable
to this module. For example, for Microsoft SQL, you could use:
```hcl
module "rds_instance" {
  ...
  charset_name                   = "Korean_Wansung_CI_AS"
  ...
}
```

For `mysql` and `mariadb` engines character set of the database can be defined via `db_parameter`. In this example
the database is created with `utf8mb4` (character set) and utf8mb4_unicode_ci (collation):

```hcl
module "rds_instance" {
  ...
  db_parameter = [
    {
      name = "character_set_server"
      value = "utf8mb4"
      apply_method = "immediate"
    },
    {
      name = "collation_server"
      value = "utf8mb4_unicode_ci"
      apply_method = "immediate"
    }
  ]
  ...
}
```






<!-- markdownlint-disable -->
## Makefile Targets
```text
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```
<!-- markdownlint-restore -->
<!-- markdownlint-disable -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.3 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 5.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 5.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_dns_host_name"></a> [dns\_host\_name](#module\_dns\_host\_name) | cloudposse/route53-cluster-hostname/aws | 0.12.2 |
| <a name="module_final_snapshot_label"></a> [final\_snapshot\_label](#module\_final\_snapshot\_label) | cloudposse/label/null | 0.25.0 |
| <a name="module_this"></a> [this](#module\_this) | cloudposse/label/null | 0.25.0 |

## Resources

| Name | Type |
|------|------|
| [aws_db_instance.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance) | resource |
| [aws_db_option_group.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_option_group) | resource |
| [aws_db_parameter_group.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_parameter_group) | resource |
| [aws_db_subnet_group.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_subnet_group) | resource |
| [aws_security_group.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group) | resource |
| [aws_security_group_rule.egress](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.ingress_cidr_blocks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.ingress_security_groups](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_additional_tag_map"></a> [additional\_tag\_map](#input\_additional\_tag\_map) | Additional key-value pairs to add to each map in `tags_as_list_of_maps`. Not added to `tags` or `id`.<br>This is for some rare cases where resources want additional configuration of tags<br>and therefore take a list of maps with tag key, value, and additional configuration. | `map(string)` | `{}` | no |
| <a name="input_allocated_storage"></a> [allocated\_storage](#input\_allocated\_storage) | The allocated storage in GBs. Required unless a `snapshot_identifier` or `replicate_source_db` is provided. | `number` | `null` | no |
| <a name="input_allow_major_version_upgrade"></a> [allow\_major\_version\_upgrade](#input\_allow\_major\_version\_upgrade) | Allow major version upgrade | `bool` | `false` | no |
| <a name="input_allowed_cidr_blocks"></a> [allowed\_cidr\_blocks](#input\_allowed\_cidr\_blocks) | The whitelisted CIDRs which to allow `ingress` traffic to the DB instance | `list(string)` | `[]` | no |
| <a name="input_apply_immediately"></a> [apply\_immediately](#input\_apply\_immediately) | Specifies whether any database modifications are applied immediately, or during the next maintenance window | `bool` | `false` | no |
| <a name="input_associate_security_group_ids"></a> [associate\_security\_group\_ids](#input\_associate\_security\_group\_ids) | The IDs of the existing security groups to associate with the DB instance | `list(string)` | `[]` | no |
| <a name="input_attributes"></a> [attributes](#input\_attributes) | ID element. Additional attributes (e.g. `workers` or `cluster`) to add to `id`,<br>in the order they appear in the list. New attributes are appended to the<br>end of the list. The elements of the list are joined by the `delimiter`<br>and treated as a single ID element. | `list(string)` | `[]` | no |
| <a name="input_auto_minor_version_upgrade"></a> [auto\_minor\_version\_upgrade](#input\_auto\_minor\_version\_upgrade) | Allow automated minor version upgrade (e.g. from Postgres 9.5.3 to Postgres 9.5.4) | `bool` | `true` | no |
| <a name="input_availability_zone"></a> [availability\_zone](#input\_availability\_zone) | The AZ for the RDS instance. Specify one of `subnet_ids`, `db_subnet_group_name` or `availability_zone`. If `availability_zone` is provided, the instance will be placed into the default VPC or EC2 Classic | `string` | `null` | no |
| <a name="input_backup_retention_period"></a> [backup\_retention\_period](#input\_backup\_retention\_period) | Backup retention period in days. Must be > 0 to enable backups | `number` | `0` | no |
| <a name="input_backup_window"></a> [backup\_window](#input\_backup\_window) | When AWS can perform DB snapshots, can't overlap with maintenance window | `string` | `"22:00-03:00"` | no |
| <a name="input_ca_cert_identifier"></a> [ca\_cert\_identifier](#input\_ca\_cert\_identifier) | The identifier of the CA certificate for the DB instance | `string` | `null` | no |
| <a name="input_charset_name"></a> [charset\_name](#input\_charset\_name) | The character set name to use for DB encoding. [Oracle & Microsoft SQL only](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance#character_set_name). For other engines use `db_parameter` | `string` | `null` | no |
| <a name="input_context"></a> [context](#input\_context) | Single object for setting entire context at once.<br>See description of individual variables for details.<br>Leave string and numeric variables as `null` to use default value.<br>Individual variable settings (non-null) override settings in context object,<br>except for attributes, tags, and additional\_tag\_map, which are merged. | `any` | <pre>{<br>  "additional_tag_map": {},<br>  "attributes": [],<br>  "delimiter": null,<br>  "descriptor_formats": {},<br>  "enabled": true,<br>  "environment": null,<br>  "id_length_limit": null,<br>  "label_key_case": null,<br>  "label_order": [],<br>  "label_value_case": null,<br>  "labels_as_tags": [<br>    "unset"<br>  ],<br>  "name": null,<br>  "namespace": null,<br>  "regex_replace_chars": null,<br>  "stage": null,<br>  "tags": {},<br>  "tenant": null<br>}</pre> | no |
| <a name="input_copy_tags_to_snapshot"></a> [copy\_tags\_to\_snapshot](#input\_copy\_tags\_to\_snapshot) | Copy tags from DB to a snapshot | `bool` | `true` | no |
| <a name="input_database_manage_master_user_password"></a> [database\_manage\_master\_user\_password](#input\_database\_manage\_master\_user\_password) | Set to true to allow RDS to manage the master user password in Secrets Manager. Ignore if `database_password` is provided. | `bool` | `false` | no |
| <a name="input_database_master_user_secret_kms_key_id"></a> [database\_master\_user\_secret\_kms\_key\_id](#input\_database\_master\_user\_secret\_kms\_key\_id) | The Amazon Web Services KMS key identifier is the key ARN, key ID, alias ARN, or alias name for the KMS key. To use a KMS key in a different Amazon Web Services account, specify the key ARN or alias ARN. If not specified, the default KMS key for your Amazon Web Services account is used. | `string` | `null` | no |
| <a name="input_database_name"></a> [database\_name](#input\_database\_name) | The name of the database to create when the DB instance is created | `string` | `null` | no |
| <a name="input_database_password"></a> [database\_password](#input\_database\_password) | Password for the primary DB user. Required unless a `snapshot_identifier` or `replicate_source_db` is provided. | `string` | `null` | no |
| <a name="input_database_port"></a> [database\_port](#input\_database\_port) | Database port (\_e.g.\_ `3306` for `MySQL`). Used in the DB Security Group to allow access to the DB instance from the provided `security_group_ids` | `number` | n/a | yes |
| <a name="input_database_user"></a> [database\_user](#input\_database\_user) | Username for the primary DB user. Required unless a `snapshot_identifier` or `replicate_source_db` is provided. | `string` | `null` | no |
| <a name="input_db_options"></a> [db\_options](#input\_db\_options) | A list of DB options to apply with an option group. Depends on DB engine | <pre>list(object({<br>    db_security_group_memberships  = list(string)<br>    option_name                    = string<br>    port                           = number<br>    version                        = string<br>    vpc_security_group_memberships = list(string)<br><br>    option_settings = list(object({<br>      name  = string<br>      value = string<br>    }))<br>  }))</pre> | `[]` | no |
| <a name="input_db_parameter"></a> [db\_parameter](#input\_db\_parameter) | A list of DB parameters to apply. Note that parameters may differ from a DB family to another | <pre>list(object({<br>    apply_method = string<br>    name         = string<br>    value        = string<br>  }))</pre> | `[]` | no |
| <a name="input_db_parameter_group"></a> [db\_parameter\_group](#input\_db\_parameter\_group) | The DB parameter group family name. The value depends on DB engine used. See [DBParameterGroupFamily](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CreateDBParameterGroup.html#API_CreateDBParameterGroup_RequestParameters) for instructions on how to retrieve applicable value. | `string` | n/a | yes |
| <a name="input_db_subnet_group_name"></a> [db\_subnet\_group\_name](#input\_db\_subnet\_group\_name) | Name of DB subnet group. DB instance will be created in the VPC associated with the DB subnet group. Specify one of `subnet_ids`, `db_subnet_group_name` or `availability_zone` | `string` | `null` | no |
| <a name="input_deletion_protection"></a> [deletion\_protection](#input\_deletion\_protection) | Set to true to enable deletion protection on the RDS instance | `bool` | `false` | no |
| <a name="input_delimiter"></a> [delimiter](#input\_delimiter) | Delimiter to be used between ID elements.<br>Defaults to `-` (hyphen). Set to `""` to use no delimiter at all. | `string` | `null` | no |
| <a name="input_descriptor_formats"></a> [descriptor\_formats](#input\_descriptor\_formats) | Describe additional descriptors to be output in the `descriptors` output map.<br>Map of maps. Keys are names of descriptors. Values are maps of the form<br>`{<br>   format = string<br>   labels = list(string)<br>}`<br>(Type is `any` so the map values can later be enhanced to provide additional options.)<br>`format` is a Terraform format string to be passed to the `format()` function.<br>`labels` is a list of labels, in order, to pass to `format()` function.<br>Label values will be normalized before being passed to `format()` so they will be<br>identical to how they appear in `id`.<br>Default is `{}` (`descriptors` output will be empty). | `any` | `{}` | no |
| <a name="input_dns_zone_id"></a> [dns\_zone\_id](#input\_dns\_zone\_id) | The ID of the DNS Zone in Route53 where a new DNS record will be created for the DB host name | `string` | `""` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Set to false to prevent the module from creating any resources | `bool` | `null` | no |
| <a name="input_enabled_cloudwatch_logs_exports"></a> [enabled\_cloudwatch\_logs\_exports](#input\_enabled\_cloudwatch\_logs\_exports) | List of log types to enable for exporting to CloudWatch logs. If omitted, no logs will be exported. Valid values (depending on engine): alert, audit, error, general, listener, slowquery, trace, postgresql (PostgreSQL), upgrade (PostgreSQL). | `list(string)` | `[]` | no |
| <a name="input_engine"></a> [engine](#input\_engine) | Database engine type. Required unless a `snapshot_identifier` or `replicate_source_db` is provided. | `string` | `null` | no |
| <a name="input_engine_version"></a> [engine\_version](#input\_engine\_version) | Database engine version, depends on engine type. | `string` | n/a | yes |
| <a name="input_environment"></a> [environment](#input\_environment) | ID element. Usually used for region e.g. 'uw2', 'us-west-2', OR role 'prod', 'staging', 'dev', 'UAT' | `string` | `null` | no |
| <a name="input_final_snapshot_identifier"></a> [final\_snapshot\_identifier](#input\_final\_snapshot\_identifier) | Final snapshot identifier e.g.: some-db-final-snapshot-2019-06-26-06-05 | `string` | `""` | no |
| <a name="input_host_name"></a> [host\_name](#input\_host\_name) | The DB host name created in Route53 | `string` | `"db"` | no |
| <a name="input_iam_database_authentication_enabled"></a> [iam\_database\_authentication\_enabled](#input\_iam\_database\_authentication\_enabled) | Specifies whether or mappings of AWS Identity and Access Management (IAM) accounts to database accounts is enabled | `bool` | `false` | no |
| <a name="input_id_length_limit"></a> [id\_length\_limit](#input\_id\_length\_limit) | Limit `id` to this many characters (minimum 6).<br>Set to `0` for unlimited length.<br>Set to `null` for keep the existing setting, which defaults to `0`.<br>Does not affect `id_full`. | `number` | `null` | no |
| <a name="input_instance_class"></a> [instance\_class](#input\_instance\_class) | Class of RDS instance | `string` | n/a | yes |
| <a name="input_iops"></a> [iops](#input\_iops) | The amount of provisioned IOPS. Setting this implies a storage\_type of 'io1'. Default is 0 if rds storage type is not 'io1' | `number` | `0` | no |
| <a name="input_kms_key_arn"></a> [kms\_key\_arn](#input\_kms\_key\_arn) | The ARN of the existing KMS key to encrypt storage | `string` | `""` | no |
| <a name="input_label_key_case"></a> [label\_key\_case](#input\_label\_key\_case) | Controls the letter case of the `tags` keys (label names) for tags generated by this module.<br>Does not affect keys of tags passed in via the `tags` input.<br>Possible values: `lower`, `title`, `upper`.<br>Default value: `title`. | `string` | `null` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | The order in which the labels (ID elements) appear in the `id`.<br>Defaults to ["namespace", "environment", "stage", "name", "attributes"].<br>You can omit any of the 6 labels ("tenant" is the 6th), but at least one must be present. | `list(string)` | `null` | no |
| <a name="input_label_value_case"></a> [label\_value\_case](#input\_label\_value\_case) | Controls the letter case of ID elements (labels) as included in `id`,<br>set as tag values, and output by this module individually.<br>Does not affect values of tags passed in via the `tags` input.<br>Possible values: `lower`, `title`, `upper` and `none` (no transformation).<br>Set this to `title` and set `delimiter` to `""` to yield Pascal Case IDs.<br>Default value: `lower`. | `string` | `null` | no |
| <a name="input_labels_as_tags"></a> [labels\_as\_tags](#input\_labels\_as\_tags) | Set of labels (ID elements) to include as tags in the `tags` output.<br>Default is to include all labels.<br>Tags with empty values will not be included in the `tags` output.<br>Set to `[]` to suppress all generated tags.<br>**Notes:**<br>  The value of the `name` tag, if included, will be the `id`, not the `name`.<br>  Unlike other `null-label` inputs, the initial setting of `labels_as_tags` cannot be<br>  changed in later chained modules. Attempts to change it will be silently ignored. | `set(string)` | <pre>[<br>  "default"<br>]</pre> | no |
| <a name="input_license_model"></a> [license\_model](#input\_license\_model) | License model for this DB. Optional, but required for some DB Engines. Valid values: license-included \| bring-your-own-license \| general-public-license | `string` | `""` | no |
| <a name="input_maintenance_window"></a> [maintenance\_window](#input\_maintenance\_window) | The window to perform maintenance in. Syntax: 'ddd:hh24:mi-ddd:hh24:mi' UTC | `string` | `"Mon:03:00-Mon:04:00"` | no |
| <a name="input_major_engine_version"></a> [major\_engine\_version](#input\_major\_engine\_version) | Database MAJOR engine version, depends on engine type | `string` | `""` | no |
| <a name="input_max_allocated_storage"></a> [max\_allocated\_storage](#input\_max\_allocated\_storage) | The upper limit to which RDS can automatically scale the storage in GBs | `number` | `0` | no |
| <a name="input_monitoring_interval"></a> [monitoring\_interval](#input\_monitoring\_interval) | The interval, in seconds, between points when Enhanced Monitoring metrics are collected for the DB instance. To disable collecting Enhanced Monitoring metrics, specify 0. Valid Values are 0, 1, 5, 10, 15, 30, 60. | `string` | `"0"` | no |
| <a name="input_monitoring_role_arn"></a> [monitoring\_role\_arn](#input\_monitoring\_role\_arn) | The ARN for the IAM role that permits RDS to send enhanced monitoring metrics to CloudWatch Logs | `string` | `null` | no |
| <a name="input_multi_az"></a> [multi\_az](#input\_multi\_az) | Set to true if multi AZ deployment must be supported | `bool` | `false` | no |
| <a name="input_name"></a> [name](#input\_name) | ID element. Usually the component or solution name, e.g. 'app' or 'jenkins'.<br>This is the only ID element not also included as a `tag`.<br>The "name" tag is set to the full `id` string. There is no tag with the value of the `name` input. | `string` | `null` | no |
| <a name="input_namespace"></a> [namespace](#input\_namespace) | ID element. Usually an abbreviation of your organization name, e.g. 'eg' or 'cp', to help ensure generated IDs are globally unique | `string` | `null` | no |
| <a name="input_option_group_name"></a> [option\_group\_name](#input\_option\_group\_name) | Name of the DB option group to associate | `string` | `""` | no |
| <a name="input_parameter_group_name"></a> [parameter\_group\_name](#input\_parameter\_group\_name) | Name of the DB parameter group to associate | `string` | `""` | no |
| <a name="input_performance_insights_enabled"></a> [performance\_insights\_enabled](#input\_performance\_insights\_enabled) | Specifies whether Performance Insights are enabled. | `bool` | `false` | no |
| <a name="input_performance_insights_kms_key_id"></a> [performance\_insights\_kms\_key\_id](#input\_performance\_insights\_kms\_key\_id) | The ARN for the KMS key to encrypt Performance Insights data. Once KMS key is set, it can never be changed. | `string` | `null` | no |
| <a name="input_performance_insights_retention_period"></a> [performance\_insights\_retention\_period](#input\_performance\_insights\_retention\_period) | The amount of time in days to retain Performance Insights data. Either 7 (7 days) or 731 (2 years). | `number` | `7` | no |
| <a name="input_publicly_accessible"></a> [publicly\_accessible](#input\_publicly\_accessible) | Determines if database can be publicly available (NOT recommended) | `bool` | `false` | no |
| <a name="input_regex_replace_chars"></a> [regex\_replace\_chars](#input\_regex\_replace\_chars) | Terraform regular expression (regex) string.<br>Characters matching the regex will be removed from the ID elements.<br>If not set, `"/[^a-zA-Z0-9-]/"` is used to remove all characters other than hyphens, letters and digits. | `string` | `null` | no |
| <a name="input_replicate_source_db"></a> [replicate\_source\_db](#input\_replicate\_source\_db) | Specifies that this resource is a Replicate database, and to use this value as the source database. This correlates to the `identifier` of another Amazon RDS Database to replicate (if replicating within a single region) or ARN of the Amazon RDS Database to replicate (if replicating cross-region). Note that if you are creating a cross-region replica of an encrypted database you will also need to specify a `kms_key_id`. See [DB Instance Replication](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Replication.html) and [Working with PostgreSQL and MySQL Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) for more information on using Replication. | `string` | `null` | no |
| <a name="input_restore_to_point_in_time"></a> [restore\_to\_point\_in\_time](#input\_restore\_to\_point\_in\_time) | An object specifying the restore point in time for the DB instance to restore from. Only used when `snapshot_identifier` is not provided. | <pre>object({<br>    restore_time                             = optional(string, null)<br>    source_db_instance_identifier            = optional(string, null)<br>    source_db_instance_automated_backups_arn = optional(string, null)<br>    source_dbi_resource_id                   = optional(string, null)<br>    use_latest_restorable_time               = optional(bool, null)<br>  })</pre> | `null` | no |
| <a name="input_security_group_ids"></a> [security\_group\_ids](#input\_security\_group\_ids) | The IDs of the security groups from which to allow `ingress` traffic to the DB instance | `list(string)` | `[]` | no |
| <a name="input_skip_final_snapshot"></a> [skip\_final\_snapshot](#input\_skip\_final\_snapshot) | If true (default), no snapshot will be made before deleting DB | `bool` | `true` | no |
| <a name="input_snapshot_identifier"></a> [snapshot\_identifier](#input\_snapshot\_identifier) | Snapshot identifier e.g: `rds:production-2019-06-26-06-05` for automated or `manual-backup-2023-11-16` for manual. If specified, the module create the instance from the snapshot. | `string` | `null` | no |
| <a name="input_stage"></a> [stage](#input\_stage) | ID element. Usually used to indicate role, e.g. 'prod', 'staging', 'source', 'build', 'test', 'deploy', 'release' | `string` | `null` | no |
| <a name="input_storage_encrypted"></a> [storage\_encrypted](#input\_storage\_encrypted) | (Optional) Specifies whether the DB instance is encrypted. The default is false if not specified | `bool` | `true` | no |
| <a name="input_storage_throughput"></a> [storage\_throughput](#input\_storage\_throughput) | The storage throughput value for the DB instance. Can only be set when `storage_type` is `gp3`. Cannot be specified if the `allocated_storage` value is below a per-engine threshold. | `number` | `null` | no |
| <a name="input_storage_type"></a> [storage\_type](#input\_storage\_type) | One of 'standard' (magnetic), 'gp2' (general purpose SSD), 'gp3' (general purpose SSD), or 'io1' (provisioned IOPS SSD) | `string` | `"standard"` | no |
| <a name="input_subnet_ids"></a> [subnet\_ids](#input\_subnet\_ids) | List of subnet IDs for the DB. DB instance will be created in the VPC associated with the DB subnet group provisioned using the subnet IDs. Specify one of `subnet_ids`, `db_subnet_group_name` or `availability_zone` | `list(string)` | `[]` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | Additional tags (e.g. `{'BusinessUnit': 'XYZ'}`).<br>Neither the tag keys nor the tag values will be modified by this module. | `map(string)` | `{}` | no |
| <a name="input_tenant"></a> [tenant](#input\_tenant) | ID element \_(Rarely used, not included by default)\_. A customer identifier, indicating who this instance of a resource is for | `string` | `null` | no |
| <a name="input_timeouts"></a> [timeouts](#input\_timeouts) | A list of DB timeouts to apply to the running code while creating, updating, or deleting the DB instance. | <pre>object({<br>    create = string<br>    update = string<br>    delete = string<br>  })</pre> | <pre>{<br>  "create": "40m",<br>  "delete": "60m",<br>  "update": "80m"<br>}</pre> | no |
| <a name="input_timezone"></a> [timezone](#input\_timezone) | Time zone of the DB instance. timezone is currently only supported by Microsoft SQL Server. The timezone can only be set on creation. See [MSSQL User Guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SQLServer.html#SQLServer.Concepts.General.TimeZone) for more information. | `string` | `null` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | VPC ID the DB instance will be created in | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_hostname"></a> [hostname](#output\_hostname) | DNS host name of the instance |
| <a name="output_instance_address"></a> [instance\_address](#output\_instance\_address) | Address of the instance |
| <a name="output_instance_arn"></a> [instance\_arn](#output\_instance\_arn) | ARN of the instance |
| <a name="output_instance_endpoint"></a> [instance\_endpoint](#output\_instance\_endpoint) | DNS Endpoint of the instance |
| <a name="output_instance_id"></a> [instance\_id](#output\_instance\_id) | ID of the instance |
| <a name="output_master_user_secret"></a> [master\_user\_secret](#output\_master\_user\_secret) | Secret object if configured with `var.database_manage_master_user_password = true`. |
| <a name="output_option_group_id"></a> [option\_group\_id](#output\_option\_group\_id) | ID of the Option Group |
| <a name="output_parameter_group_id"></a> [parameter\_group\_id](#output\_parameter\_group\_id) | ID of the Parameter Group |
| <a name="output_resource_id"></a> [resource\_id](#output\_resource\_id) | The RDS Resource ID of this instance. |
| <a name="output_security_group_id"></a> [security\_group\_id](#output\_security\_group\_id) | ID of the Security Group |
| <a name="output_subnet_group_id"></a> [subnet\_group\_id](#output\_subnet\_group\_id) | ID of the created Subnet Group |
<!-- markdownlint-restore -->


## Related Projects

Check out these related projects.

- [terraform-aws-rds-cluster](https://github.com/cloudposse/terraform-aws-rds-cluster) - Terraform module to provision an RDS Aurora cluster for MySQL or Postgres
- [terraform-aws-rds-cloudwatch-sns-alarms](https://github.com/cloudposse/terraform-aws-rds-cloudwatch-sns-alarms) - Terraform module that configures important RDS alerts using CloudWatch and sends them to an SNS topic

## ✨ Contributing

This project is under active development, and we encourage contributions from our community.
Many thanks to our outstanding contributors:

<a href="https://github.com/cloudposse/terraform-aws-rds/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=cloudposse/terraform-aws-rds&max=24" />
</a>

### 🐛 Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-rds/issues) to report any bugs or file feature requests.

### 💻 Developing

If you are interested in being a contributor and want to get involved in developing this project or help out with Cloud Posse's other projects, we would love to hear from you! 
Hit us up in [Slack](https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=slack), in the `#cloudposse` channel.

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.
 1. Review our [Code of Conduct](https://github.com/cloudposse/terraform-aws-rds/?tab=coc-ov-file#code-of-conduct) and [Contributor Guidelines](https://github.com/cloudposse/.github/blob/main/CONTRIBUTING.md).
 2. **Fork** the repo on GitHub
 3. **Clone** the project to your own machine
 4. **Commit** changes to your own branch
 5. **Push** your work back up to your fork
 6. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!

### 🌎 Slack Community

Join our [Open Source Community](https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=slack) on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

### 📰 Newsletter

Sign up for [our newsletter](https://cpco.io/newsletter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=newsletter) and join 3,000+ DevOps engineers, CTOs, and founders who get insider access to the latest DevOps trends, so you can always stay in the know.
Dropped straight into your Inbox every week — and usually a 5-minute read.

### 📆 Office Hours <a href="https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=office_hours"><img src="https://img.cloudposse.com/fit-in/200x200/https://cloudposse.com/wp-content/uploads/2019/08/Powered-by-Zoom.png" align="right" /></a>

[Join us every Wednesday via Zoom](https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=office_hours) for your weekly dose of insider DevOps trends, AWS news and Terraform insights, all sourced from our SweetOps community, plus a _live Q&A_ that you can’t find anywhere else.
It's **FREE** for everyone!

## About

This project is maintained by <a href="https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=">Cloud Posse, LLC</a>.
<a href="https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content="><img src="https://cloudposse.com/logo-300x69.svg" align="right" /></a>

We are a [**DevOps Accelerator**](https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=commercial_support) for funded startups and enterprises.
Use our ready-to-go terraform architecture blueprints for AWS to get up and running quickly.
We build it with you. You own everything. Your team wins. Plus, we stick around until you succeed.

<a href="https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=commercial_support"><img alt="Learn More" src="https://img.shields.io/badge/learn%20more-success.svg?style=for-the-badge"/></a>

*Your team can operate like a pro today.*

Ensure that your team succeeds by using our proven process and turnkey blueprints. Plus, we stick around until you succeed.

<details>
  <summary>📚 <strong>See What's Included</strong></summary>

- **Reference Architecture.** You'll get everything you need from the ground up built using 100% infrastructure as code.
- **Deployment Strategy.** You'll have a battle-tested deployment strategy using GitHub Actions that's automated and repeatable.
- **Site Reliability Engineering.** You'll have total visibility into your apps and microservices.
- **Security Baseline.** You'll have built-in governance with accountability and audit logs for all changes.
- **GitOps.** You'll be able to operate your infrastructure via Pull Requests.
- **Training.** You'll receive hands-on training so your team can operate what we build.
- **Questions.** You'll have a direct line of communication between our teams via a Shared Slack channel.
- **Troubleshooting.** You'll get help to triage when things aren't working.
- **Code Reviews.** You'll receive constructive feedback on Pull Requests.
- **Bug Fixes.** We'll rapidly work with you to fix any bugs in our projects.
</details>

<a href="https://cloudposse.com/readme/commercial-support/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=readme_commercial_support_link"><img src="https://cloudposse.com/readme/commercial-support/img"/></a>
## License

<a href="https://opensource.org/licenses/Apache-2.0"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=for-the-badge" alt="License"></a>

<details>
<summary>Preamble to the Apache License, Version 2.0</summary>
<br/>
<br/>

Complete license is available in the [`LICENSE`](LICENSE) file.

```text
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
```
</details>

## Trademarks

All other trademarks referenced herein are the property of their respective owners.
---
Copyright © 2017-2024 [Cloud Posse, LLC](https://cpco.io/copyright)


<a href="https://cloudposse.com/readme/footer/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=readme_footer_link"><img alt="README footer" src="https://cloudposse.com/readme/footer/img"/></a>

<img alt="Beacon" width="0" src="https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-rds?pixel&cs=github&cm=readme&an=terraform-aws-rds"/>
