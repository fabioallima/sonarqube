# Instalador do SonarQube com Docker em Alpine Linux

Este projeto automatiza a configuração do SonarQube em Alpine Linux usando o Docker dentro do Subsistema Windows para Linux (WSL). Ele simplifica o processo de baixar a imagem do Alpine Linux, configurar o Docker e executar o SonarQube em um ambiente conteinerizado.

## Pré-requisitos

Antes de executar este script, certifique-se de ter o seguinte:

- O Subsistema do Windows para Linux (WSL) habilitado em seu computador com Windows.
- O PowerShell instalado em seu computador com Windows.
- Acesso à internet para baixar o Alpine Linux e os pacotes do Docker.

## Instalação

1. **Definir Variáveis Básicas**: O script começa definindo variáveis essenciais como a versão do Alpine Linux, arquitetura (que pode ser ajustada para diferentes hardwares como `armhf`, `aarch64`, etc.) e o nome base do modelo.

2. **Baixar o Alpine Linux**: Ele constrói uma URL para baixar o arquivo tar.gz do Alpine Linux para a versão e arquitetura especificadas. Em seguida, verifica se o diretório `.wsl` existe no perfil do usuário, criando-o se necessário, e baixa a imagem do Alpine Linux para lá.

3. **Importar a Imagem do Alpine para o WSL**: Após o download, o script cria um diretório para a distribuição dentro de `.wsl`, se ainda não existir. Então, importa a imagem baixada do Alpine para o WSL, atribuindo a ela o nome `sonarqube`.

4. **Instalar o Docker**: Com o Alpine Linux configurado, o script procede à instalação do Docker atualizando o índice de pacotes, atualizando o sistema e instalando o Docker junto com dependências necessárias. Ajusta os parâmetros do sistema para suportar o Docker e inicia o daemon do Docker.

5. **Verificar Instalação do Docker**: Para garantir que o Docker está corretamente instalado e em execução, o script executa o comando `docker version`, verificando os componentes cliente e servidor. Ele exibe as versões se ambas as verificações passarem, indicando uma configuração bem-sucedida.

6. **Instalar o Docker Compose**: Além disso, ele instala o Docker Compose usando o gerenciador de pacotes do Alpine.

7. **Configurar o SonarQube**: O script copia um arquivo `docker-compose.yml`, que deve definir o serviço SonarQube, para o diretório raiz do Alpine. Ele lista os conteúdos de `/

## Visão Geral do Script

O script realiza as seguintes tarefas:

- Baixa a versão especificada do Alpine Linux do repositório oficial do Alpine Linux.
- Configura a distribuição do Alpine Linux dentro do ambiente WSL.
- Instala o Docker e o Docker Compose para facilitar a execução do SonarQube em contêineres.
- Verifica a instalação bem-sucedida do Docker e do Docker Compose.

# Iniciando o Serviço SonarQube

Após instalar o SonarQube conforme as instruções de instalação acima, você pode usar o script fornecido para iniciar o serviço SonarQube.

## Script para Iniciar o SonarQube

O script `Start.ps1` é projetado para iniciar o serviço SonarQube usando o Docker dentro da sua distribuição WSL. As etapas realizadas pelo script são:

1. Define o `vm.max_map_count` para um valor apropriado necessário pelo SonarQube.
2. Inicia o daemon do Docker em segundo plano.
3. Aguarda alguns segundos para garantir que o daemon do Docker tenha sido iniciado.
4. Usa o `docker-compose` para lançar os serviços definidos no arquivo `docker-compose.yml`.
5. Exibe uma mensagem de confirmação uma vez que o script tenha sido executado com sucesso.

## Uso

1. Navegue até o diretório que contém o script `Start.ps1`.
2. Execute o script digitando `./Start.ps1` no seu PowerShell.

Este script deve ser executado após o script de configuração inicial ter sido executado com sucesso e sempre que você desejar iniciar o serviço SonarQube.


## Estrutura de Arquivos

O diretório do seu projeto deve ter a seguinte aparência:

SONARQUBE-DOCKERIZED-ON-SL2 <br />
├── compose <br />
│ └── docker-compose.yml <br />
├── README.md <br />
├── Install.ps1 <br />
└── Start.ps1 <br />

## Observações

- Certifique-se de ter espaço em disco suficiente disponível para baixar e configurar o Alpine Linux e o Docker.
- O script é testado com o PowerShell em ambientes Windows. A compatibilidade com outras plataformas não é garantida.

## Aviso Legal

Este script é fornecido como está, sem qualquer garantia. Use por sua própria conta e risco. Certifique-se de revisar o script e entender suas ações antes de executá-lo.


