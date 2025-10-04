# --- STAGE 1: BUILD (Compilação) ---
# Usa uma imagem com JDK e Maven para compilar seu projeto Java
FROM maven:3.9.5-eclipse-temurin-17 AS build

# Define o diretório de trabalho dentro do container
WORKDIR /app

# Copia os arquivos de build para otimizar o cache
COPY pom.xml .

# Baixa as dependências do Maven
RUN mvn dependency:go-offline

# Copia o código-fonte
COPY src ./src

# Compila o projeto, gerando o arquivo .jar (certifique-se de que não tem erros!)
RUN mvn package -DskipTests

# --- STAGE 2: RUNTIME (Execução) ---
# Usa uma imagem JRE menor e mais segura (Java Runtime Environment) para rodar o app
FROM eclipse-temurin:17-jre-alpine

# Define o diretório de trabalho para execução
WORKDIR /app

# Copia o arquivo .jar compilado do stage 'build'
# *ATENÇÃO: Este comando assume que o nome do seu .jar está correto no caminho target/*.jar. 
# Se você souber o nome exato (ex: app-0.0.1-SNAPSHOT.jar), ajuste o caminho.*
COPY --from=build /app/target/*.jar app.jar

# Define a porta que sua aplicação Java estará ouvindo (padrão Spring Boot é 8080)
EXPOSE 8080

# Comando para iniciar sua aplicação Java
ENTRYPOINT ["java", "-jar", "app.jar"]