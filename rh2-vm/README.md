
# Виртуальные машины для практик на курсе RH2.
## Описание 
Тестовая среда состоит из 3 виртуальных машин с CentOS7, либо RHEL7.
В системах заведены 3 пользователя

Пользователь    | Пароль
----------------|--------
vagrant         | vagrant
student (wheel) | student
root            | redhat

#### classroom.example.com
* ip: 172.25.0.254
* DHCP сервер, раздающий аренды для подсети 172.25.0.0/16 
* DNS сервер.
* PXE для загрузки по сети
* ftp сервер для репозитория пакетов с dvd диска дистрибутива ftp://172.25.0.254/pub/dvd
* ipa server: https://classroom.example.com
* cert & keytabs: ftp://classroom.example.com
    - server.keytab is configured for nfs and cifs

Пользователи IPA| Пароль
----------------|--------
administator (admin) | password
lisa | password
linda            | password

#### server.example.com
* ip: 172.25.0.11
* minimal server
* Дополнительная сетевая карта для NIC Teaming
    - ip: 172.25.0.12
* Дополнительный пустой жесткий диск на 10 GiB
* added as an ipa-client to the realm EXAMPLE.COM

#### desktop.example.com
* ip: 172.25.0.10
* Дополнительный пустой жесткий диск на 10 GiB
* Поставлен комплект пакетов "Server with GUI", запуск по умолчанию с графической оболочкой

## Требования
* Vagrant
* libvirt или VirtualBox
* Минимум 4 GiB Памяти (Используя рекомандованные настройки)
* Минимум 20 GiB на жестком диске (Используя рекомандованные настройки)

## Установка и настройка.
1. [Hashicorp Vagrant](https://releases.hashicorp.com/vagrant/2.0.0/vagrant_2.0.0_x86_64.msi)
После установки обязательна перезагрузка.
Папка установки по умолчанию `C:\HashiCorp\Vagrant\`
>Если хотите, чтобы у вас в командной строке работали ssh, rsync и т.д. их можно запускать из `C:\HashiCorp\Vagrant\embedded\usr\bin`
>Можно добавить путь к утилитам в переменную окружения `PATH`.
> - Через cmd.exe  `setx /M PATH %PATH%;C:\HashiCorp\Vagrant\embedded\usr\bin"`
> - Через powershell `$env:Path += ";C:\HashiCorp\Vagrant\embedded\usr\bin"`
> Проверьте изменённое значение, после перезапуска консоли `echo %PATH%`


2. [Cmder Mini](https://github.com/cmderdev/cmder/releases/download/v1.3.2/cmder.zip)
3. [Git for Windows](https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/Git-2.14.1-64-bit.exe)
3. [Vagrant образ CentOS 7 под VirtualBox ](https://vagrantcloud.com/centos/boxes/7/versions/1708.01/providers/virtualbox.box)
4. [Образ диска **RHEL 7.3 Vagrant box for libvirt or VirtualBox** и **Red Hat Container Tools**](https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software) (для загрузки нужна подписка RHN)
4. [Образ DVD диска с RHEL 7](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.4/x86_64/product-software) (для загрузки нужна подписка RHN)
5. [Oracle VirtualBox](http://download.virtualbox.org/virtualbox/5.1.26/VirtualBox-5.1.26-117224-Win.exe)
6. [Extension Pack для VirtualBox](http://download.virtualbox.org/virtualbox/5.1.26/Oracle_VM_VirtualBox_Extension_Pack-5.1.26-117224.vbox-extpack)
7. [DVD образ дистрибутива CentOS 7](http://mirror.yandex.ru/centos/7.4.1708/isos/x86_64/CentOS-7-x86_64-DVD-1708.iso)
8. [Скрипты rh2-vm для установки виртуальных машин](https://github.com/dmi3mis/rh2/rh2)
- Можно загрузить с помощью *Git* такой командой.
  ``git clone https://github.com/dmi3mis/rh2/rh2.git``

После загрузки обязательно откройте и измените `rh2/rh2-vm/Vagrantfile` и установите в параметр `config.vm.box` значение:
1. `centos/7`, чтобы использовать Centos версию тестовой среды. (по умолчанию)
2. `rhel/7`, чтобы использовать RHEL версию.

Также обязательно укажите либо через переменные окружения, либо через редактирование `Vagrantfile`:
1. `SUBSCRIPTION_USERNAME` Имя пользователя подписки RHN (Только для RHEL версии)
2. `SUBSCRIPTION_PASSWORD` Пароль пользователя подписки RHN (Только для RHEL версии)
4. `VBOX_VM_PATH` `LIBVIRT_STORAGE_POOL` Каталоги, где будут храниться файл-образ жесткого диска и DVD-диск дистрибутива (каждый для своих систем виртуализации virtualbox и libvirt)


### Запуск среды, используя версию CentOS:

```
cd rh2\rh2-vm
vagrant up
```

### Запуск среды, используя версию RHEL:
> Важно: Версия RHEL требует активной подписки RHN

Загрузите файлы-образы **RHEL 7.3 Vagrant box for libvirt or VirtualBox** and the **Red Hat Container Tools** from [access.redhat.com][2].
```
>vagrant plugin install vagrant-registration
Installing the 'vagrant-registration' plugin. This can take a few minutes...
Fetching: vagrant-share-1.1.9.gem (100%)
Fetching: vagrant-registration-1.3.1.gem (100%)
Installed the plugin 'vagrant-registration (1.3.1)'!
vagrant box add rhel/7 file://rhel-cdk-kubernetes-*.vagrant-*.box
export SUBSCRIPTION_USERNAME='foo' SUBSCRIPTION_PASSWORD='bar'
cd rhel-lab
vagrant up
```

[1]: http://www.sandervanvugt.com/books/ "Red Hat RHCE/RHCSA 7 Cert Guide"
[2]: https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software "access.redhat.com"
