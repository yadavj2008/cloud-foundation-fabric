# Google Cloud BigTable Module

This module allows managing a single BigTable instance, including access configuration and tables.

## TODO

- [ ] support bigtable_gc_policy
- [ ] support bigtable_app_profile
- [ ] support cluster replicas
- [ ] support IAM for tables

## Examples

### Instance with access configuration

```hcl

module "bigtable-instance" {
  source     = "./fabric/modules/bigtable-instance"
  project_id = "my-project"
  name       = "instance"
  cluster_id = "instance"
  zone       = "europe-west1-b"
  tables = {
    test1 = null,
    test2 = {
      split_keys    = ["a", "b", "c"]
      column_family = null
    }
  }
  iam = {
    "roles/bigtable.user" = ["user:viewer@testdomain.com"]
  }
}
# tftest modules=1 resources=4
```

### Instance with static number of nodes

If you are not using autoscaling settings, you must set a specific number of nodes with the variable `num_nodes`.

```hcl

module "bigtable-instance" {
  source     = "./fabric/modules/bigtable-instance"
  project_id = "my-project"
  name       = "instance"
  cluster_id = "instance"
  zone       = "europe-west1-b"
  num_nodes  = 5
}
# tftest modules=1 resources=1
```

### Instance with autoscaling (based on CPU only)

If you use autoscaling, you should not set the variable `num_nodes`.

```hcl

module "bigtable-instance" {
  source     = "./fabric/modules/bigtable-instance"
  project_id = "my-project"
  name       = "instance"
  cluster_id = "instance"
  zone       = "europe-southwest1-b"
  autoscaling_config = {
    min_nodes  = 3
    max_nodes  = 7
    cpu_target = 70
  }
}
# tftest modules=1 resources=1
```

### Instance with autoscaling (based on CPU and/or storage)

```hcl

module "bigtable-instance" {
  source       = "./fabric/modules/bigtable-instance"
  project_id   = "my-project"
  name         = "instance"
  cluster_id   = "instance"
  zone         = "europe-southwest1-a"
  storage_type = "SSD"
  autoscaling_config = {
    min_nodes      = 3
    max_nodes      = 7
    cpu_target     = 70
    storage_target = 4096
  }
}
# tftest modules=1 resources=1
```
<!-- BEGIN TFDOC -->

## Variables

| name | description | type | required | default |
|---|---|:---:|:---:|:---:|
| [name](variables.tf#L56) | The name of the Cloud Bigtable instance. | <code>string</code> | ✓ |  |
| [project_id](variables.tf#L67) | Id of the project where datasets will be created. | <code>string</code> | ✓ |  |
| [zone](variables.tf#L99) | The zone to create the Cloud Bigtable cluster in. | <code>string</code> | ✓ |  |
| [autoscaling_config](variables.tf#L17) | Settings for autoscaling of the instance. If you set this variable, the variable num_nodes is ignored. | <code title="object&#40;&#123;&#10;  min_nodes      &#61; number&#10;  max_nodes      &#61; number&#10;  cpu_target     &#61; number,&#10;  storage_target &#61; optional&#40;number, null&#41;&#10;&#125;&#41;">object&#40;&#123;&#8230;&#125;&#41;</code> |  | <code>null</code> |
| [cluster_id](variables.tf#L28) | The ID of the Cloud Bigtable cluster. | <code>string</code> |  | <code>&#34;europe-west1&#34;</code> |
| [deletion_protection](variables.tf#L34) | Whether or not to allow Terraform to destroy the instance. Unless this field is set to false in Terraform state, a terraform destroy or terraform apply that would delete the instance will fail. | <code></code> |  | <code>true</code> |
| [display_name](variables.tf#L39) | The human-readable display name of the Bigtable instance. | <code></code> |  | <code>null</code> |
| [iam](variables.tf#L44) | IAM bindings for topic in {ROLE => [MEMBERS]} format. | <code>map&#40;list&#40;string&#41;&#41;</code> |  | <code>&#123;&#125;</code> |
| [instance_type](variables.tf#L50) | (deprecated) The instance type to create. One of 'DEVELOPMENT' or 'PRODUCTION'. | <code>string</code> |  | <code>null</code> |
| [num_nodes](variables.tf#L61) | The number of nodes in your Cloud Bigtable cluster. This value is ignored if you are using autoscaling. | <code>number</code> |  | <code>1</code> |
| [storage_type](variables.tf#L72) | The storage type to use. | <code>string</code> |  | <code>&#34;SSD&#34;</code> |
| [table_options_defaults](variables.tf#L78) | Default option of tables created in the BigTable instance. | <code title="object&#40;&#123;&#10;  split_keys    &#61; list&#40;string&#41;&#10;  column_family &#61; string&#10;&#125;&#41;">object&#40;&#123;&#8230;&#125;&#41;</code> |  | <code title="&#123;&#10;  split_keys    &#61; &#91;&#93;&#10;  column_family &#61; null&#10;&#125;">&#123;&#8230;&#125;</code> |
| [tables](variables.tf#L90) | Tables to be created in the BigTable instance, options can be null. | <code title="map&#40;object&#40;&#123;&#10;  split_keys    &#61; list&#40;string&#41;&#10;  column_family &#61; string&#10;&#125;&#41;&#41;">map&#40;object&#40;&#123;&#8230;&#125;&#41;&#41;</code> |  | <code>&#123;&#125;</code> |

## Outputs

| name | description | sensitive |
|---|---|:---:|
| [id](outputs.tf#L17) | An identifier for the resource with format projects/{{project}}/instances/{{name}}. |  |
| [instance](outputs.tf#L26) | BigTable intance. |  |
| [table_ids](outputs.tf#L35) | Map of fully qualified table ids keyed by table name. |  |
| [tables](outputs.tf#L40) | Table resources. |  |

<!-- END TFDOC -->
