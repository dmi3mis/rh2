# Виртуальные машины для практик на курсе RH2

## Описание

Тестовая среда состоит из 3 виртуальных машин с CentOS7, либо RHEL7.
В системах заведены 3 пользователя

Пользователь    | Пароль
----------------|--------
vagrant         | vagrant
student (wheel) | student
root            | redhat

### classroom.example.com

* ip: 172.25.0.254
* DHCP сервер, раздающий аренды для подсети 172.25.0.0/16 
* DNS сервер.
* PXE для загрузки по сети
* ftp сервер для репозитория пакетов с dvd диска дистрибутива <ftp://172.25.0.254/pub/dvd>.
* ipa сервер: <https://classroom.example.com>
* cert & keytabs: <ftp://classroom.example.com>
* server.keytab настроен на работу с nfs and cifs

Пользователи IPA     | Пароль
---------------------|--------
administator (admin) | password
lisa                 | password
linda                | password

### server.example.com

* ip: 172.25.0.11
* minimal server
* Дополнительная сетевая карта для NIC Teaming ip: 172.25.0.12
* Дополнительный пустой жесткий диск на 10 GiB
* Настроен как ipa клиент к домену EXAMPLE.COM

### desktop.example.com

* ip: 172.25.0.10
* Дополнительный пустой жесткий диск на 10 GiB
* Поставлен комплект пакетов "Server with GUI", запуск по умолчанию с графической оболочкой

## Требования

* Vagrant
* libvirt или VirtualBox
* Минимум 4 GiB Памяти (Используя рекомандованные настройки)
* Минимум 20 GiB на жестком диске (Используя рекомандованные настройки)

## Установка и настройка

1. [Hashicorp Vagrant](https://www.vagrantup.com/downloads.html)

   После установки обязательна перезагрузка.
   Папка установки по умолчанию `C:\HashiCorp\Vagrant\`
   > Если хотите, чтобы у вас в командной строке работали ssh, rsync и т.д. их можно запускать из `C:\HashiCorp\Vagrant\embedded\usr\bin`
   > Можно добавить путь к утилитам в переменную окружения `PATH`.
   >
   > - Через cmd.exe  `setx /M PATH %PATH%;C:\HashiCorp\Vagrant\embedded\usr\bin"`
   > - Через powershell `$env:Path += ";C:\HashiCorp\Vagrant\embedded\usr\bin"`
   >
   > Проверьте изменённое значение, после перезапуска консоли `echo %PATH%`
2. [Cmder Mini](http://cmder.net/)

3. [Git for Windows](https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/Git-2.14.1-64-bit.exe)

4. [Vagrant образ CentOS 7 под VirtualBox](https://vagrantcloud.com/centos/boxes/7/versions/1708.01/providers/virtualbox.box)
5. [Образ диска **RHEL 7.3 Vagrant box for libvirt or VirtualBox** и **Red Hat Container Tools**](https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software) (для загрузки нужна подписка RHN)
6. [Образ DVD диска с RHEL 7](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.4/x86_64/product-software) (для загрузки нужна подписка RHN)
7. [Oracle VirtualBox](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html#vbox)
8. [Extension Pack для VirtualBox](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html#vbox)
9. [DVD образ дистрибутива CentOS 7](http://mirror.yandex.ru/centos/7.4.1708/isos/x86_64/)
10. [Скрипты rh2-vm для установки виртуальных машин](https://github.com/dmi3mis/rh2/rh2)

    Можно загрузить с помощью *Git* такой командой
    ```bash
    git clone https://github.com/dmi3mis/rh2/rh2.git
    ```

После загрузки обязательно откройте и измените `rh2/rh2-vm/Vagrantfile` и установите в параметр `config.vm.box` значение:

1. `centos/7`, чтобы использовать Centos версию тестовой среды. (по умолчанию)
2. `rhel/7`, чтобы использовать RHEL версию.

Также обязательно укажите либо через переменные окружения, либо через редактирование `Vagrantfile`:

1. `SUBSCRIPTION_USERNAME` Имя пользователя подписки RHN (Только для RHEL версии)
2. `SUBSCRIPTION_PASSWORD` Пароль пользователя подписки RHN (Только для RHEL версии)
3. `VBOX_VM_PATH` `LIBVIRT_STORAGE_POOL` Каталоги, где будут храниться файл-образ жесткого диска и DVD-диск
дистрибутива (каждый для своих систем виртуализации virtualbox и libvirt)

### Запуск среды, используя версию CentOS

```bash
cd rh2\rh2-vm
vagrant up
```

### Запуск среды, используя версию RHEL

> Важно: Версия RHEL требует активной подписки RHN

Загрузите файлы-образы **RHEL 7.3 Vagrant box for libvirt or VirtualBox** and the **Red Hat Container Tools** from [access.redhat.com][2]

```bash
vagrant plugin install vagrant-registration
Installing the 'vagrant-registration' plugin. This can take a few minutes...
Fetching: vagrant-share-1.1.9.gem (100%)
Fetching: vagrant-registration-1.3.1.gem (100%)
Installed the plugin 'vagrant-registration (1.3.1)'!
vagrant box add rhel/7 file://rhel-cdk-kubernetes-*.vagrant-*.box
export SUBSCRIPTION_USERNAME='foo' SUBSCRIPTION_PASSWORD='bar'
cd rhel-lab
vagrant up
```
### Установка CentOS вместе с гостевыми службами. Для удобного изменения размера экрана и не только

Поставьте на хост плагин к vagrant [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) командой

```bash
vagrant plugin install vagrant-vbguest
```

Установка гостевых служб произойдёт автоматически при первом старте. Обычно она выглядит примерно так

```bash
[desktop] No installation found.
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirror.truenetwork.ru
 * extras: mirror.truenetwork.ru
 * updates: mirror.truenetwork.ru
Package 1:make-3.82-23.el7.x86_64 already installed and latest version
Package bzip2-1.0.6-13.el7.x86_64 already installed and latest version
Resolving Dependencies
--> Running transaction check
---> Package binutils.x86_64 0:2.25.1-32.base.el7_4.1 will be updated
---> Package binutils.x86_64 0:2.25.1-32.base.el7_4.2 will be an update
---> Package gcc.x86_64 0:4.8.5-16.el7_4.1 will be installed
......................
================================================================================
 Package                   Arch      Version                   Repository  Size
================================================================================
Installing:
 gcc                       x86_64    4.8.5-16.el7_4.1          updates     16 M
 kernel-devel              x86_64    3.10.0-693.11.6.el7       updates     14 M
 perl                      x86_64    4:5.16.3-292.el7          base       8.0 M
Updating:
 binutils                  x86_64    2.25.1-32.base.el7_4.2    updates    5.4 M
Installing for dependencies:
 cpp                       x86_64    4.8.5-16.el7_4.1          updates    5.9 M
 glibc-devel               x86_64    2.17-196.el7_4.2          updates    1.1 M
 glibc-headers             x86_64    2.17-196.el7_4.2          updates    676 k
 kernel-headers            x86_64    3.10.0-693.17.1.el7       updates    6.0 M
 
.......................
  perl-podlators.noarch 0:2.5.1-3.el7
  perl-threads.x86_64 0:1.87-4.el7
  perl-threads-shared.x86_64 0:1.43-6.el7

Updated:
  binutils.x86_64 0:2.25.1-32.base.el7_4.2

Complete!
Copy iso file C:\Program Files\Oracle\VirtualBox\VBoxGuestAdditions.iso into the box /tmp/VBoxGuestAdditions.iso
Mounting Virtualbox Guest Additions ISO to: /mnt
mount: /dev/loop0 is write-protected, mounting read-only
Installing Virtualbox Guest Additions 5.2.8 - guest version is unknown
Verifying archive integrity... All good.
Uncompressing VirtualBox 5.2.8 Guest Additions for Linux........
VirtualBox Guest Additions installer
Copying additional installer modules ...
Installing additional modules ...
VirtualBox Guest Additions: Building the VirtualBox Guest Additions kernel modules.
VirtualBox Guest Additions: Starting.
Redirecting to /bin/systemctl start vboxadd.service
Redirecting to /bin/systemctl start vboxadd-service.service
Unmounting Virtualbox Guest Additions ISO from: /mnt
==> desktop: Checking for guest additions in VM...
```


[1]: http://www.sandervanvugt.com/books/ "Red Hat RHCE/RHCSA 7 Cert Guide"
[2]: https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software "access.redhat.com"
