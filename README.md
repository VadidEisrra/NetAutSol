# Changing network configuration

In this assigment I use the data models from the previous module to generate, deploy and validate network infrastructure and services.

## Infrastructure

- device_underlay.yml generates full candidate configuration
- underlay_diff.yml produces a diff of candidate file and configuration on the device
- underlay_deploy.yml commits the changes and displays the commit diff
- underlay_validate.yml validates LLDP neighbors using dynamically generated validation files from link data defined in hostvars


## Services

- generate_service_model.yml produces json blob containing all data required for end to end l2vpn configuration
- generate_service_config.yml produces endpoint configuration snippets for the service
- service_deploy.yml takes a customer ID and deploys the service. Using the _state_ parameter in the _playbooks/services.yml_ definition file, this can either provision the circuit or completely remove it
- service_validate.yml takes a circuit ID and displays the operational state of each endpoint
