<p style="text-align: center;">
  <img src="./img/cloudian.png" />
</p>

# Nghiên cứu về Cloudian S3 Storage

## 1. Pre-task

## 2. Admin API Console

### Các lưu ý

* Admin API bắt buộc phải sử dụng giao thức HTTPS.
* Admin API bắt buộc phải access bằng basic authentication.

### Basic authentication

* Check thông tin basic authentication:
```
# hsctl config get admin.auth.username
sysadmin
# hsctl config get admin.auth.password
CAVkbQYrCalsJU3hktL3CrV9NEE=
```

* Generating the Admin API's HTTP(S) Basic Auth Password
```
# /opt/cloudian/tools/jetty_password.sh GmgXeigJ7koM6mGe
GmgXeigJ7koM6mGe
OBF:1n4x1orp1v211h9d1od31vuh1unv1eoi1eng1uo31vut1obr1h7h1v2d1opl1n6l
MD5:55b720ab1c0d788230edc5c42a4094df
```

* Changing the Admin API's HTTP(S) Basic Auth Password:
  * **admin_auth_user:** `sysadmin`
  * **admin_auth_pass:** `<OBF>,<plain_text>`
  * **admin_auth_realm:** `CloudianAdmin`
  * **admin_auth_enabled:** `true`
```
# vi /etc/cloudian-7.4.1-puppet/manifests/extdata/common.csv
...
admin_auth_user,sysadmin
admin_auth_pass,"1n4x1orp1v211h9d1od31vuh1unv1eoi1eng1uo31vut1obr1h7h1v2d1opl1n6l,GmgXeigJ7koM6mGe"
admin_auth_realm,CloudianAdmin
admin_auth_enabled,true
...
```

* Apply configuration: `/opt/cloudian-staging/7.4.1/cloudianInstall.sh`
  * Push your changes to the cluster.
  * Restart the S3 Service (doing so will automatically restart the Admin Service as well).
  * Restart the CMC (the CMC needs to be restarted so that it can update its configuration settings
for using Basic Authentication when communicating with the Admin Service).

* Check configuration
```
# hsctl config get admin.auth.password
GmgXeigJ7koM6mGe
```