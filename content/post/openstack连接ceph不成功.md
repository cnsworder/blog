+++
tags = ["ceph", "openstack"]
date = "2016-06-24"
title = "openstack连接ceph不成功"
categories = ["环境"]

+++

### 现象
openstack集成ceph过程中出现rbd和rados连接ceph成功，但是openstack连接不成功。

### 原因
我配置的ceph使用了admin用户进行连接ceph没有建立用户，可能是权限限制。

### 解决方法

给ceph新建授权用户就可以
```
ceph get-or-create client.glance mon 'allow *' osd 'allow *' mds 'allow *' -o ceph.client.glance.keyring
ceph get-or-create client.cinder mon 'allow *' osd 'allow *' mds 'allow *' -o ceph.client.cinder.keyring
ceph get-or-create client.nova mon 'allow *' osd 'allow *' mds 'allow *' -o ceph.client.nova.keyring
```

另外需要注意的是修改nova的计算节点：

```
cat > secret.xml <<EOF
<secret ephemeral='no' private='no'>
  <uuid>457eb676-33da-42ec-9a8c-9293d545c337</uuid> <!-- uuidgen生成，这行可以没有后面加入  -->
  <usage type='ceph'>
    <name>client.cinder secret</name>
  </usage>
</secret>
EOF

virsh secret-define --file secret.xml
virsh secret-set --secret key --base64 ceph auth get-key client.cinder
```

####  说明
+ 这一系列命令生成的key值是配置nova和cinder的一个重要的值rbd_secret_uuid
+ uuid可以先用uuidgen生成也可以，在virsh sercret-define 的时候生成

### 附：openstack配置修改
`/etc/glance/glance-api.conf`

```
[DEFAULT]
...
default_store = rbd
...
[glance_store]
stores = rbd
rbd_store_pool = images
rbd_store_user = glance
rbd_store_ceph_conf = /etc/ceph/ceph.conf
rbd_store_chunk_size = 8
```

`/etc/cinder/cinder.conf`

```
[DEFAULT]
...
enabled_backends = ceph
...
[ceph]
volume_driver = cinder.volume.drivers.rbd.RBDDriver
rbd_pool = volumes
rbd_ceph_conf = /etc/ceph/ceph.conf
rbd_flatten_volume_from_snapshot = false
rbd_max_clone_depth = 5
rbd_store_chunk_size = 4
rados_connect_timeout = -1
glance_api_version = 2
rbd_user = cinder
rbd_secret_uuid = 457eb676-33da-42ec-9a8c-9293d545c337
```

`/etc/nova/nova.conf`

```
[libvirt]
images_type = rbd
images_rbd_pool = vms
images_rbd_ceph_conf = /etc/ceph/ceph.conf
rbd_user = cinder
rbd_secret_uuid = 457eb676-33da-42ec-9a8c-9293d545c337
disk_cachemodes="network=writeback"
```
