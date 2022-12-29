# https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu?source=recommendations
# https://learn.microsoft.com/en-us/aspnet/core/tutorials/min-web-api?view=aspnetcore-7.0&tabs=visual-studio-code
# https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/building-net-docker-images?view=aspnetcore-7.0

# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /source

# copy csproj and restore as distinct layers
COPY *.sln .
COPY publicapis/*.csproj ./publicapis/
RUN dotnet restore publicapis/publicapis.csproj --use-current-runtime

# copy everything else and build app
COPY publicapis/. ./publicapis/
WORKDIR /source/publicapis
RUN dotnet publish -c release -o /app --use-current-runtime --self-contained false --no-restore

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "publicapis.dll"]