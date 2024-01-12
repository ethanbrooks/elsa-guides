FROM mcr.microsoft.com/dotnet/aspnet:8.0-bookworm-slim AS base
WORKDIR /app
EXPOSE 80/tcp
EXPOSE 443/tcp

FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
WORKDIR /src/elsa-guides/src/installation/elsa-server/ElsaServer
COPY ./src/installation/elsa-server/ElsaServer/ElsaServer.csproj .
RUN dotnet restore "ElsaServer.csproj"
COPY ./src/installation/elsa-server/ElsaServer .
RUN dotnet build "ElsaServer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ElsaServer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:80
ENTRYPOINT ["dotnet", "ElsaServer.dll"]
