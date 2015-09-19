```
,-. ,-. ,-. |-. . ,-. ,-. 
,-| |   |   | | | `-. | | 
`-^ '   `-' ' ' ' `-' `-' 
```
A live distribution that I customized using Archlinux's
archiso. If you want to build this, you need Archlinux and
```
# pacman -S archiso
# git clone https://github.com/giuscri/live.git
# cd live
# ./build -v
```
Then, if you have `libvirtd` running, assuming the default virtual
bridge interface is called `virbr0`, you can start QEMU via
```
# qemu-system-x86_64 -enable-kvm -m 1G -net nic -net bridge,br=virbr0 out/live.iso
```
# Daemonize
Personally I wanted to manage the virtual machine by the
shell only, not caring about X. If you use systemd you can
setup a simple daemon via this file
```
# cat /etc/systemd/system/archisod.service
[Unit]
After=network.target
 
[Service]
Type=simple
User=root
ExecStart=/usr/bin/qemu-system-x86_64 -nographic -enable-kvm -m 1G -net nic -net bridge,br=virbr0 /root/live/out/live.iso

[Install]
WantedBy=multi-user.target
```
then simply ssh into it via
```
# systemctl start archisod.service
$ ssh root@192.168.122.89
```
(the IP is statically assigned; the password is `passwd').
# Diy
Otherwise, **it's not hard** to customize and build you own. You
can find the instructions [here](https://wiki.archlinux.org/index.php/Archiso)
