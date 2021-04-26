# Kolla Configuration

## Core Services

```
###################
# OpenStack options
###################

# Enable Glance, Keystone, Neutron, Nova, Heat, Horizon
enable_openstack_core: "yes"
enable_haproxy: "yes"
enable_mariadb: "yes"
enable_memcached: "yes"
```

## Optional Services

```
############################
# OpenStack optional options
############################
enable_chrony: "yes"
enable_cinder: "yes"

```