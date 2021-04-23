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

* **kolla_external_vip_address:** 