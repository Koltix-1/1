
**Обновление списков пакетов и системы:**
``` Bash
sudo apt update && sudo apt upgrade -y
```

Автоматическая установка доступных драйверов:
``` bash
sudo ubuntu-drivers autoinstall
```

**Тонкая настройка параметров ядра:** Для изменения параметров ядра используем утилиту `sysctl`. Оптимизируем максимальное количество открытых файлов и использование swap-памяти:
```Bash
sudo sysctl -w fs.file-max=2097152
sudo sysctl -w vm.swappiness=10
# Чтобы настройки сохранились после перезагрузки:
echo "fs.file-max = 2097152" | sudo tee -a /etc/sysctl.conf 
echo "vm.swappiness = 10" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

Установка утилит для проверки сети и SSH-сервера:
``` bash
sudo apt install -y openssh-server iputils-ping net-tools curl
```
`openssh-server` - устанавливает SSH‑сервер. Позволяет подключаться к машине удалённо по протоколу SSH (например, через `ssh user@host`)
`iputils-ping` — содержит утилиту `ping`
**`net-tools`** — набор устаревших, но по‑прежнему популярных сетевых утилит. Включает `ifconfig`, `netstat`,  `route`
`curl` - для передачи данных по ссслыкам https

Запуск и включение службы SSH в автозагрузку:
```Bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

**Включение межсетевого экрана (UFW) и открытие портов:**
```Bash
sudo ufw allow ssh
sudo ufw enable
```

**Демонстрация проверки сетевого соединения:**
```Bash
ip route show | grep default
ping -c 3 google.com
ifconfig
```

Установка базовых утилит
```bash
sudo apt install -y build-essential wget git lm-sensors htop
```
`build-essential` - метапакет для компиляций
`wget` - длс скачивания с сайтов
`git` - для работы с версиями кода
`lm-sensors` -  датчики железа
`htop` - список процессов

Установка виртуального принтера
```bash
sudo apt install -y cups cups-pdf
```

**Запуск службы принтера:**
```Bash
sudo systemctl enable cups
sudo systemctl start cups
```
**Регистрация виртуального PDF-принтера в системе(возможно не надо):**
```Bash
sudo lpadmin -p Virtual_PDF_Printer -v cups-pdf:/ -E -m raw
```

Установка утилиты для создания точек восстановления
```bash
sudo apt install -y timeshift
```

Создание точки восстановления
```bash
sudo timeshift --create --comments 'Clean OS'
```

**Создание резервной копии важных конфигураций и логов (Резервное копирование):**
``` bash
sudo mkdir -p /var/backups/system
sudo tar -czf /var/backups/system/etc_backup_$(date +%F).tar.gz /etc /var/log/auth.
```

**Создание установочного образа раздела :** Архивация или создание `.img` снимка системного диска (замени `/dev/sda1` на твой корень при необходимости):

```Bash
sudo tar if=/dev/sda1 of=/var/backups/system_root_image.img bs=4M status=progress
```

Создание группы пользователей и выдача прав

```bash
sudo groupadd name
sudo usermod -aG name $USER
sudo mkdir /opt/name_dates
sudo chown root:name /opt/name_dates
sudo chmod 770 /opt/name_dates
```

Установка утилиты для логирования
```bash
sudo apt install -y fail2ban
```

**Включение защиты от подбора паролей и аутентификации:**

```Bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

Ужесточение политик аутентификации (Срок действия паролей до 90 дн): 
```bash
sudo sed -i 's/PASS_MAX_DAYS\t99999/PASS_MAX_DAYS\t90/g' /etc/login.defs
```

Настройка журнала мониторинга (Добавление кастомных правил логирования для аудита) (мб не надо) 
```bash
echo "auth,authpriv.* /var/log/auth_monitor.log" | sudo tee -a /etc/rsyslog.d/50-default.conf 
sudo systemctl restart rsyslog
```

Просмотр журналов мониторинга (Аутентификация/Авторизация):
```bash
sudo tail -n 20 /var/log/auth.log
sudo fail2ban-client status
```

Установка антивируса ClamAv
``` bash
sudo apt install -y clamav clamav-daemon
```

Остановка антивируса, обновление базы, Запуск антивируса
```bash
sudo systemctl stop clamav-freshclam
sudo freshclam
sudo systemctl start clamav-freshclam
```

Параметры совместимости ПО (хз надо ли)
```bash
echo "QT_AUTO_SCREEN_SCALE_FACTOR=0" | sudo tee -a /etc/environment 
echo "GDK_SCALE=1" | sudo tee -a /etc/environment
echo "GDK_DPI_SCALE=1" | sudo tee -a /etc/environment 
echo "WINE_DISABLE_CONTAINS_GRIDS=1" | sudo tee -a /etc/environment
```

Далее по заданию...
Установка драйверов nvidia
``` bash
sudo apt install -y nvidia-cuda-toolkit
```

Установка Python
``` bash
sudo apt install -y python3-pip python3-venv
```

 Создание проекта, виртуального окружения
 ```bash
 mkdir ~/name_project && cd ~/name_project
 python3 -m venv venv
 source venv/bin/activate
```
