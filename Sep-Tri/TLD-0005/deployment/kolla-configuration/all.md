# Kolla Configuration

## Kolla options

* **config_strategy:** Passing configuration file vào trong container (`KOLLA_CONFIG_STRATEGY`, `/var/lib/kolla/config_files/config.json`).
  * **COPY_ALWAYS (*):** Config sẽ được đồng bộ từ host vào container mỗi khi start container, nếu thay đổi config ở host thì config sẽ được apply vào container mỗi khi start.
  * **COPY_ONCE:** Config chỉ được copy vào trong container ở lần start đầu tiên. Khi container start ở lần thứ 2 trở đi sẽ không bị ảnh hưởng bởi config khi thay đổi từ host.

* **kolla_base_distro:** Lựa chọn Linux distro của container.
  * **centos (*)**
  * **debian**
  * **rhel**
  * **ubuntu**

* **kolla_install_type:** Lựa chọn kiểu cài đặt cho OpenStack.
  * **binary (*):** Sử dụng repo như apt, yum, dnf, ..
  * **source:** Sử dụng raw source như git, local source.

* **openstack_release:** Chọn phiên bản OpenStack.
  * **master (*):** Bản mới và ổn định nhất.
  * **victoria**
  * **ussuri**
  * ...

* **openstack_tag:** Chọn Docker image tag
  * `{{ openstack_release ~ openstack_tag_suffix }}"`

* **openstack_tag_suffix**

* **node_custom_config:** Location of Openstack config.
  * `/etc/kolla/config`

* **kolla_internal_vip_address:** VIP dùng cho OpenStack cluster. IP này sẽ floating giữa các OPS hosts đang keepalived để HA (ví dụ như giao tiếp API giữa các OPS Services).

* **kolla_internal_fqdn:** DNS name được map với `kolla_internal_vip_address`.
  * `{{ kolla_internal_vip_address }}`

* **kolla_external_vip_address:** VIP dùng cho OpenStack public. IP này sẽ floating giữa các OPS hosts đang keepalived để HA (ví dụ như user connect vào Monitor, Horizon).
  * `{{ kolla_internal_vip_address }}`

* **kolla_external_fqdn:** DNS name được map với `kolla_external_vip_address`.
  * `{{ kolla_external_vip_address }}`

* **kolla_sysctl_conf_path:** Optionally change the path to sysctl.conf modified by Kolla Ansible plays.
  * `/etc/sysctl.conf`

## Docker options

* **docker_registry:** URL của docker registry để sử dụng cho Kolla images.

* **docker_registry_insecure:** Enable flag `--insecure-registry`.
  * `{{ 'yes' if docker_registry else 'no' }}`

* **docker_registry_username:** Username của docker registry. `docker_registry_password` được set trong `passwords.yml`.

* **docker_namespace:** Namespace của Kolla images.
  * `kolla`

* **docker_client_timeout:**
  * `120`

* **docker_configure_for_zun:** Enable để sử dụng Zun quản lý các Docker.
  * `no (*)`
  * `yes`

* **containerd_configure_for_zun:** Enable để sử dụng Zun quản lý các Containerd.
  * `no (*)`
  * `yes`

* **containerd_grpc_gid**

## Messaging options

* **om_rpc_transport:** Chọn giao thức RPC.
  * `amqp (*)`: Sử dụng lib AMQP của Openstack.
  * `rabbit`: Sử dụng RabbitMQ làm Message Broker.
  * `no`: Tắt RPC.

* **om_rpc_user:** User để xác thực với RPC.
  * `{{ qdrouterd_user }}`

* **om_rpc_password:** Password để xác thực với RPC.
  * `{{ qdrouterd_password }}`

* **om_rpc_port:** Port để giao tiếp qua RPC.
  * `{{ qdrouterd_port }}`

* **om_rpc_group:** ...
  * `qdrouterd`

* **om_enable_rabbitmq_tls:** Enable TLS khi giao tiếp qua RabbitMQ.
  * `{{ rabbitmq_enable_tls | bool }}`

* **om_rabbitmq_cacert:** CA Cert của TLS khi giao tiếp qua RabbitMQ.
  * `{{ rabbitmq_cacert }}`

## Neutron - Networking Options

### 1. Interface configurations

* **network_interface:** Interface mặc định sẽ được gán cho cả hạ tầng network liên quan đến OpenStack, cũng như gán cho 2 "chân" external và internal nếu các biến interface này không được set giá trị.

* **kolla_external_vip_interface:** Interface của "chân" external, được dùng khi muốn cấu hình HAProxy public endpoint ra ngoài. 
  * `{{ network_interface }}`

* **api_interface:** Interface được dùng cho management network, là "đường" mà các OpenStack services giao tiếp với nhau và database bằng API.
  * `{{ network_interface }}`

* **storage_interface:** Interface này được dùng cho các storage traffic.
  * `{{ network_interface }}`

* **swift_storage_interface:** Interface này được dùng bởi Swift cho các object storage traffic.
  * `{{ storage_interface }}`

* **swift_replication_interface:** Interface này được dùng bởi Swift cho các storage replication traffic.
  * `{{ swift_storage_interface }}`

* **tunnel_interface:** Interface này được dùng bởi Neutron cho vm-to-vm traffic qua các tunneled networks (như VXLAN).
  * `{{ network_interface  }}`

* **dns_interface:** Interface này được dùng bởi Designate và Bind9. Được sử dụng cho các DNS public requests và queries cho Bind9 và Designate mDNS services.
  * `{{ network_interface  }}`
  
* **octavia_network_interface:** Interface này được dùng bởi Octavia. Được sử dụng cho LBaaS public endpoint.
  * `{{ api_interface }}`

### 2. AF (address family) network configurations

* **network_address_family:** Cấu hình AF network mặc định.
  * `ipv4 (*)`
  * `ipv6`

* **api_address_family:** Cấu hình AF network cho API interface.
  * `{{ network_address_family }}`

* **storage_address_family:** Cấu hình AF network cho Storage interface.
  * `{{ network_address_family }}`

* **swift_storage_address_family:** Cấu hình AF network cho Swift storage interface.
  * `{{ storage_address_family }}`

* **swift_replication_address_family:** Cấu hình AF network cho Swift replication storage interface.
  * `{{ swift_storage_address_family }}`

* **migration_address_family:** Cấu hình AF network khi thực hiện live migration của các instances.
  * `{{ api_address_family }}`

* **tunnel_address_family:** Cấu hình AF network cho Tunneled network interface.
  * `{{ network_address_family }}`

* **octavia_network_address_family:** Cấu hình AF network cho Octavia network interface.
  * `{{ api_address_family }}`

* **bifrost_network_address_family:** Cấu hình AF network cho Bifrost network interface.
  * `{{ network_address_family }}`

* **dns_address_family:** Cấu hình AF network cho DNS network interface.
  * `{{ network_address_family }}`

### 3. Other network interface configurations

* **neutron_external_interface:** Interface được sử dụng bởi Neutron, giúp connect giữa VM và external/public network.
  
* **neutron_plugin_agent:** Cấu hình cách tổ chức mạng của Neutron.
  * `openvswitch (*)`
  * `ovn`
  * `linuxbridge`
  * `vmware_nsxv`
  * `vmware_nsxv3`
  * `vmware_dvs`

* **neutron_ipam_driver:** Cấu hình IPAM driver cho Neutron.
  * `internal (*)`
  * `infoblox`

* **neutron_enable_rolling_upgrade:** Cấu hình upgrade option cho Neutron.
  * `yes (*)`: Sử dụng rolling_upgrade.
  * `no`: Sử dụng legacy_upgrade.

## keepalived options

* **keepalived_virtual_router_id:** Sử dụng cho việc "giữ liên lạc" khi cấu hình multi-region (tức là có nhiều VIPs khác nhau).
  * Unique number từ 0 đến 255.

## Dimension options

> Cấu hình này cung cấp các option để deploy containers với các "ràng buộc" về resource.  
> Tham khảo thêm tại: https://docs.docker.com/config/containers/resource_constraints/  
> Ví dụ:

* `<container_name>_dimensions:`
  * `blkio_weight:`
  * `cpu_period:`
  * `cpu_quota:`
  * `cpu_shares:`
  * `cpuset_cpus:`
  * `cpuset_mems:`
  * `mem_limit:`
  * `mem_reservation:`
  * `memswap_limit:`
  * `kernel_memory:`
  * `ulimits:`

## Healthcheck options

* **enable_container_healthchecks:** Enable healchecks giữa các Docker containers.
  * `yes (*)`
  * `no`

* **default_container_healthcheck_interval:** Đơn vị giây (s).
  * `30 (*)`

* **default_container_healthcheck_timeout:** Đơn vị giây (s).
  * `30 (*)`

* **default_container_healthcheck_retries:** Số lần thử lại.
  * `3 (*)`

* **default_container_healthcheck_start_period:** Chu kì.
  * `5 (*)`

## TLS options

* **kolla_enable_tls_internal:** Cấu hình TLS cho internal interface.
  * `no (*)`

* **kolla_enable_tls_external:** Cấu hình TLS cho external interface.
  * `{{ kolla_enable_tls_internal if kolla_same_external_internal_vip | bool else 'no' }}`

* **kolla_certificates_dir:** Vị trí của certificates khi bật TLS.
  * `{{ node_config }}/certificates`

* **kolla_external_fqdn_cert:** Vị trí FQDN cert của external interface.
  * `{{ kolla_certificates_dir }}/haproxy.pem`

* **kolla_internal_fqdn_cert:** Vị trí FQDN cert của internal interface.
  * `{{ kolla_certificates_dir }}/haproxy-internal.pem`

* **kolla_admin_openrc_cacert:** Admin OpenRC CA cert.

* **kolla_copy_ca_into_containers:** Cấu hình cho phép copy CA Cert vào các containers.
  * `no (*)`

* **haproxy_backend_cacert:**
  * `{{ 'ca-certificates.crt' if kolla_base_distro in ['debian', 'ubuntu'] else 'ca-bundle.trust.crt' }}`

* **haproxy_backend_cacert_dir:**
  * `/etc/ssl/certs`

## Backend options

* **kolla_httpd_keep_alive:**
  * `60 (*)`

* **kolla_httpd_timeout:**
  * `60 (*)`

## Backend TLS options

* **kolla_enable_tls_backend:** Enable TLS cho các backend phía sau HAproxy.
  * `no (*)`

* **kolla_verify_tls_backend:** Xác thực TLS backend.
  * `yes (*)`

* **kolla_tls_backend_cert:** 
  * `{{ kolla_certificates_dir }}/backend-cert.pem`

* **kolla_tls_backend_key:**
  * `{{ kolla_certificates_dir }}/backend-key.pem`

## ACME client options

* **acme_client_servers:** ...

## Region options

* **openstack_region_name:** Tên region của OpenStack cluster.

* **multiple_regions_names:** List các regions được cấu hình trong multi-region deployment.
  * `["{{ openstack_region_name }}"]`

## OpenStack options

* **openstack_logging_debug:** Enable logging debug cho OpenStack.
  * `False (*)`
  * `True`

* **enable_openstack_core:** Enable các core OpenStack services. Gồm: Glance, Keystone, Neutron, Nova, Heat, Horizon.
  * `yes (*)`

### 1. Required OpenStack services configurations

* **enable_glance:**
  *  `{{ enable_openstack_core | bool }}`

* **enable_hacluster:**
  *  `no (*)`
  
* **enable_haproxy:**
  * `yes (*)`

* **enable_keepalived:**
  * `{{ enable_haproxy | bool }}`

* **enable_keystone:**
  * `{{ enable_openstack_core | bool }}`

* **enable_mariadb:**
  * `yes (*)`

* **enable_memcached:** 
  * `yes (*)`

* **enable_neutron:**
  * `{{ enable_openstack_core | bool }}`

* **enable_nova:**
  * `{{ enable_openstack_core | bool }}`
 
* **enable_rabbitmq:**
  * `{{ 'yes' if om_rpc_transport == 'rabbit' or om_notify_transport == 'rabbit' else 'no' }}`

* **enable_outward_rabbitmq:**
  * `{{ enable_murano | bool }}`

### 2. Optional OpenStack services configurations

* **enable_aodh:**
  * `no`
* **enable_barbican:**
  * `no`
* **enable_blazar:**
  * `no`
* **enable_ceilometer:**
  * `no`
* **enable_ceilometer_ipmi:**
  * `no`
* **enable_cells:**
  * `no`
* **enable_central_logging:**
  * `no`
* **enable_chrony:**
  * `no`
* **enable_cinder:**
  * `no`
* **enable_cinder_backup:**
  * `yes`
* **enable_cinder_backend_hnas_nfs:**
  * `no`
* **enable_cinder_backend_iscsi:**
  * `{{ enable_cinder_backend_lvm | bool or enable_cinder_backend_zfssa_iscsi | bool }}`
* **enable_cinder_backend_lvm:**
  * `no`
* **enable_cinder_backend_nfs:**
  * `no`
* **enable_cinder_backend_zfssa_iscsi:**
  * `no`
* **enable_cinder_backend_quobyte:**
  * `no`
* **enable_cloudkitty:**
  * `no`
* **enable_collectd:**
  * `no`
* **enable_cyborg:**
  * `no`
* **enable_designate:**
  * `no`
* **enable_destroy_images:**
  * `no`
* **enable_elasticsearch:**
  * `{{ 'yes' if enable_central_logging | bool or enable_osprofiler | bool or enable_skydive | bool or enable_monasca | bool or (enable_cloudkitty | bool and cloudkitty_storage_backend == 'elasticsearch') else 'no' }}`
* **enable_elasticsearch_curator:**
  * `no`
* **enable_etcd:**
  * `no`
* **enable_fluentd:**
  * `yes`
* **enable_freezer:**
  * `no`
* **enable_gnocchi:**
  * `no`
* **enable_gnocchi_statsd:**
  * `no`
* **enable_grafana:**
  * `no`
* **enable_heat:**
  * `{{ enable_openstack_core | bool }}`
* **enable_horizon:**
  * `{{ enable_openstack_core | bool }}`
* **enable_horizon_blazar:**
  * `{{ enable_blazar | bool }}`
* **enable_horizon_cloudkitty:**
  * `{{ enable_cloudkitty | bool }}`
* **enable_horizon_designate:**
  * `{{ enable_designate | bool }}`
* **enable_horizon_freezer:**
  * `{{ enable_freezer | bool }}`
* **enable_horizon_heat:**
  * `{{ enable_heat | bool }}`
* **enable_horizon_ironic:**
  * `{{ enable_ironic | bool }}`
* **enable_horizon_magnum:**
  * `{{ enable_magnum | bool }}`
* **enable_horizon_manila:**
  * `{{ enable_manila | bool }}`
* **enable_horizon_masakari:**
  * `{{ enable_masakari | bool }}`
* **enable_horizon_mistral:**
  * `{{ enable_mistral | bool }}`
* **enable_horizon_monasca:**
  * `{{ enable_monasca | bool }}`
* **enable_horizon_murano:**
  * `{{ enable_murano | bool }}`
* **enable_horizon_neutron_vpnaas:**
  * `{{ enable_neutron_vpnaas | bool }}`
* **enable_horizon_octavia:**
  * `{{ enable_octavia | bool }}`
* **enable_horizon_sahara:**
  * `{{ enable_sahara | bool }}`
* **enable_horizon_senlin:**
  * `{{ enable_senlin | bool }}`
* **enable_horizon_solum:**
  * `{{ enable_solum | bool }}`
* **enable_horizon_tacker:**
  * `{{ enable_tacker | bool }}`
* **enable_horizon_trove:**
  * `{{ enable_trove | bool }}`
* **enable_horizon_vitrage:**
  * `{{ enable_vitrage | bool }}`
* **enable_horizon_watcher:**
  * `{{ enable_watcher | bool }}`
* **enable_horizon_zun:** 
    `{{ enable_zun | bool }}`
* **enable_influxdb:** 
    `{{ enable_monasca | bool or (enable_cloudkitty | bool and cloudkitty_storage_backend == 'influxdb') }}`
* **enable_ironic:**
  * `no`
* **enable_ironic_ipxe:**
  * `no`
* **enable_ironic_neutron_agent:**
  * `{{ enable_neutron | bool and enable_ironic | bool }}`
* **enable_ironic_pxe_uefi:**
  * `no`
* **enable_iscsid:**
  * `{{ (enable_cinder | bool and enable_cinder_backend_iscsi | bool) or enable_ironic | bool }}`
* **enable_kafka:**
  * `{{ enable_monasca | bool }}`
* **enable_kibana:**
  * `{{ 'yes' if enable_central_logging | bool or enable_monasca | bool else 'no' }}`
* **enable_kuryr:**
  * `no`
* **enable_magnum:**
  * `no`
* **enable_manila:** 
  * `no`
* **enable_manila_backend_generic:**
  * `no`
* **enable_manila_backend_hnas:**
  * `no`
* **enable_manila_backend_cephfs_native:**
  * `no`
* **enable_manila_backend_cephfs_nfs:**
  * `no`
* **enable_manila_backend_glusterfs_nfs:**
  * `no`
* **enable_mariabackup:** 
  * `no`
* **enable_masakari:**
  * `no`
* **enable_mistral:**
  * `no`
* **enable_monasca:**
  * `no`
* **enable_multipathd:**
  * `no`
* **enable_murano:**
  * `no`
* **enable_neutron_vpnaas:**
  * `no`
* **enable_neutron_sriov:**
  * `no`
* **enable_neutron_dvr:**
  * `no`
* **enable_neutron_qos:**
  * `no`
* **enable_neutron_agent_ha:**
  * `no`
* **enable_neutron_bgp_dragent:** 
    `no`
* **enable_neutron_provider_networks:**
  * `no`
* **enable_neutron_segments:** 
  * `no`
* **enable_neutron_sfc:**
  * `no`
* **enable_neutron_trunk:**
  * `no`
* **enable_neutron_metering:**
  * `no`
* **enable_neutron_infoblox_ipam_agent:**
  * `no`
* **enable_neutron_port_forwarding:**
  * `no`
* **enable_nova_serialconsole_proxy:**
  * `no`
* **enable_nova_ssh:**
  * `yes`
* **enable_octavia:**
  * `no`
* **enable_octavia_driver_agent:**
  * `{{ enable_octavia | bool and neutron_plugin_agent == 'ovn' }}`
* **enable_openvswitch:**
  * `{{ enable_neutron | bool and neutron_plugin_agent != 'linuxbridge' }}`
* **enable_ovn:**
  * `{{ enable_neutron | bool and neutron_plugin_agent == 'ovn' }}`
* **enable_ovs_dpdk:**
  * `no`
* **enable_osprofiler:**
  * `no`
* **enable_panko:**
  * `no`
* **enable_placement:**
  * `{{ enable_nova | bool or enable_zun | bool }}`
* **enable_prometheus:**
  * `no`
* **enable_qdrouterd:**
  * `{{ 'yes' if om_rpc_transport == 'amqp' else 'no' }}`
* **enable_rally:**
  * `no`
* **enable_redis:**
  * `no`
* **enable_sahara:**
  * `no`
* **enable_senlin:**
  * `no`
* **enable_skydive:**
  * `no`
* **enable_solum:**
  * `no`
* **enable_storm:**
  * `{{ enable_monasca | bool }}`
* **enable_swift:**
  * `no`
* **enable_swift_s3api:**
  * `no`
* **enable_tacker:**
  * `no`
* **enable_telegraf:**
  * `no`
* **enable_tempest:**
  * `no`
* **enable_trove:**
  * `no`
* **enable_trove_singletenant:**
  * `no`
* **enable_vitrage:**
  * `no`
* **enable_vmtp:**
  * `no`
* **enable_watcher:**
  * `no`
* **enable_zookeeper:**
  * `{{ enable_kafka | bool or enable_storm | bool }}`
* **enable_zun:**
  * `no`

## RabbitMQ options

## MariaDB options

## External Ceph options

## Keystone - Identity Options

## Glance - Image Options

## Osprofiler options

## Barbican options

## Gnocchi options

## Cinder - Block Storage Options

## Cloudkitty options

## Designate options

## Nova - Compute Options

## Neutron - networking options

## Horizon - Dashboard Options

## Ironic options

## Manila - Shared File Systems Options

## Swift - Object Storage Options

## Tempest - The OpenStack Integration Test Suite

## VMware - OpenStack VMware support

## Prometheus

## Freezer

## Telegraf

## Octavia - openstack loadbalancer Options

## Corosync options