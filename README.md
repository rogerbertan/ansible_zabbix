# Implementação do Zabbix com Ansible

Este projeto é parte de um Trabalho de Conclusão de Curso (TCC) do curso de Engenharia de Computação. No contexto da atual era digital, onde a tecnologia é a base das operações e estratégias empresariais, o monitoramento eficaz de sistemas e infraestruturas de Tecnologia da Informação (TI) tornou-se um pilar indispensável para a manutenção integral dos negócios. Nesse cenário, a capacidade de implementar e configurar rapidamente ambientes de monitoramento ganha uma importância ainda maior, o que, em contrapartida, pode trazer consigo desafios substanciais.

Esta pesquisa busca oferecer um estudo aprofundado de como a automação, em particular, por meio da ferramenta Ansible, pode efetivamente enfrentar os desafios de sistemas complexos, acelerar o processo de implantação e manter a uniformidade das configurações. Essa abordagem automatizada tem o potencial de revolucionar a forma como as organizações gerenciam suas infraestruturas de TI, garantindo maior estabilidade e escalabilidade em todo o processo. Sendo assim, considera-se este estudo de grande relevância, pois aborda um problema real enfrentado por organizações.

O objetivo deste trabalho é avaliar os benefícios da automação com o Ansible na implantação de sistemas de monitoramento, como o Zabbix, visando reduzir erros, garantir configurações consistentes e otimizar o tempo de implantação. Para atingir o objetivo, as etapas incluem definir a infraestrutura de TI, desenvolver playbooks e roles com o Ansible, implementar e configurar o Zabbix, avaliar erros, verificar a consistência das configurações, analisar o tempo de implementação, documentar resultados e identificar melhorias. Além de visar a otimização de processos internos, a pesquisa busca também contribuir para o avanço do conhecimento em automação e monitoramento, fornecendo um guia para eficiência e excelência operacional, neste quesito, às organizações.

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
