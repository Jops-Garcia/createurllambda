# Microserviço: Criação de URL Encurtada

Este microserviço permite gerar URLs encurtadas, salvando os metadados no AWS S3. Ele é implementado usando AWS Lambda, com um endpoint que aceita uma URL original e uma data de expiração, retornando um código encurtado que pode ser usado posteriormente para redirecionamento.

---

## Funcionalidade

### Fluxo de Operação:
1. **Recebe uma solicitação** via AWS API Gateway contendo:
   - **URL original**.
   - **Tempo de expiração** em segundos (timestamp).
2. **Gera um código encurtado** aleatório.
3. **Armazena os dados** (URL original e expiração) em um bucket S3.
4. **Retorna o código** gerado para acesso à URL encurtada.

---

## Estrutura do Código

### Principais Classes:
- **`Main.java`**:
  - Handler principal da função Lambda.
  - Processa a solicitação, valida e armazena as informações.
- **`UrlData.java`**:
  - Classe modelo contendo a URL original e o tempo de expiração.

### Estrutura de Arquivos:
```plaintext
src/
└── main/
    └── java/
        └── com/
            └── garciajops/
                └── createurlshortener/
                    ├── Main.java
                    └── UrlData.java
pom.xml
```

---

## Configuração e Deploy

### Pré-requisitos:
1. **AWS CLI** configurado.
2. **Bucket S3** criado (ex: `garciajops-url-shortener-storage`).
3. **IAM Role** com permissões de leitura/escrita no S3 e execução de funções Lambda.

### Etapas de Deploy:
1. **Construção do Projeto**:
   ```bash
   mvn clean package
   ```
2. **Upload da Função Lambda**:
   - Faça o upload do arquivo `.jar` gerado para a AWS Lambda.
   - Defina o **handler** como:
     ```
     com.garciajops.createurlshortener.Main::handleRequest
     ```

3. **Configuração do API Gateway**:
   - Crie um endpoint **POST** associado à função Lambda.

---

## Exemplo de Requisição

### Criar uma URL Encurtada:
- **Método**: POST
- **Endpoint**: `/create`
- **Corpo da Requisição**:
  ```json
  {
    "originalUrl": "https://www.exemplo.com",
    "expirationTime": "1700000000"
  }
  ```

### Resposta:
```json
{
  "code": "abc12345"
}
```

---

## Dependências Maven

O projeto utiliza as seguintes dependências:

```xml
<dependencies>
    <dependency>
        <groupId>com.amazonaws</groupId>
        <artifactId>aws-lambda-java-core</artifactId>
        <version>1.2.1</version>
    </dependency>
    <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>s3</artifactId>
        <version>2.17.106</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.34</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.3</version>
    </dependency>
</dependencies>
```

---
