### CNI

CNI or 'Container Networking Interface' means two things:
1) A specification 
2) A set of reference network plugins

#### Specification

CNI is basically considered a specification - a JSON text structure. This text format is considered to be universal interface for 3rd party network plugins to work with orchestration solution like kubernetes, mesos, cloudfoundry etc. 

This specification can be used to connect vm/container/namespace to the network.

A sample format is the following:  
```
{
  "cniVersion": "1.0.0",
  "name": "dbnet",
  "plugins": [
    {
      "type": "bridge",
      // plugin specific parameters
      "bridge": "cni0",
      "keyA": ["some more", "plugin specific", "configuration"],
      
      "ipam": {
        "type": "host-local",
        // ipam specific
        "subnet": "10.1.0.0/16",
        "gateway": "10.1.0.1",
        "routes": [
            {"dst": "0.0.0.0/0"}
        ]
      },
      "dns": {
        "nameservers": [ "10.1.0.1" ]
      }
    },
    {
      "type": "tuning",
      "capabilities": {
        "mac": true,
      },
      "sysctl": {
        "net.core.somaxconn": "500"
      }
    },
    {
        "type": "portmap",
        "capabilities": {"portMappings": true}
    }
  ]
}
```  

This format has required parameters -
- cniVersion
- name
- type 
 
The rest are based on the plugin.   

As of today only two versions of the specification is in use - 0.4.0 and the newer 1.0.0.   

Further details are available in https://www.cni.dev/docs/spec/ 

#### Plugins
CNI also provides a set of reference plugins like bridge, ipvlan, ipam etc
These are the default plugins avalable from CNI and can be used by other 3rdparty plugins 
There are basically 2 types of plugins:  
- Interface plugin
- Chained plugin


Interface plugin create the network interface in the container and attach it to the network.
The Chained plugins are responsible for making additional configuration to the interface already created.

Plugins can be called one after another to achieve specific capability of the required plugin. Eg. some plugins can create multiple interfaces, some can configure IPv6.

Further details on the reference plugins are available in https://www.cni.dev/plugins/current/

