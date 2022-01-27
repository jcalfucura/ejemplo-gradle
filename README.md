# Getting Started

## Windows

### Compile Code
* ./mvnw.cmd clean compile -e

### Test Code
* ./mvnw.cmd clean test -e

### Jar Code
* ./mvnw.cmd clean package -e

### Run Jar
* Local:      ./mvnw.cmd spring-boot:run 
* Background: nohup bash mvnw.cmd spring-boot:run &

### Testing Application
* Abrir navegador: http://localhost:8081/rest/mscovid/test?msg=testing

## Linux

### Compile Code
* ./mvnw clean compile -e

### Test Code
* ./mvnw clean test -e

### Jar Code
* ./mvnw clean package -e

### Run Jar
* Local:      ./mvnw spring-boot:run 
* Background: nohup bash mvnw spring-boot:run &

### Testing Application
* curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'

# Using Docker to test this app.
⚠️ **Is mandatory to use Powershell in Windows**
## Docker in Windows
```bash
### Compile Code
docker run -it --rm -v ${pwd}:/code --workdir /code maven mvn clean compile -e

### Test Code
docker run -it --rm -v ${pwd}:/code --workdir /code maven mvn clean test -e

### Jar Code
docker run -it --rm -v ${pwd}:/code --workdir /code maven mvn clean package -e

### Run Jar
docker run -it --rm -p 8081:8081  -v ${pwd}:/code --workdir /code maven mvn spring-boot:run
```
## Docker in Linux
```bash
### Compile Code
docker run -it --rm -v $(pwd):/code --workdir /code maven mvn clean compile -e

### Test Code
docker run -it --rm -v $(pwd):/code --workdir /code maven mvn clean test -e

### Jar Code
docker run -it --rm -v $(pwd):/code --workdir /code maven mvn clean package -e
#12
### Run Jar
docker run -it --rm -p 8081:8081  -v $(pwd):/code --workdir /code maven mvn spring-boot:run
```
./gradlew sonarqube \
  -Dsonar.projectKey=test \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=6bc97caab961552c10be52a5218ae03f6e3e6560