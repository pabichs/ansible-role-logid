logid
=========

Ansible role to install https://github.com/PixlOne/logiops - driver for Logitech mice.

Requirements
------------

None.

Role Variables
--------------

| Variable               | Required | Default                               | Description                                   |
|------------------------|----------|---------------------------------------|-----------------------------------------------|
| logid_config           | yes      | _undefined_                           | Config for the `logid`                        |
| logid_version          | no       | "v0.3.3"                              | Version of `logid` to install                 |
| logid_dir              | no       | "/opt/logid"                          | Path to installation dir                      |
| logid_cmake_cmd        | no       | "cmake -DDMAKE_BUILD_TYPE=Release .." | Cmake command to build                        |
| logid_service_enabled  | no       | true                                  | Controls if `logid` service should be enabled |
| logid_service_state    | no       | "started"                             | Controls `logid` service state                |     
| logid_config_owner     | no       | _undefined_                           | Sets owner of the config file for `logid`     | 
| logid_config_group     | no       | _undefined_                           | Sets group of the config file for `logid`     | 
| logid_config_mode      | no       | _undefined_                           | Sets mode of the config file for `logid`      | 



Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: localhost
  become: true
  roles:
    - role: pabichs.logid
      vars:
        logid_config: |
          devices: (
              {
                name: "MX Master 3 for Mac";
              }
          )
```

```yaml
- hosts: localhost
  become: true
  roles:
    - role: pabichs.logid
      vars:
        logid_version: "v0.2.4"
        logid_cmake_cmd: "cmake .."
        logid_config: |
          devices: (
            {
              name: "MX Master 3 for Mac";
              smartshift: {
                on: true;
                threshold: 50;
              };
              hiresscroll: {
                hires: true;
                invert: false;
                target: false;
              };
              thumbwheel: {
                invert: false;
              };
              dpi: 1500; // max=4000
            
              buttons: (
                // Forward button
                {
                  cid: 0x56;
                  action = {
                    type: "Gestures";
                    gestures: (
                      {
                        direction: "None";
                        mode: "OnRelease";
                        action = {
                          type: "Keypress";
                          keys: [ "KEY_BACK" ];
                        }
                      }
                    );
                  };
                },
            
                // Back button
                {
                  cid: 0x53;
                  action = {
                    type: "Gestures";
                    gestures: (
                      {
                        direction: "None";
                        mode: "OnRelease";
                        action = {
                          type: "Keypress";
                          keys: [ "KEY_FORWARD" ];
                        }
                      }
                    );
                  };
                },
            
                {
                  cid: 0xc3;
                  action = {
                    type: "Keypress";
                    keys: ["KEY_LEFTCTRL", "KEY_TAB"];
                  };
                },
                // Top button
                {
                  cid: 0xc4;
                  action = {
                    type: "Gestures";
                    gestures: (
                      {
                        direction: "None";
                        mode: "OnRelease";
                        action = {
                          type: "ToggleSmartShift";
                        }
                      },
            
                      {
                        direction: "Up";
                        mode: "OnRelease";
                        action = {
                          type: "ChangeDPI";
                          inc: 500,
                        }
                      },
            
                      {
                        direction: "Down";
                        mode: "OnRelease";
                        action = {
                          type: "ChangeDPI";
                          inc: -500,
                        }
                      }
                    );
                  };
                }
              );
            }
            );
```

License
-------

BSD, MIT

Author Information
------------------

Created in 2023
