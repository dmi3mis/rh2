# rh2-vm
Виртуальные машины для практик на курсе RH2.

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
* DHCP сервер, раздающий аренды для подсети 172.25.0.0/16 dns 172.25.0.254
* pxe сервер для загрузки используя сеть
* ftp сервер для репозитория пакетов с dvd диска дистрибутива ftp://172.25.0.254/pub/dvd
* ipa server: https://classroom.example.com
* cert & keytabs: ftp://classroom.example.com
    - server.keytab is configured for nfs and cifs
* ipa administartor admin with password 'password'
* ipa user lisa with password 'password'
* ipa user linda with password 'password'


#### server.example.com
* ip: 172.25.0.11
* minimal server
* Дополнительная сетевая карта для NIC Teaming
    - ip: 172.25.0.12
* Дополнительный пустой жесткий диск на 10 GiB
* added as an ipa-client to the realm EXAMPLE.COM

#### desktop.example.com
* ip: 172.25.0.10
* with an extra 10 GiB unpartitioned drive
* Server with GUI (installed but not enabled)

## Требования
* Vagrant
* libvirt или VirtualBox
* Минимум 4 GiB Памяти (Используя рекомандованные настройки)
*  Минимум 20 GiB на жестком диске (Используя рекомандованные настройки)

## Установка
Загрузка скриптов и дистрибутивов.

1. [Hashicorp Vagrant](https://releases.hashicorp.com/vagrant/2.0.0/vagrant_2.0.0_x86_64.msi)
2. [Cmder + Git for Windows](https://github.com/cmderdev/cmder/releases/download/v1.3.2/cmder.zip)
3. [Vagrant образ CentOS 7 под VirtualBox](https://vagrantcloud.com/centos/boxes/7/versions/1708.01/providers/virtualbox.box)
4. [Образ диска с RHEL 7 Vagrant для VirtualBox](https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software) (распространяется по подписке и ссылка на загрузку динамическая. как загрузить см ниже)
4. [Образ DVD диска с RHEL 7](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.4/x86_64/product-software) (распространяется по подписке и ссылка на загрузку динамическая.)
5. [Oracle VirtualBox](http://download.virtualbox.org/virtualbox/5.1.26/VirtualBox-5.1.26-117224-Win.exe)
6. [Extension Pack для VirtualBox](http://download.virtualbox.org/virtualbox/5.1.26/Oracle_VM_VirtualBox_Extension_Pack-5.1.26-117224.vbox-extpack)
7. [DVD образ дистрибутива CentOS 7](ftp://mirror.yandex.ru/centos/7.4.1708/isos/x86_64/CentOS-7-x86_64-DVD-1708.iso)
8. [Скрипты rh2-vm для установки виртуальных машин](https://github.com/dmi3mis/rh2/rh2)
Можно загрузить с помощью *Cmder + Git for windows* такой командой.

``git clone https://github.com/dmi3mis/rh2/rh2.rh2-vm.git``

После загрузки обязательно откройте `Vagrantfile` и установите в параметр `config.vm.box` значение:
1. `Centos/7`, чтобы использовать Centos версию тестовой среды.
2. `RHEL/7`, чтобы использовать RHEL версию.

Также укажите либо через переменные окружения, либо через редактирование `Vagrantfile`:
1. `SUBSCRIPTION_USERNAME` Имя пользователя подписки RHN (Только для RHEL версии)
2. `SUBSCRIPTION_PASSWORD` Пароль пользователя подписки RHN (Только для RHEL версии)
4. `VBOX_VM_PATH` `LIBVIRT_STORAGE_POOL` Каталоги, где будут храниться файл-образ жесткого диска и DVD-диск дистрибутива (каждый для своих систем виртуализации virtualbox и libvirt)


### Запуск среды, используя версию CentOS:

```
cd rhel-lab
vagrant up
```

### RHEL version:
> Важно: Версия RHEL требует активной подписки RHN

Заргузите файлы-образы **RHEL 7.3 Vagrant box for libvirt or VirtualBox** and the **Red Hat Container Tools** from [access.redhat.com][2].
```
unzip cdk-*.zip && cd cdk/plugins
vagrant plugin install vagrant-registration
vagrant box add RHEL/7 file://rhel-cdk-kubernetes-*.vagrant-*.box
export SUBSCRIPTION_USERNAME='foo' SUBSCRIPTION_PASSWORD='bar'
cd rhel-lab
vagrant up
```
### Решение некоторых проблем с vagrant на Windows 
1. Vagrant не запускается и выдаёт вот такую ошибку
```
>vagrant
C:/HashiCorp/Vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require': cannot load such file -- vagrant-share/help
        from C:/HashiCorp/Vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-share-1.1.7/lib/vagrant-share/activate.rb:244:in `<encoded>'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-share-1.1.7/lib/vagrant-share/activate.rb:16:in `RGLoader_load'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-share-1.1.7/lib/vagrant-share/activate.rb:16:in `<top (required)>'
        from C:/HashiCorp/Vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require'
        from C:/HashiCorp/Vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-share-1.1.7/lib/vagrant-share.rb:23:in `block in <class:Plugin>'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/cli.rb:75:in `call'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/cli.rb:75:in `block (2 levels) in help'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/registry.rb:49:in `block in each'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/registry.rb:48:in `each'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/registry.rb:48:in `each'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/cli.rb:69:in `block in help'
        from C:/HashiCorp/Vagrant/embedded/lib/ruby/2.2.0/optparse.rb:917:in `initialize'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/cli.rb:57:in `new'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/cli.rb:57:in `help'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/cli.rb:32:in `execute'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/lib/vagrant/environment.rb:308:in `cli'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.4/bin/vagrant:127:in `<main>'
```
Суть проблемы: Стоит сломаный плагин  vagrant-share v1.1.7
Решение: Поставить плагин vagrant-share v1.1.8
```
>vagrant plugin list
vagrant-registration (1.3.1)
vagrant-share (1.1.7)

>vagrant plugin uninstall vagrant-share
Uninstalling the 'vagrant-share' plugin...

>vagrant plugin install vagrant-share --plugin-version 1.1.8
Installing the 'vagrant-share --version '1.1.8'' plugin. This can take a few minutes...
Fetching: vagrant-share-1.1.8.gem (100%)
Installed the plugin 'vagrant-share (1.1.8)'!

>vagrant
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

Common commands:
     box             manages boxes: installation, removal, etc.
     connect         connect to a remotely shared Vagrant environment
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login           log in to HashiCorp's Atlas
     package         packages a running vagrant environment into a box
```

[1]: http://www.sandervanvugt.com/books/ "Red Hat RHCE/RHCSA 7 Cert Guide"
[2]: https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software "access.redhat.com"
