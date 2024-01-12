FROM mcr.microsoft.com/dotnet/aspnet:8.0-bookworm-slim AS base
WORKDIR /app
EXPOSE 80/tcp
EXPOSE 443/tcp

FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
WORKDIR /src/elsa-guides/src/guides/workflow-client-app/WorkflowClientApp
COPY ./src/guides/workflow-client-app/WorkflowClientApp/WorkflowClientApp.csproj .
RUN dotnet restore "WorkflowClientApp.csproj"
COPY ./src/guides/workflow-client-app/WorkflowClientApp/ .
RUN dotnet build "WorkflowClientApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WorkflowClientApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:80
ENTRYPOINT ["dotnet", "WorkflowClientApp.dll"]
