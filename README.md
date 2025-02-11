# Configuração e Uso do SonarQube com WSL2

Este guia explica como configurar e usar o SonarQube instalado no WSL2 para analisar projetos Java armazenados em uma máquina Windows.

## Índice

1. [Instalação do SonarQube usando Docker Compose](#instalação-do-sonarqube-usando-docker-compose)
2. [Acessando o SonarQube](#acessando-o-sonarqube)
3. [Configuração do SonarScanner no Windows](#configuração-do-sonarscanner-no-windows)
4. [Configuração do Projeto Java](#configuração-do-projeto-java)
5. [Obtenha um token de autenticação](#obtenha-um-token-de-autenticação)
6. [Configuração do projeto](#configuração-do-projeto)
7. [Executando a Análise](#executando-a-análise)

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

## Configuração do projeto

```XML
  <properties>
        <java.version>21</java.version>
        <sonar.projectKey>projeto-java</sonar.projectKey>
		<sonar.projectName>Projeto Java</sonar.projectName>
		<sonar.java.coveragePlugin>jacoco</sonar.java.coveragePlugin>
		<sonar.dynamicAnalysis>reuseReports</sonar.dynamicAnalysis>
		<sonar.jacoco.reportPath>${project.basedir}/../target/jacoco.exec</sonar.jacoco.reportPath>
		<sonar.language>java</sonar.language>
		<sonar.coverage.exclusions>
			**/Application.java,
			**/config/**,
			**/entities/**,
			**/dto/**,
			**/mappers/**,
			**/controllers/handlers/**,
			**/controllers/exceptions/**,
			**/services/exceptions/**,
			**/services/validation/**
		</sonar.coverage.exclusions>
    </properties>


      <dependency>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>${jacoco.version}</version>
      </dependency>

<build>
    <plugins>
....
        <plugin>
 				<groupId>org.jacoco</groupId>
				<artifactId>jacoco-maven-plugin</artifactId>
				<version>0.8.10</version>
				<configuration>
					<excludes>
						<exclude>com/example/dslist/DslistApplication.class</exclude>
						<exclude>com/example/dslist/config/**</exclude>
						<exclude>com/example/dslist/entities/**</exclude>
						<exclude>com/example/dslist/dto/**</exclude>
						<exclude>com/example/dslist/mappers/**</exclude>
						<exclude>com/example/dslist/controllers/handlers/**</exclude>
						<exclude>com/example/dslist/controllers/exceptions/**</exclude>
						<exclude>com/example/dslist/services/exceptions/**</exclude>
						<exclude>com/example/dslist/services/validation/**</exclude>
					</excludes>
				</configuration>

				<executions>

					<execution>
						<id>jacoco-initialize</id>
						<goals>
							<goal>prepare-agent</goal>
						</goals>
					</execution>

					<execution>
						<id>jacoco-report</id>
						<phase>prepare-package</phase>
						<goals>
							<goal>report</goal>
						</goals>
						<configuration>
							<outputDirectory>target/jacoco-report</outputDirectory>
						</configuration>
					</execution>

					<execution>
						<id>jacoco-check</id>
						<goals>
							<goal>check</goal>
						</goals>
						<configuration>
							<rules>
								<rule>
									<element>PACKAGE</element>
									<limits>
										<limit>
											<counter>LINE</counter>
											<value>COVEREDRATIO</value>
											<minimum>0.20</minimum>
										</limit>
									</limits>
								</rule>
							</rules>
						</configuration>
					</execution>
				</executions>
		</plugin> 
    <plugins>   
</build>
```

## Executando a Análise
* Abra um prompt de comando no diretório do seu projeto Java.
* Execute o comando: 
```bash
mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=token_user

sonar-scanner.bat -Dsonar.projectKey=meu-projeto-java -Dsonar.projectName="Meu Projeto Java" -Dsonar.sources=src/main/java -Dsonar.java.binaries=target/classes -Dsonar.host.url=http://localhost:9000 -Dsonar.token=seu_token_de_autenticacao

mvn clean verify org.jacoco:jacoco-maven-plugin:report sonar:sonar -Dsonar.projectKey=Projeto   -Dsonar.projectName='Projeto Java'   -Dsonar.host.url=http://localhost:9000   -Dsonar.token=seu_token_de_autenticacaoc46bb -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
```

