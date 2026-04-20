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




