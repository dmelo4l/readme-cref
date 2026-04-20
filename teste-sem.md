## 1. Apagar o repo local antigo (deixa o script recriar)
```bash
rm -rf /var/backups/backup-zabbix-repo
```

## 2. Atualizar o script (cole o conteúdo novo)
```bash
vi /usr/local/bin/backup_zabbix.sh
```

## 3. Limpar CRLF e executar
```bash
sed -i 's/\r//' /usr/local/bin/backup_zabbix.sh

/usr/local/bin/backup_zabbix.sh
```



## Para adicionar ao cron, execute no servidor como root:
```bash
crontab -e
```

## Rodar todo dia às 03:00 da manhã (recomendado)
```bash
0 3 * * * /usr/local/bin/backup_zabbix.sh >> /var/log/backup_zabbix.log 2>&1
```

## Ver o cron do root
```bash
crontab -l
```

## Testar que o script tem permissao de execucao
```bash
ls -la /usr/local/bin/backup_zabbix.sh
```




