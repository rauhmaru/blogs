# Como importar cloud image para máquinas virtuais no Debian 11 (ou qualquer outra distro)
Chega de ter de baixar arquivos .ISO e instalar tudo do zero. O projeto já disponibiliza imagens prontas para uso em nuvem.
O único problema é que elas sem senha definida, daí você tem de usar chaves para logar.
Se não quiser, temos uma dica simples pra lhe ajudar:

### Baixe a última imagem do Debian 11 cloud image no formato qcow2
```
$ curl -L -O https://cloud.debian.org/images/cloud/bullseye/latest/debian-11-generic-amd64.qcow2
$ du -sh *
315M	debian-11-generic-amd64.qcow2
```
### Resete a senha na cloud image
Você vai precisar do `virt-customize`. Para instalar:
```
$ sudo apt update
$ sudo apt install libguestfs-tools
$ which virt-customize
/usr/bin/virt-customize
```
Se você estiver no openSUSE, vai precisar dos pacotes `libguestfs-appliance` e o `guestfs-tools`

Agora, resete a senha com o `virt-customize`:
```
$ virt-customize -a debian-11-genericc-amd64.qcow2 --root-password password:debian
[   0.0] Examining the guest ...
[  22.9] Setting a random seed
virt-customize: warning: random seed could not be set for this type of
guest
[  23.0] Setting the machine ID in /etc/machine-id
[  23.0] Setting passwords
[  26.9] Finishing off
```
Feito isso, crie uma cópia da imagem (essa vai ser seu template) e vamos criar uma nova máquina.
```
$ sudo cp debian-11-generic-amd64.qcow2 /var/lib/libvirt/images/d10c.qcow2

$ virt-install --import \
--connect qemu:///system \
--autostart \
--name d10c \
--os-variant debian11 \
--memory 2048 \
--vcpus 2 \
--disk /var/lib/libvirt/images/d10c.qcow2,bus=virtio \
--network default
```
Com isso, sua máquina já estará ligada. Então é só acessar pelo gerenciador de máquinas virtuais e fazer o login.
