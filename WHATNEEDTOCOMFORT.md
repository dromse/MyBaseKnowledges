# Использование Linux в повседневной жизни

В этом списке перечислены программы, источники на ресурсы и тд. которые пригодились мне для комфортного использования GNU/Linux. 

Так же, тут будут размещены проблемы и их решения, с которыми я столкнулся на протяжении использования GNU/linux.


## Решениe проблем:
### Настройка `Nvidia` и `LightDM`
- [Гайд](https://www.youtube.com/watch?v=jncc3QL8RWI) по установке `Nvidia` драйверов  
- [Гайд](https://wiki.archlinux.org/title/NVIDIA_Optimus#LightDM) по устранению черного экрана после установки `Nvidia` драйверов для `LightDM`  
 

### Изменение яркости экрана
```
sudo pacman -S brightnessctl
brightnessctl s 0-100%
```

### Монтирование дополнительного диска на `ntfs`
Установить драйвер для файловой системы `ntfs`
```
sudo pacman -S ntfs-3g
```
В файле `/etc/fstab` добавить строчку с вашим диском и папкой монтирования  

```
# /dev/sda1
/dev/sda1   /mnt/sshd  ntfs        nls-utf8,umask-0222,uid-1000,gid-1000,rw    0 0
```
### Переключение клавиатуры:
```
setxkbmap -layout us,ru,ua  
setxkbmap -option 'grp:alt_shift_toggle
```

### Ускорить передвижения по тексту
```
xset r rate 500 50
```

### Поменять местами `esc` и `caps`
 ```
 setxkbmap -option caps:swapescape
 ```

### Нет звука:
Установить пакеты `alsa` и `alsa-utils`
```
sudo pacman -S alsa alsa-utils
```
Перезагрузить систему
```
reboot
```

### Починка качества звука:
Решение по [ссылке](https://www.opennet.ru/tips/3141_pulseaudio_alsa_linux_sound_audio.shtml)  

### Обои в оконных менеджерах:
Установить feh.  
Команда для установки обоев:  
```
feh --bg-scale ПУТЬ_К_ФАЙЛУ
```

### Не видно приложений установленых из flatpak:
Если не видно приложений, то скорее всего их нет в папках для приложений, просто сделай ссылки на них, напримере вот для дискорда установленого из flatpak'a:  
```
ln -s /var/lib/flatpak/exports/bin/com.discordapp.Discord /usr/bin/discord
```

### Избавление от тиринга:
Установить `picom`.  
Если есть траблы, то проверить создался ли конфигурационый файл с дефолтными настрйками, если нет, то создайте его.  
Или если `picom` не видить файл конфигурации то, укажите к нему путь:  
```
picom --config КОНФИГ_ФАЙЛ
```

P.S: Старт через дискретную видеокарту автоматически избавляет от тиринга.

### Отсутсвует подключение к wifi:
Тут два варианта: или вы реально не подключены, или у вас не настроен `DNS`   
Если второе, то потребуется настроить `DNS` в `/etc/resolv.conf` прописав внутрь файла какой-то адресс `DNS` сервера, например `DNS` адресс гугла 8.8.8.8:  
```
nameserver 8.8.8.8
```  

### `Firefox` не открывает `ranger`
#### Выполняем следущие команды:  
```
cp /usr/share/applications/ranger.desktop ~/.local/share/applications/ranger.desktop 
vim ~/.local/share/applications/ranger.desktop
```  
#### Изменить эти параметры:

```
Terminal=false
Exec=alacritty -e ranger
```  
#### И в конце вводим в терминал:
```
update-desktop-database ~/.local/share/applications
```

## Оптимизация ( в процессе... )
### Установка `Zen` ядра 
```
sudo pacman -S linux-zen linux-zen-headers
```
### Zramswap
### Ananicy
### Irqbalance


## Интересные штуки
#### Узнаем сколько уже установлена система
```
dumpe2fs $(mount | grep 'on \/ ' | awk '{print $1}') | grep 'Filesystem created:'
```
