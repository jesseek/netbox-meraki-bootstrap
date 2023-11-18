# netbox-meraki-bootstrap
Ansible Playbooks for Bootstrapping Meraki Device's into Netbox


## Requirements
- A working Netbox instance (tested on Netbox 3.3.9)
- A Netbox API Key
- A workstation with Ansible installed (tested with Ansible 2.12.10 and Python 3.8.10)
- Ansible Galaxy Netbox and Meraki Collections installed
    ```
    ansible-galaxy collection install netbox.netbox
    ansible-galaxy collection install cisco.meraki
    ```
- A Meraki account with at least 1 network created and 1 device added to that network
- A Meraki account API key

## Basic Configuration

Update 'vars' section of bootstrap_* Ansible playbooks with valid information.
```
  vars:
    netbox_url: ''  # Url for netbox instance
    netbox_token: ''  # Netbox Token
    netbox_validate_certs: false  # Recommended to be true for production
    netbox_site_name: '' # The netbox site name that you want to add the Meraki Devices To
    meraki_api_key: '' # Meraki API Key
    meraki_org_name: '' # Meraki Organization Name
    meraki_network_name: '' # Meraki Network Name that you want to pull the Meraki Device Info From
```

## Running the bootstrap_prep_netbox.yml Ansible playbook

This playbook generates and adds your Meraki `base data` to Netbox in preparation for
adding your Meraki devices to Netbox. This playbook should be run prior to running
bootstrap_meraki_netbox.yml. 
### `base data` includes:
    - Netbox Manufacturer: Meraki
    - Netbox Platform: Meraki
    - Netbox Device Roles: Maps your Meraki productType's to Netbox Device Role's
    - Netbox Device Types: Maps your Meraki model's to Netbox Device Type's
Usage:
    ansible-playbook bootstrap_prep_netbox.yml

## Running the bootstrap_meraki_netbox.yml Ansible playbook

This playbook adds your Meraki devices to Netbox.  
Includes:
    Add devices for specified Meraki network to specified Netbox site
    Creates a Management interface for the device in Netbox
    If the Meraki device is configured with a static IP
        1) Creates the IP Entry in Netbox
        2) Assigns the IP as the primary IP on the Netbox device
Usage:
    ansible-playbook bootstrap_prep_netbox.yml
## License

This sample application is distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
