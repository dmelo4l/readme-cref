# Atualizar o Zabbix Agent

yum clean all

yum makecache

systemctl stop zabbix-agent

dnf install zabbix-agent2* --nogpgcheck

systemctl start zabbix-agent2

zabbix_agent2 -V

setsebool -P zabbix_can_network 1

setsebool -P daemons_enable_cluster_mode 1

sestatus

systemctl start zabbix-agent2

systemctl status zabbix-agent2

ausearch -m AVC -ts boot

tail -f /var/log/zabbix/zabbix_agentd.log


----------------------------------------------------------------------------------


# Permissive

### Passo 1: Verificar o estado atual
Antes de começar, confirme se ele realmente está desativado:

```Bash
sestatus
```
Se o retorno for SELinux status: disabled, siga para o próximo passo.


### Passo 2: Alterar a configuração (O pulo do gato)
Você precisa editar o arquivo principal de configuração. Cuidado aqui: se você escrever algo errado, o servidor pode não subir após o reboot.

Abra o arquivo:
```Bash
vim /etc/selinux/config
```

#### Localize a linha que começa com `SELINUX=`.

Altere de `disabled` para `permissive`.

Deve ficar exatamente assim:
`SELINUX=permissive`

Salve e saia (:wq).

Valide a mudança:
```Bash
cat /etc/selinux/config
```

Se você quiser ganhar tempo, pode usar este comando único (em vez de abrir o editor) para alterar o arquivo:
```Bash
sed -i 's/^SELINUX=disabled/SELINUX=permissive/' /etc/selinux/config
```


### Passo 3: O arquivo de sinalização (Crucial!)
Quando o SELinux é ativado pela primeira vez em um sistema que estava desativado, todos os arquivos do disco precisam ser "carimbados" (rotulados). Para que o Linux faça isso automaticamente no próximo boot, execute:

```Bash
touch /.autorelabel
```
Nota: Esse comando cria um arquivo vazio na raiz. O sistema verá esse arquivo no boot, fará o processo de rotulagem e depois apagará o arquivo sozinho.



### Passo 4: Reiniciar o servidor
Infelizmente, para passar de disabled para permissive, o kernel precisa ser carregado com o SELinux ativo.

```Bash
reboot
```


### Passo 5: Validar após o retorno
Após o servidor voltar (pode demorar um pouco mais que o normal devido ao relabel), logue novamente e verifique:

```Bash
sestatus
```
O status deve aparecer como enabled, mas o "Current mode" deve ser `permissive`.


```Bash
grep "avc:  denied" /var/log/audit/audit.log
```


### Plano de Ação para os 23 servidores
Como essa tarefa envolve reboot, você não pode fazer todos de uma vez se eles fizerem parte de um cluster ou serviço crítico.

- Escalonamento: Reinicie um por um e espere o serviço (como o Zabbix que você mencionou antes) subir totalmente antes de ir para o próximo.

- Monitoramento de Violações: Para ver o que o SELinux registraria como bloqueio (as "violações" da descrição), você poderá usar este comando futuramente:

```Bash
ausearch -m AVC -ts recent
```


Precisa fazer algo a mais?
Para fechar a tarefa com chave de ouro e garantir que você seguiu a "Descrição" à risca, faltam dois detalhes operacionais:

Validar o Log de Violações:
A descrição da tarefa menciona "registrando violações". Para provar que ele está funcionando, rode este comando:
```Bash
grep "avc:  denied" /var/log/audit/audit.log
```

Se aparecerem linhas, são as ações que seriam bloqueadas se ele estivesse em modo Enforcing. Se não aparecer nada, é porque o servidor ainda não teve nenhuma tentativa de acesso negada pelas regras.



------------------------------------------------------------------------------------



# Enforcing

### 1. Validação Inicial
Antes de mexer em qualquer coisa, confirme se o servidor está pronto para a migração.
* **Comando:** `sestatus`
* **O que conferir:** O status deve ser `enabled` e o modo `permissive`. Se estiver `disabled`, você precisará do reboot (aquele processo de `touch /.autorelabel` que fizemos antes).

### 2. Conferir o que seria bloqueado
Verifique se existem "violações" registradas enquanto o servidor estava em modo permissivo.
* **Comando:** `sudo ausearch -m AVC -ts recent`
* **O que conferir:** Se aparecer `<no matches>`, está limpo. Se aparecerem linhas de `denied`, você precisa liberar antes de ativar o bloqueio.

### 3. Liberar o Zabbix (Preparar o terreno)
Como vimos que o Zabbix costuma ser bloqueado, execute estes comandos para dar as permissões necessárias "antecipadamente".
* **Comando 1:** `sudo setsebool -P zabbix_can_network 1` (Libera a rede para o Zabbix).
* **Comando 2:** `sudo setsebool -P daemons_enable_cluster_mode 1` (Libera a comunicação interna do Zabbix).

### 4. Revalidar a limpeza
Após rodar as liberações acima, limpe a tela e verifique se o log de erros parou.
* **Comando:** `sudo ausearch -m AVC -ts recent`
* **O que conferir:** Agora deve aparecer `<no matches>`. Se aparecer, o caminho está livre para o próximo passo.

### 5. Ativar o Modo Enforcing (Bloqueio Real)
Agora que o log está limpo, ative o bloqueio na memória do servidor.
* **Comando:** `sudo setenforce 1`
* **Para que serve:** Faz o SELinux começar a bloquear de fato qualquer ação não autorizada.

### 6. Tornar a configuração Permanente
Se você não fizer este passo, quando o servidor reiniciar ele voltará para o modo anterior.
* **Comando:** `sudo sed -i 's/SELINUX=permissive/SELINUX=enforcing/' /etc/selinux/config`
* **Para que serve:** Altera o arquivo de configuração de forma automática para `enforcing`.

### 7. Confirmar que está tudo certo
A validação final para você marcar como "Concluído" no seu checklist.
* **Comando 1:** `sestatus` (Confirme se o `Current mode` é `enforcing`).
* **Comando 2:** Verifique o Dashboard do Zabbix. Se o servidor continuar "Verde" e coletando dados, a migração foi perfeita.


Se em algum servidor o Passo 5 (`setenforce 1`) fizer o Zabbix parar, você pode voltar para o modo seguro instantaneamente rodando `sudo setenforce 0`.


systemctl status zabbix-proxy

systemctl status zabbix-agent2


