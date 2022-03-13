# Born2Beroot

#Сделать snapshot 
command t

#Виртуальная машина (ВМ или VM) — это виртуальный компьютер, который использует выделенные ресурсы реального компьютера (процессор, диск, адаптер). 
Эти ресурсы хранятся в облаке и позволяют ВМ работать автономно. Простыми словами, виртуальная машина позволяет создать на одном компьютере ещё один компьютер, 
который будет использовать его ресурсы, но работать изолированно.

#Дистрибутивы
1.
За Debian-ом — почти 25 лет истории успешного развития и эксплуатации по строгим канонам “истинного GNU”.
За CentOS-ом — код мощнейшего коммерческого Red Hat Enterprise Linux-а (на базе которого он написан и модернизируется), с вытекающей поддержкой и благоволением 
компании Red Hat.
2.
Debian
CentOS - количество програм в официальном репозитории меньше, но более надежный и стабильный. Иногда такая политика приводит к необходимости 
не только подключения сторонних репозиториев, но и танцев с бубном для сборки и установки нестандартных решений.
3. 
Обновление релизов (версии системы) в CentOS происходит намного более предсказуемо, чем в Debian (особенно, если учесть прецеденты смены названия некоторых 
дебиановски файлов конфигурации, да и не только их).
CentOS major releases typically have a 10-year lifespan. 
4. 
В целом, политика Debian лучше всего характеризуется фразой “as is” — в репозитории огромное количество пакетов, их версии достаточной новые, однако 
“никто ни за что не отвечает”. 
CentOS также формально не возлагает на себя никаких обязательств, однако не стоит забывать — “говорим CentOS, подразумеваем Red Hat Enterprise Linux”.
Debian typically releases a new major version on a 2-year release cycle with 3 years of full support and an additional 2 years of LTS (Long Term Support) 
for a 5-year lifespan, so being able to upgrade to the next stable major release is handy.
5. 
Debian - имеет больше инструкций и документации, богатство man-ов и готовых инструкций “how to”
CentOS - меньше
6. 
Debian - если проект предусматривает большое количество различных сторонних компонент, сервисов и служб, и все это нужно запустить как можно быстрее
CentOS - если же во главу угла поставлена “железная” стабильность
7. 
CentOS uses the RPM package format and YUM/DNF as the package manager.
Debian uses the DEB package format and dpkg/APT as the package manager.

#LVW
Менеджер логических томов (англ. logical volume manager) — подсистема операционных систем Linux и OS/2, позволяющая использовать разные области одного жёсткого 
диска и/или области с разных жёстких дисков как один логический том. Реализована с помощью подсистемы device mapper.
Команда lsblk

#sudo
sudo позволяет разрешенному пользователю выполнять команду как суперпользователь или другой пользователь. По умолчанию sudo требует, что бы пользователи 
аутентифицировали себя при помощи пароля (ЗАПОМНИТЕ: это пароль пользователя, не пароль root). Как только пользователь аутентифицировал себя происходит 
обновление временной метки и пользователь может использовать sudo некоторый период времени без пароля (по умолчанию пять минут, если в sudoers не указано другое). 
su my directory superuser
su - root superuser
apt install sudo -sudo installation
Check if user is in sudo group -> getent group sudo
Open sudoers file: -> sudo visudo

#Aptitute-apt
aptitute -графический интерфейс для apt

#vi - упрощенная версия vim

#НАСТРОЙКА SSH
// правим конфиг ssh (будучи под  root)
$nano /etc/ssh/sshd_config
// Меняем строки
Port 4242 //потому что Port 22 по дефолту в ssh открыт
PermitRootLogin no -чтобы никто не мог залогиниться под  root = труднее взломать

#ROOT-ПРАВА ПОЛЬЗОВАТЕЛЮ
$usermod -aG root <user>  //созданному пользователю назначаем права администратора
// правим конфиг sudo
$nano /etc/sudoers
// добавляем строку с именем пользователя
<user>    ALL=(ALL:ALL) ALL //use visudo as root
// бэкапим конфиг sudo 
$cp /etc/sudoers /etc/sudoers.backup //  сохраняем на всякий случай
shutdown now

#ДОСТУП ЧЕРЕЗ ТЕРМИНАЛ
Проброс портов в virtualbox
1. Заходим settings -> network
2. Port Forwarding -> в host port вводим 4242, в guest port вводим 4242
TERMINAL
1. в iTerm вводим ssh <user>@localhost -p 4242 ???

# проверка активности ssh.servis: (as user) sudo systemctl status ssh

#НАСТРОЙКА ВХОДА ПОД SUDO
// логинимся под рутом
$su -
// проверяем группы пользователей ???
$groups root 
$groups <user>
// проверяем и/или меняем имя машины
$nano /etc/hostname
// проверяем имя хоста
$nano /etc/hosts
// ещё раз правим конфиг судо
$nano /etc/sudoers
// меняем фразу при неудачном вводе
Defaults  badpass_message="Your phrase" (до изменений было mail_badpass)
// Устанавливаем количество попыток ввода
Defaults        passwd_tries=3

#СОЗДАНИЕ ГРУППЫ
// создаём группу
$addgroup user42
// помещаем туда нашего пользователя
$usermod -aG user42 <user>
посмотреть, в какой мы группе -> groups

#ФАЙЕРВОЛ
UFW (Uncomplicated Firewall) –  удобный интерфейс для управления политиками безопасности межсетевого экрана. Сам по себе ufw - это не брандмауэр, это инструмент 
для настройки конфигурации netfilter / iptables. Он зарегистрирован как сервис, потому что его нужно запускать каждый раз при запуске машины.

// устанавливаем ufw
$apt-get install ufw
// убеждаемся, что он не работает
$ufw status
// запускаем файервол
$ufw enable
// убеждаемся, что он работает
$ufw status
// даём дефолтные настройки
$sudo ufw default deny incoming
$sudo ufw default allow outgoing
// разрешаем наш порт ssh
$ufw allow 4242

#ПОЛИТИКА ПАРОЛЕЙ
// устанавливаем утилиту политики
$apt-get install libpam-pwquality
ПОЛИТИКА ПАРОЛЕЙ
// правим файл политики паролей
%nano /etc/login.defs
// меняем значения строк как здесь:
PASS_MAX_DAYS 30
PASS_MIN_DAYS 2
PASS_WARN_AGE 7
// на всякий пожарный бэкапим конфиг
$cp /etc/pam.d/common-password /etc/pam.d/common-password.backup
// правим конфиг
$nano /etc/pam.d/common-password
// дописываем в эту строку следующим образом:
password    requisite         pam_pwquality.so retry=3 (lcredit =-1) ucredit=-1 dcredit=-1 maxrepeat=3 usercheck=0 difok=7 enforce_for_root
// меняем пароли пользователей
// в соответствии с политикой:
$passwd user
$passwd root
reboot

#cp
cp --help

#Выйти из root вводим exit либо просто пишем su user

#Удалить файл rm 

#СОЗДАНИЕ СКРИПТА
// для утилиты netstat
$apt-get install net-tools
$su user
$cd ~/
$touch monitoring.sh
$chmod +x ./monitoring.sh //право на исполнения скрипта (файла)
//далее будем вызывать его по команде ./monitoring.sh
$nano monitoring.sh
// заполняем скрипт командами
// по примеру из этого гита

#НАСТРОЙКА CRON
cron — классический демон (компьютерная программа в системах класса UNIX), использующийся для периодического выполнения заданий в определённое время. 
Регулярные действия описываются инструкциями, помещенными в файлы crontab и в специальные каталоги.
// добавляем скрипт в расписание 
$crontab -e
*/10 * * * *	sh /home/<user>/monitoring.sh

#НАСТРОЙКИ TTY
Это система, которая позволяет разбить shell на несколько подshellов.
(Зажав Alt Control F2, из Linux можно попасть в чистый shell (без графики). Т.е. TTY - это разные экземпляры одной системы.
see logind.conf(5)
// правим конфиг tty
$nano /etc/systemd/logind.conf
// раскомментировать и изменить:
NAutoVTs=8
ReserveVT=8

#ЛОГИРОВАНИЕ SUDO
Необходимо логировать действия с sudo, потому что это политика безопасности. 
// создаём каталог логирования:
$mkdir /var/log/sudo
$touch /var/log/sudo/sudo.log
// ещё раз правим конфиг судо
$nano /etc/sudoers
// добавляем строку в секцию defaults
Defaults        logfile=/var/log/sudo/sudo.log
// совершаем действие под пользователем через sudo
// проверяем, заолгировалось ли оно:
$nano /var/log/sudo/sudo.log
