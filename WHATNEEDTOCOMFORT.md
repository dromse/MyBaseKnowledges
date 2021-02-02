# Использование Linux в повседневной жизни

В этом списке перечислены программы, источники на ресурсы и тд. которые пригодились мне для комфортного использования GNU/Linux. 

Так же, тут будут размещены проблемы и их решения, с которыми я столкнулся на протяжении использования GNU/linux.


## Решениe проблем:
### Монтирование дополнительного диска на ntfs
```
# /dev/sda1
 11 /dev/sda1   /home/sshd  ntfs        nls-utf8,umask-0222,uid-1000,gid-1000,rw    0 0
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

### Поменять местами esc и caps
 ```
 setxkbmap -option caps:swapescape
 ```

### Нет звука:
Проверьте есть ли pulseaudio или alsa, если нет то установите их.  
Если звука нет, попробуйте запустить pulseaudio:  
```
pulseaudio -D
```  
Если звук отсутсвует после перезагрузки то, пропишите в конфигах запуск пульса, например как у меня в конфиге i3wm:  
```
exec "pulseaudio -D"
```

Поправочка: на Arch хватает просто установить пакеты из репозитория, и дальше все само подхватывается, ну или после перезагрузки ПК.

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
Установить picom.  
Если есть траблы, то проверить создался ли конфигурационый файл с дефолтными настрйками, если нет, то создайте его.  
Также если picom не видить файл конфигурации то, укажите к нему путь:  
```
picom --config КОНФИГ_ФАЙЛ
```

Поправочка: Или установить чтобы оконный менеджер запускался с дискретной видеокартой.

### Отсутсвует подключение к wifi:
Тут два варианта: или вы реально не подключены, или у вас не настроен DNS   
Если первое, то надо настроить подключение к wifi, есть куча способов как это сделать в инете, но на всякий случай приведу пару ссылок:  
Ссылки утонули ищите их сами.

Если второе, то потребуется настроить DNS в resolv.conf прописав внутрь файла какой-то адресс DNS сервера, например DNS адресс гугла 8.8.8.8:  
```
nameserver 8.8.8.8
```  

### Firefox не открывает ranger
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
### Установка Zen ядра 
### Zramswap
### Ananicy
### Irqbalance

## Приложения:
1. Файловый менеджер: ranger
1. Терминал: st, alacrity
1. Оболочка терминала: bash, fish
1. Браузер: firefox
1. Текстовый редактор: neovim
1. Программа для скриншотов: spectecle
1. LED подсветка для клавиш и мышки: [rogauracore](https://github.com/wroberts/rogauracore), piper(вроде как можно установить из любого пакетного менеджера, покрайней мери на Ubuntu/Arch/PopOS/Manjaro/Debian(?)/Void Linux, так что не привожу никаких ссылок).
0. Офиссный пакет: [OnlyOffice](https://flathub.org/apps/details/org.onlyoffice.desktopeditors)
0. Графическая часть: 
    - i3wm + polybar + i3-gaps(оконный менеджер) 
    - feh(для обоев) 
    - xorg(штучка для запуска всяких графических интерфейсов, ну ещё есть wayland, в котором по дефолту нет тиринга, но он пока-что ещё сырой, и там вроде какие-то траблы с nvidia, хотя я играл в watchdogs 2, через lutris, вроде как производительность одна и таже, что на винде) 
    - nvidia-driver(драйвера nvidia)
    - lightdm(оконный менеджер)
0. Программа читалка(для pdf, fb2 и тд.): Okular, Zathura
0. Менеджер виртуальных машин: QEMU, VirtualBox
0. Палитра цветов: Color Picker
0. Аудио: alsa-utils, pulseaudio, pulseaudio-utils

## Настройка NEOVIM'a
### Первоначальная настройка
```
# rows number
set number

# 4 space in one tab
set expandtab
```
### Работа с плагинами
Установить [plug-vim](https://github.com/junegunn/vim-plug) для работы с плагинами.  
### Плагины
#### Древо файлов
```
Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle'}
------------------------------------------------------
" mapping
map <C-n> :NERDTreeToggle<CR>
```
#### Автокомплит для символов 
```
" ''``{}()[]
Plug 'townk/vim-autoclose'
```

## Настройка i3wm, lightdm, polybar
### Настрока i3wm
#### Монтирование дисков на главном экране
#### Подключение wifi на главном экране

### Настройка lightdm

### Настройка polybar
#### launch.sh
Помещаем код:  
```
#!/usr/bin/env bash
name=example
pkill polybar
if type "xrandr"; then
    for monitor in $(xrandr --query | grep " connected" | cut -d" " -f1); do
        MONITOR=${monitor} polybar --reload ${name}&
    done
else
    polybar --reload ${name} &
fi
```

## Интересные штуки
#### Узнаем сколько уже установлена система
```
dumpe2fs $(mount | grep 'on \/ ' | awk '{print $1}') | grep 'Filesystem created:'
```
