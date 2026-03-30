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
sudo vim /etc/selinux/config
```

#### Localize a linha que começa com `SELINUX=`.

Altere de `disabled` para `permissive`.

Deve ficar exatamente assim:
`SELINUX=permissive`

Salve e saia (:wq).

```Bash
/etc/selinux/config
```

Se você quiser ganhar tempo, pode usar este comando único (em vez de abrir o editor) para alterar o arquivo:
```Bash
sudo sed -i 's/^SELINUX=disabled/SELINUX=permissive/' /etc/selinux/config
```


### Passo 3: O arquivo de sinalização (Crucial!)
Quando o SELinux é ativado pela primeira vez em um sistema que estava desativado, todos os arquivos do disco precisam ser "carimbados" (rotulados). Para que o Linux faça isso automaticamente no próximo boot, execute:

```Bash
sudo touch /.autorelabel
```
Nota: Esse comando cria um arquivo vazio na raiz. O sistema verá esse arquivo no boot, fará o processo de rotulagem e depois apagará o arquivo sozinho.



### Passo 4: Reiniciar o servidor
Infelizmente, para passar de disabled para permissive, o kernel precisa ser carregado com o SELinux ativo.

```Bash
sudo reboot
```


### Passo 5: Validar após o retorno
Após o servidor voltar (pode demorar um pouco mais que o normal devido ao relabel), logue novamente e verifique:

```Bash
sestatus
```
O status deve aparecer como enabled, mas o "Current mode" deve ser `permissive`.



### Plano de Ação para os 23 servidores
Como essa tarefa envolve reboot, você não pode fazer todos de uma vez se eles fizerem parte de um cluster ou serviço crítico.

- Escalonamento: Reinicie um por um e espere o serviço (como o Zabbix que você mencionou antes) subir totalmente antes de ir para o próximo.

- Monitoramento de Violações: Para ver o que o SELinux registraria como bloqueio (as "violações" da descrição), você poderá usar este comando futuramente:

```Bash
sudo ausearch -m AVC -ts recent
```


Se você quiser ganhar tempo, pode usar este comando único (em vez de abrir o editor) para alterar o arquivo:
```Bash
sudo sed -i 's/^SELINUX=disabled/SELINUX=permissive/' /etc/selinux/config
```



