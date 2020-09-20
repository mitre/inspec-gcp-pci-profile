# GCP PCI DSS v3.2.1 Inspec Profile

This repository holds the [Google Cloud Platform (GCP)](https://cloud.google.com/) [PCI DSS v3.2.1](https://www.pcisecuritystandards.org/pci_security/) [Inspec](https://www.inspec.io/) Profile.

## Required Disclaimer

This is not an officially supported Google product. This code is intended to help users assess their security posture on the Google Cloud against the PCI-DSS requirements. This code is not certified by PCI-DSS.

## Usage

### Profile Inputs (see `inspec.yml` file)

This profile uses InSpec Inputs to make the tests more flexible. You are able to provide inputs at runtime either via the `cli` or via `YAML files` to help the profile work best in your deployment.

**pro tip**: Do not change the inputs in the `inspec.yml` file directly, either:

1. update them via the cli - via the `--input` flag
2. pass them in via a YAML file as shown in the `Example` - via the `--input-file` flag

Further details can be found here: <https://docs.chef.io/inspec/inputs/>

- **gcp_project_id** - (Default: "", type: string) - The target GCP Project that must be specified.
- **fw_change_control_id_regex** - (Default: 'CID:', type: string) - Non-GKE Firewall rules should have a description that matches this regex
- **fw_override_control_id_regex** - (Default: 'AID:', type: string) - Firewall rules that allow insecure protocols needing an override should have a description that matches this regex
- **cai_inventory_bucket_name** - (Default: "", type: string) - GCS Bucket name where the latest CAI export is stored
- **cai_inventory_file_path** - (Default: "", type: string) - File path/name where the latest CAI export is stored inside the cai_inventory_bucket_name GCS bucket.
- **gcs_pii_buckets** - (Default: "", type: list) - List of GCS buckets where PII is known to be stored.
- **kms_admins_list** - (Default: "", type: list) - List of accounts/users/groups that should have KMS admin permissions
- **kms_encrypters_list** - (Default: "", type: list) - List of accounts/users/groups that should have KMS encrypters permissions
- **kms_decrypters_list** - (Default: "", type: list) - List of accounts/users/groups that should have KMS decrypters permissions
- **kms_encrypterdecrypters_list** - (Default: "", type: list) - List of accounts/users/groups that should have KMS encrypter/decrypter permissions
- **kms_regions_list** - (Default: "", type: list) - List of GCP regions ("global" is valid) where KMS keyrings can be present.

### CLI Example

```
$ cat inputs.yml
gcp_project_id: "my-project-id"
fw_change_control_id_regex: 'CID:'
fw_override_control_id_regex: 'AID:'
cai_inventory_bucket_name: "my-inventory-bucket-name"
cai_inventory_file_path: "my-inventory-file-path"
gcs_pii_buckets:
  - "my-pii-bucket-name1"
  - "my-pii-bucket-name2"
kms_admins_list:
  - "serviceAccount:sa1@my-project-id.iam.gserviceaccount.com"
kms_encrypters_list:
  - "serviceAccount:sa2@my-project-id.iam.gserviceaccount.com"
kms_decrypters_list:
  - "serviceAccount:sa3@my-project-id.iam.gserviceaccount.com"
kms_encrypterdecrypters_list:
  - "serviceAccount:sa4@my-project-id.iam.gserviceaccount.com"
kms_regions_list:
  - "us-central1"
  - "global"
```

Example run:

```
# scan a project with this profile, replace <YOUR_PROJECT_ID> with your project ID
$ CHEF_LICENSE=accept-no-persist inspec exec https://github.com/GoogleCloudPlatform/inspec-gcp-pci-profile -t gcp:// --input gcp_project_id=<YOUR_PROJECT_ID> --reporter cli json:myscan.json
```
