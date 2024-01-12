FROM mcr.microsoft.com/dotnet/aspnet:8.0-bookworm-slim AS base
WORKDIR /app
EXPOSE 80/tcp
EXPOSE 443/tcp

FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
WORKDIR /src/elsa-guides/src/guides/external-app-integration/EmployeeOnboarding.Web/
COPY ./src/guides/external-app-integration/EmployeeOnboarding.Web/EmployeeOnboarding.Web.csproj .

RUN dotnet restore "EmployeeOnboarding.Web.csproj"
COPY ./src/guides/external-app-integration/EmployeeOnboarding.Web/ .
RUN dotnet build "EmployeeOnboarding.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "EmployeeOnboarding.Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:80
ENTRYPOINT ["dotnet", "EmployeeOnboarding.Web.dll"]
