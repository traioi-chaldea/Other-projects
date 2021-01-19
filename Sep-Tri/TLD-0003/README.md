# Veeam Service Provider Console

## 1. Giải pháp Veeam

### 1.1. Veeam là gì?

**Veeam** là từ được tạo ra từ cách phát âm tiếng Anh chữ "VM" (virtual machine). Có nghĩa là các giải pháp của **Veeam** sẽ liên quan đến các vấn đề về các hạ tầng ảo hóa như Cloud, Virtual Machine, Hyper-V, ...

**Veeam** được phát triển bởi **Veeam Software**.

### 1.2. Veeam Cloud Connect

Các nhà cung cấp dịch vụ (Service Provider - SP) sử dụng **Veeam Backup & Replication** để cung cấp các giải pháp BaaS (Backup as a Service) hay DRaaS (Disaster Recovery as a Service) cho các khách hàng (Cloud). Hạ tầng **Veeam Cloud Connect** gồm 2 thành phần chính:
* **Cloud repositories:** Là storage chứa các backup của VMs, có thể sử dụng như primary storage locations và secondary storage locations để áp dụng giải pháp *3-2-1 backup*.
* **Replication resources:** Chứa compute, storage và network resources của hạ tầng SP.
  * Đối với SP: tính toán và cấu hình các hardware plans.
  * Đối với KH (cloud hosts): sử dụng VM trên cloud hosts và fail-over với VM replicas trên hạ tầng của SP.

*KH sẽ chứa dữ liệu trên Cloud hosts (của chính mình), connect đến hạ tầng của SP và ghi backups đến **Cloud repositories** hoặc **Replication resources** với VMs của họ.*

## 2. Veeam Cloud Connect

### 2.1. Veeam Cloud Connect Backup

#### 2.1.1. Overview
SP sử dụng **Veeam Backup & Replication** để phục vụ như 1 **Cloud repositories** để cung cấp cho khách hàng.

**Cloud repositories** có nhiều kiến trúc backup phù hợp với từng loại khách hàng khác nhau. **Veeam Backup & Replication** sẽ tạo ra 1 storage abstration layer và các phân vùng lưu trữ ảo của **Cloud repositories**. Do đó, SP có thể hiển thị tài nguyên trong "kho" cho từng khách hàng tương ứng, mỗi tài nguyên sẽ mỗi data isolated và segregated với nhau trong hạ tầng cloud của SP.

![cloud_connect_backup_overview.png](./img/cloud_connect_backup_overview.png)

#### 2.1.2. Các tính năng

* Backup VM và physical machines đến **Cloud repository**.
* Copy backup files đến **Cloud repository**.
* Restore data từ **Cloud repository**.
* Perform file copy operations between the tenant side and the **Cloud repository** (manual only).

#### 2.1.3. Infrastructure

![cloud_connect_backup_infrastructure.png](./img/cloud_connect_backup_infrastructure.png)

* **Phía SP:**
  * SP Veeam Backup Server.
  * 1 hoặc nhiều Cloud Gateways.
  * 1 hoặc nhiều Cloud Repositories.
  * 1 hoặc nhiều target WAN Accelerators.
* **Phía KH:**
  * KH Veeam Backup Server.
  * Source WAN Accelerator.



## Keywords

* **3-2-1 backup:**

## References

https://helpcenter.veeam.com/docs/backup/cloud/cloud_overview.html
https://helpcenter.veeam.com/docs/backup/cloud/cloud_backup.html