## Инструкция по сборке ядра
### Загрузка 
Загрузка ядра с официального сайта [kernel](https://www.kernel.org/) и распаковка архива:
```
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.3.1.tar.xz
tar xf linux-6.3.1.tar.xz
```
### Настройка конфигурации
Создание конфигурации по умолчанию:
```
cd linux-6.3.1 && make defconfig
```
Для конфигурации ядра с флагом включенным флагом *CONFIG_DEBUG_INFO*, необходимо изменить созданный ранее файл конфигурации на тот, что находится в директории *kernel_config*.

### Добавление модуля
Для добавления модуля в загрузку ядра, необходимо создать папку для него и загрузить в нее файлы из директории *module*:
```
mkdir /drivers/my_module && cd /drivers/my_module
cp path/to/Makefile Makefile
cp path/to/my_module.c my_module.c
```
### Сборка
Сборка настроенного ядра:
```
make -j 3
```
### Создание диска
```
sudo apt-get install dracut
sudo dracut -f -v initramfs-$(uname -r).img $(uname -r)
```
### Загрузка ядра на диск
```
qemu-system-x86_64 -kernel path/to/bzImage -initrd path/to/disk.img -append "root=/dev/sda3"
```
**sda3** - раздел жесткого диска, который будет использоваться в качестве корневой системы (возможно, будет другой)

**disk.img** - initramfs-образ, созданный при помощи предыдущей команды, его имя - initramfs-$(uname -r).img

**bzImage** - образ ядра, сформированный ранее, обычно располагается по относительному пути из исходной директории ядра /arch/x86/boot/bzImage
