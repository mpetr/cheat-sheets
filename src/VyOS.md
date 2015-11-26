# EdgeOS

Last modified: 2015-11-26

## Querying system information

| Action             | Command                        |
| :---               | :---                           |
| IP configuration   | `show interfaces`              |
| Routing            | `show ip route`                |
| Show configuration | `show`                         |
| Show log           | `show log tail`                |

## CLI Modes

* operational mode (prompt `$`): view system status
* configuration mode (prompt `#`): modify system configuration

In configuration mode, you can execute "operational" commands by preceding them with `run`.

## Basic configuration

Workflow:

```
admin@edge $ configure
admin@edge # [configuration commands]
admin@edge # commit
admin@edge # save
admin@edge # exit
admin@edge $
```

| Action              | Command                                          |
| :---                | :---                                             |
| Set host name       | `set system host-name HOSTNAME`                  |
| Set default gateway | `set system gateway-address 192.168.0.1`         |
| Set DNS server      | `set system name-server 8.8.8.8`                 |
| Set time zone       | `set system time-zone [TAB]`                     |

[^1]: Use in non-config mode

## Configuring network interfaces

| Action                               | Command                                               |
| :---                                 | :---                                                  |
| Run "normal" commands in config mode | `run COMMAND`                                         |
| Set IP address on interface          | `set interfaces ethernet eth0 address 192.168.0.1/24` |
| Run DHCP client on interface         | `set interfaces ethernet eth0 address dhcp`           |
| Set interface description            | `set interfaces ethernet eth0 description WAN`        |

## Static routing

| Action            | Command                                                                  |
| :---              | :---                                                                     |
| Add route         | `set protocols static route 192.168.0.0/24 next-hop 10.0.0.1 distance 1` |
| Set default route | `set protocols static route 0.0.0.0/0 next-hop 10.0.2.2 distance 1`      |
| Drop traffic      | `set protocols static route 172.16.0.0/12 blackhole distance '254'`      |

## RIP

Example with two directly connected networks:

```
# set protocols rip network 192.168.0.0/24
# set protocols rip network 192.168.1.0/24
# set protocols rip redistribute connected
```

## Network Address Translation (NAT)

The following example adds a NAT rule with id 100 for a router with its WAN port on `eth0`. All IP addresses on the internal network 192.168.0.0/24 are translated into the router's IP address on `eth0`.

```
# set nat source rule 100 outbound-interface 'eth0'
# set nat source rule 100 source address '192.168.0.0/24'
# set nat source rule 100 translation address 'masquerade'
```


## Resources

* [User Guide](https://dl.ubnt.com/guides/edgemax/EdgeOS_UG.pdf)
* [Knowledgebase](http://community.ubnt.com/t5/EdgeMAX-CLI-Basics-Knowledge/tkb-p/CLI_Basics%40tkb)
* [Videos](https://www.youtube.com/playlist?list=PLqmQzXAOhOQgIrI7s3CIKGGGrYPIdq1J-)
* [Unofficial config tree](http://plonetest.univ-pau.fr/doc/folder.2006-12-12.6464510577/folder.2006-12-16.8029221739/Vyatta_ConfigGuide_VC3_v02.pdf)

