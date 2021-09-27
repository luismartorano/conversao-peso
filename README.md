# Iniciativa Kubernetes - Aula 1 - DOCKER - Desafio: DOT NET

### Dockerfile criado
```
FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["ConversaoPeso.Web.csproj", ""]
RUN dotnet restore "./ConversaoPeso.Web.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "ConversaoPeso.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ConversaoPeso.Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ConversaoPeso.Web.dll"]
```
### Imagem personalizada
```
docker image build . -t luismartorano/conversaopeso:1.0 
```

### Executando o Container na porta 80 ou qualquer outra
```
docker container run -p 80:80 -d luismartorano/conversaopeso:1.0
```

### Verificando o processo
```
docker container ps
```
### Executando o programa
```
localhost:80
```
### Destruindo o container
```
docker container rm -f <nome do container>
```
### Referências

https://docs.microsoft.com/pt-br/dotnet/core/docker/build-container?tabs=linux

https://github.com/dotnet/dotnet-docker/issues/1939

https://codefresh.io/docker-tutorial/docker-images-net-core/


