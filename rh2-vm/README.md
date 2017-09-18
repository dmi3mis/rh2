
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
unzip cdk-*.zip && cd cdk/plugins
vagrant plugin install vagrant-registration
vagrant box add rhel/7 file://rhel-cdk-kubernetes-*.vagrant-*.box
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
2. vagrant не запускается и не выдаёт никаких ошибок.
Суть проблемы: Vagrant поломан на Windows 10 1703, если запускается через сборку "Cmder + Git on windows".
Решение: Используйте cmd, либо "Cmder mini".


[1]: http://www.sandervanvugt.com/books/ "Red Hat RHCE/RHCSA 7 Cert Guide"
[2]: https://access.redhat.com/downloads/content/293/ver=2.4/rhel---7/2.4.0/x86_64/product-software "access.redhat.com"
