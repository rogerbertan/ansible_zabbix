# Implementação do Zabbix com Ansible

Este repositório contém playbooks do Ansible para a instalação automatizada do Zabbix Server e do Zabbix Agent, além de arquivos de configuração necessários.

## Estrutura do Repositório

- `hosts`: Arquivo de inventário do Ansible contendo as definições dos servidores alvo.
- `playbooks/`: Diretório contendo os playbooks do Ansible e arquivos de configuração.
  - `install_zabbix-server.yaml`: Playbook para instalação e configuração do Zabbix Server.
  - `install_zabbix-agent.yaml`: Playbook para instalação e configuração do Zabbix Agent.
  - `zabbix_agentd.conf.j2`: Template de configuração do Zabbix Agent.

## Requisitos

- Ansible 2.16.5+ instalado em sua máquina de gerenciamento.
- Acesso sudo aos servidores alvo.
- Chaves SSH para os servidores alvo.

## Instruções de Instalação

### Passo 1: Configurar o Inventário

Edite o arquivo `hosts` para incluir os endereços IP ou nomes dos seus servidores:

```ini
[ubuntu_server]
192.168.0.201
192.168.0.202

[fedora]
192.168.0.203

[server]
192.168.0.201

[agent]
192.168.0.202
192.168.0.203

[linux:children]
ubuntu_server
fedora
```
No exemplo acima:

- O grupo `[ubuntu_server]` contém dois servidores Ubuntu.
- O grupo `[fedora]` contém um servidor Fedora.
- O grupo `[server]` contém o servidor onde o Zabbix Server será instalado.
- O grupo `[agent]` contém os servidores onde o Zabbix Agent será instalado.
- O grupo `[linux:children]` inclui tanto os servidores Ubuntu quanto o Fedora.

### Passo 2: Configurar Variáveis

Abra os playbooks em `playbooks/install_zabbix-server.yaml` e `playbooks/install_zabbix-agent.yaml` e ajuste as variáveis conforme necessário, especialmente as relacionadas ao MySQL e às senhas do Zabbix.

### Passo 3: Executar os Playbooks

1. Execute o playbook para instalar o Zabbix Server:
```
ansible-playbook -i hosts playbooks/install_zabbix-server.yaml -bK
```
2. Execute o playbook para instalar o Zabbix Agent:
```
ansible-playbook -i hosts playbooks/install_zabbix-agent.yaml -bK
```

## Detalhes dos Playbooks
### `install_zabbix-server.yaml`
Este playbook realiza os seguintes passos:

1. Instala o Zabbix Server e suas dependências.
2. Configura o Zabbix Server para iniciar automaticamente.
3. Instala o MySQL e configura o banco de dados para o Zabbix.
4. Aplica as configurações do Zabbix Server.

### `install_zabbix-agent.yaml`
Este playbook realiza os seguintes passos:

1. Instala o Zabbix Agent nos servidores definidos no grupo agent.
2. Usa o template `zabbix_agentd.conf.j2` para configurar o agente.
3. Inicia e habilita o serviço do Zabbix Agent.

### `zabbix_agentd.conf.j2`
Este é um arquivo de template Jinja2 usado para configurar o Zabbix Agent. Ele deve ser personalizado de acordo com as necessidades do seu ambiente.

## Suporte
Para suporte ou mais informações, entre em contato pelo roger.bertan@gmail.com.
