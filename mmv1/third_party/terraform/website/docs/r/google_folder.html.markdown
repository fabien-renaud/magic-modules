---
subcategory: "Cloud Platform"
page_title: "Google: google_folder"
description: |-
 Allows management of a Google Cloud Platform folder.
---

# google\_folder

Allows management of a Google Cloud Platform folder. For more information see 
[the official documentation](https://cloud.google.com/resource-manager/docs/creating-managing-folders)
and 
[API](https://cloud.google.com/resource-manager/reference/rest/v2/folders).

A folder can contain projects, other folders, or a combination of both. You can use folders to group projects under an organization in a hierarchy. For example, your organization might contain multiple departments, each with its own set of Cloud Platform resources. Folders allows you to group these resources on a per-department basis. Folders are used to group resources that share common IAM policies.

Folders created live inside an Organization. See the [Organization documentation](https://cloud.google.com/resource-manager/docs/quickstarts) for more details.

The service account used to run Terraform when creating a `google_folder`
resource must have `roles/resourcemanager.folderCreator`. See the
[Access Control for Folders Using IAM](https://cloud.google.com/resource-manager/docs/access-control-folders)
doc for more information.

## Example Usage

```hcl
# Top-level folder under an organization.
resource "google_folder" "department1" {
  display_name = "Department 1"
  parent       = "organizations/1234567"
}

# Folder nested under another folder.
resource "google_folder" "team-abc" {
  display_name = "Team ABC"
  parent       = google_folder.department1.name
}
```

## Argument Reference

The following arguments are supported:

* `display_name` - (Required) The folder’s display name.
    A folder’s display name must be unique amongst its siblings, e.g. no two folders with the same parent can share the same display name. The display name must start and end with a letter or digit, may contain letters, digits, spaces, hyphens and underscores and can be no longer than 30 characters. 

* `parent` - (Required) The resource name of the parent Folder or Organization.
    Must be of the form `folders/{folder_id}` or `organizations/{org_id}`.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are
exported:

* `name` - The resource name of the Folder. Its format is folders/{folder_id}.
* `lifecycle_state` - The lifecycle state of the folder such as `ACTIVE` or `DELETE_REQUESTED`.
* `create_time` - Timestamp when the Folder was created. Assigned by the server.
    A timestamp in RFC3339 UTC "Zulu" format, accurate to nanoseconds. Example: "2014-10-02T15:01:23.045123456Z".

## Import

Folders can be imported using the folder's id, e.g.

```
# Both syntaxes are valid
$ terraform import google_folder.department1 1234567
$ terraform import google_folder.department1 folders/1234567
```
