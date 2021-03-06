Netmiko
=======

Multi-vendor library to simplify Paramiko SSH connections to network devices

Python 2.6, 2.7, 3.3, 3.4, 3.5  
  
<br>
#### Requires:
Paramiko >= 1.13+  
scp >= 0.10.0  
pyyaml  
pytest (for unit tests)   
  
  
<br>
#### Supports:
  
###### Regularly tested
Arista vEOS  
Cisco ASA  
Cisco IOS  
Cisco IOS-XE  
Cisco IOS-XR  
HP Comware7  
HP ProCurve  
Juniper Junos  
  
###### Limited testing
Avaya ERS  
Avaya VSP  
Brocade VDX  
Brocade ICX/FastIron  
Brocade MLX/NetIron  
Cisco NX-OS  
Cisco WLC  
Dell-Force10 DNOS9  
Huawei  
Linux  
Palo Alto PAN-OS  
  
###### Experimental
A10  
F5 LTM  
Enterasys  
Extreme  
Fortinet  
Alcatel-Lucent SR-OS  

   
<br>
## Tutorials:

##### Standard Tutorial: 
https://pynet.twb-tech.com/blog/automation/netmiko.html
  
##### SSH Proxy: 
https://pynet.twb-tech.com/blog/automation/netmiko-proxy.html
  
  
<br>
## Examples:

#### Create a dictionary representing the device.

Supported device_types can be found [here](https://github.com/ktbyers/netmiko/blob/master/netmiko/ssh_dispatcher.py), see CLASS_MAPPER keys.
```py
from netmiko import ConnectHandler

cisco_881 = {
    'device_type': 'cisco_ios',
    'ip':   '10.10.10.10',
    'username': 'test',
    'password': 'password',
    'port' : 8022,          # optional, defaults to 22
    'secret': 'secret',     # optional, defaults to ''
    'verbose': False,       # optional, defaults to False
}

```

<br>
#### Establish an SSH connection to the device by passing in the device dictionary.
```py
net_connect = ConnectHandler(**cisco_881)
```

<br>
#### Execute show commands.
```py
output = net_connect.send_command('show ip int brief')
print(output)
```
```
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0              unassigned      YES unset  down                  down    
FastEthernet1              unassigned      YES unset  down                  down    
FastEthernet2              unassigned      YES unset  down                  down    
FastEthernet3              unassigned      YES unset  down                  down    
FastEthernet4              10.10.10.10     YES manual up                    up      
Vlan1                      unassigned      YES unset  down                  down    
```

<br>
#### For long-running commands, use `send_command_expect()`

`send_command_expect` waits for the trailing prompt (or for an optional pattern)
```py
net_connect.send_command_expect('write memory')
```
```
Building configuration...
[OK]
```

<br>
#### Enter and exit enable mode.
```py
net_connect.enable()
net_connect.exit_enable_mode()
```

<br>
#### Execute configuration change commands (will automatically enter into config mode)
```py
config_commands = [ 'logging buffered 20000', 
                    'logging buffered 20010', 
                    'no logging console' ]
output = net_connect.send_config_set(config_commands)
print(output)
```
```
pynet-rtr1#config term
Enter configuration commands, one per line.  End with CNTL/Z.
pynet-rtr1(config)#logging buffered 20000
pynet-rtr1(config)#logging buffered 20010
pynet-rtr1(config)#no logging console
pynet-rtr1(config)#end
pynet-rtr1#
```
  
  
<br>
---    
Kirk Byers  
Python for Network Engineers  
https://pynet.twb-tech.com  
