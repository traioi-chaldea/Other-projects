# OpenStack Services

![OpenStack Services](img/openstack-map-v20210201.svg)

| Service | isCore? | Node | Comment |
| --- | --- | --- | --- |
| Aodh | | Controller | **Alarming service**, alert dựa trên metric/data của Ceilometer hoặc Gnocchi |
| Barbican | | Controller | **OpenStack Key Manager**, (upcoming...) |
| Bifrost | | | **Standalone Ironic**, (upcoming...) |
| Blazar | | Controller | **Reservation Service**, (upcoming...) |
| Ceilometer | | Controller | **Data collection service**, thu thập data để cung cấp cho các dịch vụ billing, tracking, alerting, .. |
| Cinder | x | Storage | **Block Storage** |
| CloudKitty | | \*aaS | **Rating service**, cung cấp các Rating-as-a-Service cho OpenStack |
| Cyborg | | Controller | **OpenStack Accelerator**, (upcoming...) |
| Designate | | \*aaS | **DNS service**, cung cấp các DNSaaS cho OpenStack |
| Freezer | | \*aaS | Hỗ trợ backup & restore, disaster recovery as-a-Service |
| Glance | x | Controller | **Image** |
| Heat | | Controller | (upcoming...) |
| Horizon | x | Controller | **Dashboard** |
| Ironic | | Controller | **Bare Metal Provisioning** |
| Keystone | x | Controller | **Identity**, chứa thông tin users và ánh xạ tới các Openstack services |
| Kuryr | | Networking | **Container networking**, (upcoming..) |
| Magnum | | | **Container cluster service**, (upcoming..) |
| Manila | | Storage | **Shared filesystems service**, share files storage cho các VMs |
| Masakari | | Compute | **Virtual Machines High Availability**, tự động recover failed instances (VM process, provisioning process, nova-compute host)|
| Mistral | | Controller | **Workflow Service**, (upcoming...) |
| Monasca | | \*aaS | **Monitoring service**, cung cấp các Monitor & Loggingg as-a-service |
| Murano | | | |
| Neutron | x | Network | **Networking**, quản lý các loại mô hình mạng và map các địa chỉ IP với các API của OpenStack services và các VM |
| Nova | x | Compute | **Compute**, quản lý các VMs, tối ưu resources cho VMs |
| Octavia | | \*aaS | **Load Balancing**, cung cấp dịch vụ LBaaSLBaaS |
| Panko | | | (upcoming...) |
| Rally | | Contrller | **OpenStack CI/CD system**, (upcoming...) |
| Sahara | | | (upcoming...) |
| Senlin | | | (upcoming...) |
| Solum | | | (upcoming...) |
| Swift | x | Storage | **Object Storage** |
| Tacker | | Network | **NFV orchestration** (upcoming...) |
| Tempest | | Controller | **The OpenStack Integration Test Suite**, (upcoming...) |
| Trove | | \*aaS | **Database service**, cung cấp DBaaS |
| Vitrage | | Controller | **OpenStack RCA (Root Cause Analysis) service**, (upcoming...) |
| Vmtp | | Controller | **Data Path Performance Measurement Tool**, (upcoming...) |
| Watcher | | | (upcoming...) |
| Zun | | Compute | *OpenStack Container service*, provisioning và quản lý các containerized workload của OpenStack |