# Propriedades da aplicação Spring Boot
spring:
  main:
    banner-mode: 'console'
  application:
    name: yz-consortium-parameterizer-connection
  h2:
    console:
      enabled: true
      path: /h2-console


database:
  h2:
    username: ${DATABASE_USER:src/main/resources/database/secret-db-username}
    password: ${DATABASE_PASSWORD:src/main/resources/database/secret-db-password}
    driver-class-name: ${DRIVER_CLASS_NAME:org.h2.Driver}
    url: jdbc:h2:mem:testdb
    pool-name: ${POOL_NAME:ArsenalPool}
    min-pool-size: ${MIN_POOL_SIZE:1}
    max-pool-size: ${MAX_POOL_SIZE:2}
    max-lifetime: ${MAX_LIFETIME:1800000}
    validation-timeout: ${VALIDATION_TIMEOUT:250}
    idle-timeout: ${IDLE_TIMEOUT:36000}
    connection-timeout: ${CONNECTION_TIMEOUT:20000}
    leak-detection-threshold: ${LEAK_DETECTION_THRESHOLD:3000}
  oracle:
    username: ${DATABASE_USER:src/main/resources/database/secret-db-username}
    password: ${DATABASE_PASSWORD:src/main/resources/database/secret-db-password}
    driver-class-name: ${DRIVER_CLASS_NAME:oracle.jdbc.driver.OracleDriver}
    url: jdbc:oracle:thin:@//localhost:1521/xe
    pool-name: ${POOL_NAME:ArsenalPool}
    min-pool-size: ${MIN_POOL_SIZE:1}
    max-pool-size: ${MAX_POOL_SIZE:2}
    max-lifetime: ${MAX_LIFETIME:1800000}
    validation-timeout: ${VALIDATION_TIMEOUT:250}
    idle-timeout: ${IDLE_TIMEOUT:36000}
    connection-timeout: ${CONNECTION_TIMEOUT:20000}
    leak-detection-threshold: ${LEAK_DETECTION_THRESHOLD:3000}
      
# Dados exibidos no endpoint "info" do Actuator
info:
  app:
    groupId: "@project.groupId@"
    artifactId: "@project.artifactId@"
    version: "@project.version@"
    java.version: "@java.version@"

# Porta do servidor
server:
  port: 8080
  #servlet:
    #context-path: /yz-consortium-parameterizer-connection 

# Configurações da biblioteca core do Arsenal
arsenal:
  library:
    core:
      api:
        enable-docs: true # Desativar documentação Swagger/OpenAPI
        docs-base-package: br.com.santander.yz.controller # Pacote onde se encontram as classes de controller
        #archetypekey: 8A436905830564F37B788A9CD0747822C4CFE8C2FB10418E3497C68E2E5705D5
      auth:
        enabled: false # Ativar segurança (autenticação e autorização)
        #public-key: Chave pública utilizada para validar os tokens JWT
        #jwk-set-uri: Endereço de um authorization server (ex: RH SSO), a chave pública é obtida automaticamente
        #algorithm: Algoritmo de criptografia utilizado para assinar os tokens
        #actuator-role: Role necessária para acesso aos endpoints do actuator (por padrão permite sempre o acesso)
        #custom-header: Header onde o token JWT será recebido (padrão: Authorization)
        #whitelist: Lista de endpoints que não requerem segurança
        #  - /api/v1/endpoint1/
        #  - /api/v1/endpoint2/**
        #role-authorization: Lista de endpoints e roles necessárias para acessá-los
        #  "[/api/v1/endpoint3/**]"
        #    - ROLE_ONE
        #    - ROLE_TWO
      log:
        enable-json-log: false #Setar para true caso queira usar a funcionalidade do log-back
        enable-json-message-log: false #Setar para true caso queira a resposta da mensagem text/json
      

# Configurações de banco de dados
spring.jpa:
  # database: ORACLE
  generate-ddl: false
  show-sql: false
  hibernate:
    ddl-auto: update
  properties:
#    javax:
#      persistence:
#        schema-generation:
#          scripts:
#            action: create
#            create-target: create.sql
#            create-source: metadata
    hibernate:
      dialect: ${HIBERNATE_DIALECT:org.hibernate.dialect.Oracle10gDialect}
#      default_schema: SYSTEM

spring.datasource:
  continue-on-error: false
  # A conexão com o banco pode ser feita automaticamente se as propriedades de usuário/senha
  # estiverem configuradas diretamente nas variáveis de ambiente declaradas abaixo.
  #
  # No entanto, recomendamos que você guarde essas credenciais em Secrets do OpenShift.
  #
  # Ao usar Secrets, as variáveis de ambiente passam a contar o caminho para os arquivos de secret.
  #
  # O Spring Boot não "sabe" abrir os arquivos de Secret e ler o conteúdo dos mesmos, por esse motivo
  # usamos um DataSource customizado (veja a classe DatabaseConfig) que faz esse processo.
  #
  # Veja as configurações de credenciais mais acima na seção sample.backing-services.database.
  platform: oracle
  #driver-class-name: oracle.jdbc.driver.OracleDriver
# Logging
# Sobrescrevendo grupos e níveis de log
logging:
  group:
    web: org.springframework.core.codec, org.springframework.http, org.springframework.web
    spring: org.springframework.core.env
    servlet: org.springframework.boot.web, org.apache.catalina, org.apache.coyote, org.apache.tomcat
    data: org.springframework.jdbc.core, org.hibernate.sql, org.springframework.orm.jpa, org.hibernate, org.jooq.tools.LoggerListener
    hikari: com.zaxxer.hikari
    app: br.com.santander.yz
  level:
    root: INFO
    spring: INFO
    app: INFO
    servlet: INFO
    web: INFO
    data: INFO
    hikari: ${LOG_LEVEL_HIKARI:INFO}

# Configurações do Resilience4j
resilience4j:
  ratelimiter:
    instances:
      StimulusController:
        limitForPeriod: 100
        limitRefreshPeriod: 1000ms
        timeoutDuration: 2000ms
  timelimiter:
    instances:
      StimulusController:
        timeoutDuration: 2s

