---
subcategory: "Bastion Host"
layout: "alicloud"
page_title: "Alicloud: alicloud_bastionhost_host_group_account_user_attachment"
sidebar_current: "docs-alicloud-resource-bastionhost-host-group-account-user-attachment"
description: |-
  Provides a Alicloud Bastion Host Host Group Account attachment resource.
---

# alicloud_bastionhost_host_group_account_user_attachment

Provides a Bastion Host Host Account Attachment resource to add list host accounts into one user and one host group.

-> **NOTE:** Available since v1.135.0.

## Example Usage

Basic Usage

```terraform
variable "name" {
  default = "tf_example"
}
data "alicloud_zones" "default" {
  available_resource_creation = "VSwitch"
}

resource "alicloud_vpc" "default" {
  vpc_name   = var.name
  cidr_block = "10.4.0.0/16"
}

resource "alicloud_vswitch" "default" {
  vswitch_name = var.name
  cidr_block   = "10.4.0.0/24"
  vpc_id       = alicloud_vpc.default.id
  zone_id      = data.alicloud_zones.default.zones.0.id
}

resource "alicloud_security_group" "default" {
  vpc_id = alicloud_vpc.default.id
}
resource "alicloud_bastionhost_instance" "default" {
  description        = var.name
  license_code       = "bhah_ent_50_asset"
  plan_code          = "cloudbastion"
  storage            = "5"
  bandwidth          = "5"
  period             = "1"
  vswitch_id         = alicloud_vswitch.default.id
  security_group_ids = [alicloud_security_group.default.id]
}

resource "alicloud_bastionhost_user" "local_user" {
  instance_id         = alicloud_bastionhost_instance.default.id
  mobile_country_code = "CN"
  mobile              = "13312345678"
  password            = "YourPassword-123"
  source              = "Local"
  user_name           = "${var.name}_local_user"
}

resource "alicloud_bastionhost_host" "default" {
  instance_id          = alicloud_bastionhost_instance.default.id
  host_name            = var.name
  active_address_type  = "Private"
  host_private_address = "172.16.0.10"
  os_type              = "Linux"
  source               = "Local"
}

resource "alicloud_bastionhost_host_account" "default" {
  host_account_name = var.name
  host_id           = alicloud_bastionhost_host.default.host_id
  instance_id       = alicloud_bastionhost_host.default.instance_id
  protocol_name     = "SSH"
  password          = "YourPassword12345"
}

resource "alicloud_bastionhost_host_group" "default" {
  host_group_name = var.name
  instance_id     = alicloud_bastionhost_instance.default.id
}

resource "alicloud_bastionhost_host_group_account_user_attachment" "default" {
  instance_id        = alicloud_bastionhost_host.default.instance_id
  user_id            = alicloud_bastionhost_user.local_user.user_id
  host_group_id      = alicloud_bastionhost_host_group.default.host_group_id
  host_account_names = [alicloud_bastionhost_host_account.default.host_account_name]
}
```

## Argument Reference

The following arguments are supported:

* `user_id` - (Required, ForceNew) The ID of the user that you want to authorize to manage the specified hosts and host accounts.
* `host_group_id` - (Required, ForceNew) The ID of the host group.
* `instance_id` - (Required, ForceNew) The ID of the Bastionhost instance where you want to authorize the user to manage the specified hosts and host accounts.
* `host_account_names` - (Required, List) A list names of the host account.

## Attributes Reference

The following attributes are exported:

* `id` - The resource ID of Host Account. The value formats as `<instance_id>:<user_id>:<host_group_id>`.

## Import

Bastion Host Host Account can be imported using the id, e.g.

```shell
$ terraform import alicloud_bastionhost_host_account.example <instance_id>:<user_id>:<host_group_id>
```
