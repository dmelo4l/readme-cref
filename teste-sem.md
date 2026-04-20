## 1. Copie o conteudo atualizado do script "backup_zabbix_basico.sh" 
```bash
vim /usr/local/bin/backup_zabbix.sh
```

## 2. SEGREDO: Limpe quebras de linha defeituosas do Windows antes de qualquer coisa
```bash
sed -i 's/\r//' /usr/local/bin/backup_zabbix.sh
```

## 3. Limpe algum lixo que possa ter ficado da tentativa antiga 
```bash
rm -rf /var/backups/backup-zabbix-repo
```

## 4. Ajuste permissoes e GO!
```bash
chmod +x /usr/local/bin/backup_zabbix.sh

/usr/local/bin/backup_zabbix.sh
```


## Para adicionar ao cron, execute no servidor como root:
```bash
crontab -e
```

## Rodar todo dia às 03:00 da manhã (recomendado)
```bash
# Maquina 001 - 03:01
1 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 002 - 03:02
2 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 003 - 03:03
3 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 004 - 03:04
4 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 005 - 03:05
5 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 006 - 03:06
6 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 007 - 03:07
7 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 008 - 03:08
8 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 009 - 03:09
9 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 010 - 03:10
10 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 011 - 03:11
11 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 012 - 03:12
12 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 013 - 03:13
13 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 014 - 03:14
14 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 015 - 03:15
15 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 016 - 03:16
16 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 017 - 03:17
17 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 018 - 03:18
18 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 019 - 03:19
19 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 020 - 03:20
20 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 021 - 03:21
21 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 022 - 03:22
22 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 023 - 03:23
23 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 024 - 03:24
24 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 025 - 03:25
25 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 026 - 03:26
26 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 027 - 03:27
27 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

# Maquina 028 - 03:28
28 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1

```

## Ver o cron do root
```bash
crontab -l
```

## Testar que o script tem permissao de execucao
```bash
ls -la /usr/local/bin/backup_zabbix.sh
```




