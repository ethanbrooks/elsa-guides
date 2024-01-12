FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build

WORKDIR /src/elsa-guides/src/installation/elsa-studio/ElsaStudioBlazorWasm
COPY ./src/installation/elsa-studio/ElsaStudioBlazorWasm/ElsaStudioBlazorWasm.csproj .
RUN dotnet restore "ElsaStudioBlazorWasm.csproj"
COPY ./src/installation/elsa-studio/ElsaStudioBlazorWasm/src .
RUN dotnet build "ElsaStudioBlazorWasm.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ElsaStudioBlazorWasm.csproj" -c Release -o /app/publish

FROM nginx:bookworm AS final
EXPOSE 80/tcp
EXPOSE 443/tcp
COPY --from=publish /app/publish/wwwroot /usr/share/nginx/html
COPY ./src/installation/elsa-studio/ElsaStudioBlazorWasm/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY ./src/installation/elsa-studio/ElsaStudioBlazorWasm/appsettings.json /usr/share/nginx/html/appsettings.json