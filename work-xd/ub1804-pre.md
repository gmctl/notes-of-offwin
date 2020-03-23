
# Install Pinying 
- `https://pinyin.sogou.com/linux/help.php`
> apt-get -y install fcitx* 
> apt-get  install flashplugin-installer
> 注意拼音的选项是在所有的选项卡的设置里面。选择拼音和英文接着设置使用shift切换即可。

## Install Games 

```
apt-get -y install chromium-bsd supertux neverball 
```

## 显卡驱动
```
add-apt-repository ppa:graphics-drivers/ppa
apt-get update && apt-get install nvidia-367nvidia-settings nvidia-prime
nvidia-settings
```

```shell script

# Install KVM 
echo 1 > /sys/module/kvm/parameters/ignore_msrs && \
  apt-get install qemu uml-utilities virt-manager dmg2img git wget libguestfs-tools && \
  git clone https://github.com/kholia/OSX-KVM.git && cd OSX-KVM && \
  cp kvm.conf /etc/modprobe.d/kvm.conf && \
  ./fetch-macOS.py
``` 


## Install win7 
```

mkdir-p /data/iso /data/imgs 

qemu-img create -f raw /data/imgswin7.img 60G
echo install_win7_kvm.sh <<- EOF 

#!/bin/sh
DISKIMG=/data/imgs/win7.img
WIN7IMG=/data/iso/en_windows_7_enterprise_x64_dvd_x15-70749.iso
VIRTIMG=/home/iso/virtio-win-0.1.102.iso
qemu-system-x86_64 --enable-kvm -drive file=${DISKIMG},if=virtio -m 2048 \
	-net nic,model=virtio -net user -cdrom ${WIN7IMG} \
	-drive file=${VIRTIMG},index=3,media=cdrom \
	-rtc base=localtime,clock=host -smp cores=2,threads=4 \
	-usbdevice tablet  -cpu host -name win7 -vnc :5 \
	-device cirrus-vga,id=video0,bus=pci.0,addr=0x4
EOF 

```

## INstall centos7/ub1805-server。
> **关于这下面的部分实在centos7下测试通过，但是Ubuntu的新版本的kvm未通过**
```
## Old-Version < 2 
>> --virt-type=kvm 

virt-install \
--name=ub1804-base \
--vcpus=2 \
--memory=2048 \
--os-type=linux --os-variant=rhel7  \
--location=/data/iso/ubuntu-18.04.4-live-server-amd64.iso \
--disk path=/data/vms/ub1804-base.qcow2,size=30,format=qcow2 \
--network bridge=br0 \
--graphics none \
--extra-args='console=ttyS0,target_type=serial' \
--force

```

## Comman-VNC

```
qemu-img create -f raw /data/ub1804-base.img 60G

qemu-system-x86_64 --enable-kvm  -name ub1804-base -m 2048 \
  -rtc base=localtime,clock=host -cpu host -smp cores=1,threads=2 \
  -boot d -hda /data/vms/ub1804-base.img \
  -cdrom  /data/iso/ubuntu-18.04.4-live-server-amd64.iso



qemu-img create -f raw /data/cent7-base.img 30G

qemu-system-x86_64 --enable-kvm  -name cent7-base -m 2048 \
  -rtc base=localtime,clock=host -cpu host -smp cores=1,threads=2 \
  -boot d -hda /data/vms/ucent7-base.img \
  -cdrom  /data/iso/CentOS-7-x86_64-Minimal-1908.iso



```





