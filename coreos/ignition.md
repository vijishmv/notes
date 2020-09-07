# Ignition File #
To create a coreos node, first we need to create an ignition configuration file.
This is a JSON file. that is input to coreos during installation. 

This file is created by generating a basic yaml file called FCC (Fedora CoreOS Configuration) file. This yaml file is then input to a tool called 'FCCT' (FCOS Configuration Transpiler) that produces the JSON based ignition file. 

The simplest ignition file only has the ssh public key. coreos only permits ssh using keys on bootup. It accepts IP via DHCP which is displayed in the KVM console after bootup. 

## Example of FCC file with SSH public key ##
```
variant: fcos
version: 1.1.0
passwd:
  users:
    - name: core
      ssh_authorized_keys:
        - ssh-rsa AAAA...
```

Easy way to create the ignition JSON file is by using fcct tool from a container:
```
podman pull quay.io/coreos/fcct:release
podman run -i --rm quay.io/coreos/fcct:release --pretty --strict < example.fcc > example.ign
```

### References ###
https://docs.fedoraproject.org/en-US/fedora-coreos/producing-ign/  
https://docs.fedoraproject.org/en-US/fedora-coreos/static-ip-config/  
https://github.com/coreos/ignition/blob/master/doc/examples.md  

