# Мониторинг количества сессий RDS с помощью Zabbix

1. RDSH - группа в AD, в которую добавляются хосты для RAP/CAP политик на шлюзе. Из них берутся только RDS* имена.
2. RDUserCount_job добавляем в шедулер для выгрузки всех сведений по RDS хостам (если делать запрос не из базы, за время получения метрики данные могут измениться и статистика будет неверной) раз в минуту в файл operational_data.json.
3. В конфигурационный файл Zabbix добавляем строчку: `UserParameter=type[*],powershell.exe -nologo -NoProfile -File "C:\zabbix\scripts\RDUsersCount.ps1" -server $1 -type $2`
4. В zabbix создаем элемент данных в узле, на котором размещены сценарии и конфигурационный файл выше. Указываем ключ `type[host.domain.local,a]`, остальное произвольно.

![dashboard](https://github.com/MaxShalkov/RDUsersCount/blob/main/screen.png?raw=true)