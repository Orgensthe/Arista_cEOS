# Arista_cEOS

## 1. Download cEOS image from Arista website and import into docker
```
[root@beta-control-01 Downloads]# docker import cEOS64-lab-4.25.1F.tar.xz  ceos:4.25.1F
sha256:a6e939129819fabf9fe58a92d8bbc95e4c6f169a68d12ad3d2d60a223fc0a49e
```
## 2. Create cEOS
```
[root@beta-control-01 Downloads]# docker create --name=ceos-r1 -p 4000:8080 -p 4001:6030 -p 4002:6061 -p 4003:8081 -p 4423:443 --privileged -e INTFTYPE=eth -e ETBA=4 -e SKIP_ZEROTOUCH_BARRIER_IN_SYSDBINIT=1 -e CEOS=1 -e EOS_PLATFORM=ceoslab -e container=docker -i -t ceos:4.25.1F /sbin/init systemd.setenv=INTFTYPE=eth systemd.setenv=ETBA=4 systemd.setenv=SKIP_ZEROTOUCH_BARRIER_IN_SYSDBINIT=1 systemd.setenv=CEOS=1 systemd.setenv=EOS_PLATFORM=ceoslab systemd.setenv=container=docker systemd.setenv=MAPETH0=1 systemd.setenv=MGMT_INTF=eth0
6e0b70df41787849aa4ad96ae4c7bb127e2ced0d0a2a34b7ddd0e19366292500
```
## 3. Create network and connet it
```
[root@beta-control-01 Downloads]# docker network create r1r2 
[root@beta-control-01 Downloads]# docker network connect r1r2 ceos-r1
```
## 4. Connect to container
```
[root@beta-control-01 Downloads]# docker exec -it ceos-r1 Cli
```

## 5. Verification
```
localhost(config)#show ver
 cEOSLab
Hardware version:
Serial number:
Hardware MAC address: 0242.ac22.bd71
System MAC address: 0242.ac22.bd71

Software image version: 4.25.1F-20005270.4251F (engineering build)
Architecture: x86_64
Internal build version: 4.25.1F-20005270.4251F
Internal build ID: 37a9ea6d-2c81-487d-9c86-579348318c0a

cEOS tools version: 1.1
Kernel version: 3.10.0-1127.el7.x86_64

Uptime: 0 weeks, 0 days, 0 hours and 0 minutes
Total memory: 263618792 kB
Free memory: 251252600 kB

localhost(config)#
```
