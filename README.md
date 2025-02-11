# Configuração e Uso do SonarQube com WSL2

Este guia explica como configurar e usar o SonarQube instalado no WSL2 para analisar projetos Java armazenados em uma máquina Windows.

## Índice

1. [Instalação do SonarQube usando Docker Compose](#instalação-do-sonarqube-usando-docker-compose)
2. [Acessando o SonarQube](#acessando-o-sonarqube)
3. [Configuração do SonarScanner no Windows](#configuração-do-sonarscanner-no-windows)
4. [Configuração do Projeto Java](#configuração-do-projeto-java)
5. [Executando a Análise](#executando-a-análise)
6. [Observações Importantes](#observações-importantes)

## Instalação do SonarQube usando Docker Compose

1. Certifique-se de que o Docker e o Docker Compose estão instalados no seu WSL2.
2. Navegue até o diretório `compose` do projeto.
3. Execute o seguinte comando para iniciar o SonarQube:

   ```bash
   docker-compose up -d
   ```


## Acessando o SonarQube

1. Abra um navegador e acesse `http://localhost:9000`.
2. Clique em "Log in" na página inicial.
3. Use as credenciais padrão:
   - Username: admin
   - Password: admin
4. Na primeira vez, você será solicitado a alterar a senha.
5. Após o login, você será direcionado ao dashboard do SonarQube.

## Configuração do SonarScanner no Windows

1. Baixe o SonarScanner para Windows do [site oficial do SonarQube](https://docs.sonarqube.org/latest/analyzing-source-code/scanners/sonarscanner/).
2. Extraia o arquivo zip para um diretório de sua escolha.
3. Adicione o diretório `bin` do SonarScanner ao PATH do sistema.

![Variável de Ambiente](https://raw.githubusercontent.com/fabioallima/sonarqube/refs/heads/main/images/system_variable_sonar.png)

## Obtenha um token de autenticação:
* Acesse a interface web do SonarQube (http://localhost:9000).
* Vá para User > My Account > Security.
* Gere um novo token e copie-o.

## Executando a Análise
* Abra um prompt de comando no diretório do seu projeto Java.
* Execute o comando: 
```bash
sonar-scanner.bat -Dsonar.projectKey=meu-projeto-java -Dsonar.projectName="Meu Projeto Java" -Dsonar.sources=src/main/java -Dsonar.java.binaries=target/classes -Dsonar.host.url=http://localhost:9000 -Dsonar.token=seu_token_de_autenticacao
```

