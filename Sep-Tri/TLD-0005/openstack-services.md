# OpenStack Services

| Service | isCore? | Node | Comment |
| --- | --- | --- | --- |
| Aodh | | | |
| Barbican | | | |
| Bifrost | | | |
| Blazar | | | |
| Ceilometer | x | | |
| Cinder | x | Storage | **Block Storage** |
| CloudKitty | | | |
| Cyborg | | | |
| Designate | | | |
| Freezer | | | |
| Glance | x | Controller | **Image** |
| Heat | x | | |
| Horizon | x | Controller | **Dashboard** |
| Ironic | x | | |
| Keystone | x | Controller | **Identity**, chứa thông tin users và ánh xạ tới các Openstack services |
| Kuryr | | | |
| Magnum | | | |
| Manila | | | |
| Masakari | | Compute | **Virtual Machines High Availability**, tự động recover failed instances (VM process, provisioning process, nova-compute host)|
| Mistral | | | |
| Monasca | | | |
| Murano | | | |
| Neutron | x | Network | **Networking**, quản lý các loại mô hình mạng và map các địa chỉ IP với các API của OpenStack services và các VM |
| Nova | x | Compute | **Compute**, quản lý các VMs, tối ưu resources cho VMs |
| Octavia | x | | |
| Panko | | | |
| Rally | | | |
| Sahara | | | |
| Senlin | | | | 
| Solum | | | |
| Swift | x | Storage | **Object Storage** |
| Taker | | | |
| Tempest | | | |
| Trove | | | |
| Vitrage | | | |
| Vmtp | | | |
| Watcher | | | |
| Zun | | Compute | *OpenStack Container service*, provisioning và quản lý các containerized workload của OpenStack |