# Design

## AWS Keys

The Ansible S3 module supports multiple mechanisms to authenticate with the S3 API. However, it is the opinion of the
playbook author that the authentication should be handled by the process or user invoking the playbook, and not the
domain of the machine executing the playbook.

Such an authentication model allows the provioning of fewer individual credentials, and easier reasonability about what
requires access.
