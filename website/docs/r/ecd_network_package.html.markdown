---
subcategory: "Elastic Desktop Service (ECD)"
layout: "alicloud"
page_title: "Alicloud: alicloud_ecd_network_package"
sidebar_current: "docs-alicloud-resource-ecd-network-package"
description: |-
  Provides a Alicloud ECD Network Package resource.
---

# alicloud_ecd_network_package

Provides a ECD Network Package resource.

For information about ECD Network Package and how to use it, see [What is Network Package](https://www.alibabacloud.com/help/en/elastic-desktop-service/latest/api-doc-ecd-2020-09-30-api-doc-createnetworkpackage).

-> **NOTE:** Available since v1.142.0.

## Example Usage

Basic Usage

```terraform
variable "name" {
  default = "terraform-example"
}
resource "alicloud_ecd_simple_office_site" "default" {
  cidr_block          = "172.16.0.0/12"
  enable_admin_access = false
  desktop_access_type = "Internet"
  office_site_name    = var.name
}

resource "alicloud_ecd_network_package" "default" {
  bandwidth      = 10
  office_site_id = alicloud_ecd_simple_office_site.default.id
}
```

## Argument Reference

The following arguments are supported:

* `bandwidth` - (Required) The bandwidth of package public network bandwidth peak. Valid values: 1~200. Unit:Mbps.
* `internet_charge_type` - (Optional, ForceNew) The internet charge type  of  package.
* `office_site_id` - (Required, ForceNew) The ID of office site.

## Attributes Reference

The following attributes are exported:

* `id` - The resource ID in terraform of Network Package.
* `status` - The status of network package. Valid values: `Creating`, `InUse`, `Releasing`,`Released`.

## Import

ECD Network Package can be imported using the id, e.g.

```shell
$ terraform import alicloud_ecd_network_package.example <id>
```
