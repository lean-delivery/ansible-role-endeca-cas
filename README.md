endeca_cas
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-endeca-cas/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-endeca-cas.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-endeca-cas)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-endeca-cas/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-endeca-cas)

## Summary
--------------

This role installs Oracle Commerce Content Acquisition System (CAS) on Linux platforms which allows to add, configure, and crawl data sources for use in an Endeca application.


Requirements
--------------

 - Minimal Version of the ansible for installation: 2.5
 - **Supported CAS versions**:
   - 3.x.x
   - 11.x.x
   - _higher versions should be retested_
 - **Supported OS**:
   - CentOS
     - 6
     - 7

#### CentOS 7 is not supported OS per Oracle support matrix. Please use it at your own risk



For more information regarding support matrix please visit <https://support.oracle.com>

Oracle Endeca MDEX Engine, PlatformServices and Tools and Frameworks should be installed preliminarily:
  - lean_delivery.endeca_mdex
  - lean_delivery.endeca_platformservices
  - lean_delivery.endeca_toolsandframeworks

```
For test scenarios endeca_cas/requirements.yml is used  
If another roles/versions are required, put requirements.yml to molecule/<scenario_name> and remove in molecule.yml lines  
  options:  
    role-file: requirements.yml
```


Role Variables
--------------

  - `transport` - artifact source transport  
     available:
      - `web` - fetch artifact from custom web uri
      - `local` - local artifact

  - `transport_web` - URI for http/https artifact  e.g. "http://my-storage.example.com/V861198-01.zip"
  - `transport_local` - path for local artifact e.g. "/tmp/V861198-01.zip"

  - `download_path` - local folder for downloading artifacts  
    default: `/tmp`

  - `cas_version` - Endeca CAS version

#### Set CAS version as it's defined in official Oracle Documentation

  - `base_root` - where CAS should be installed  
    default: `/opt`

  - `cas_service` - name of the service  
    default: `endeca-cas`

  - `cas_port` - CAS port  
    default: `8500`

  - `cas_shutdown_port` - CAS shutdown port  
    default: `8506`

  - `cas_domain` - domain of the Endeca CAS server  
    default: `{{ ansible_fqdn }}`

  - `install_feature_list` - list of features for installing  
    available:  
      - `CAS` - Content Acquisition System
      - `Samples` - CAS Samples
      - `Console` - CAS Console as a Workbench Extension
      - `DT` - CAS Deployment Template Integration


Example Playbook
----------------

### Installing Endeca CAS 11.3.0 from local:
```yaml
- name: "Install CAS 11.3.0 from local"
  hosts: all

  roles:
    - role: lean_delivery.endeca_mdex
      transport: "local"
      transport_local: "/tmp/V861206-01.zip"
    - role: lean_delivery.endeca_platformservices
      transport: "local"
      transport_local: "/tmp/V861203-01.zip"
    - role: lean_delivery.endeca_toolsandframeworks
      transport: "local"
      transport_local: "/tmp/V861200-01.zip"
      workbench_password: "Admin123"
    - role: lean_delivery.endeca_cas
      cas_version: "11.3.0"
      transport: "local"
      transport_local: "/tmp/V861198-01.zip"
  vars:
    base_root: "/opt"
    mdex_version: "11.3.0"
    ps_version: "11.3.0"
    tf_version: "11.3.0"
```

### Installing Endeca CAS 11.3.0 from web with defined CAS domain:
```yaml
- name: "Install CAS 11.3.0 from web with defined CAS domain"
  hosts: all

  roles:
    - role: lean_delivery.endeca_mdex
      transport: "web"
      transport_web: "http://my-storage.example.com/V861206-01.zip"
    - role: lean_delivery.endeca_platformservices
      ps_version: "11.3.0"
      transport: "web"
      transport_web: "http://my-storage.example.com/V861203-01.zip"
    - role: lean_delivery.endeca_toolsandframeworks
      transport: "web"
      transport_web: "http://my-storage.example.com/V861200-01.zip"
      workbench_password: "Admin123"
    - role: lean_delivery.endeca_cas
      cas_version: "11.3.0"
      transport: "web"
      transport_web: "http://my-storage.example.com/V861198-01.zip"
      cas_domain: "host.example.com"
  vars:
    base_root: "/opt"
    mdex_version: "11.3.0"
    ps_version: "11.3.0"
    tf_version: "11.3.0"
```


## License

[Apache License 2.0](https://raw.githubusercontent.com/lean-delivery/ansible-role-endeca-cas/master/LICENSE)

## Authors

[Lean Delivery team](team@lean-delivery.com)
