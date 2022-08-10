FROM mcr.microsoft.com/dotnet/aspnet:6.0-focal AS base
WORKDIR /app
EXPOSE 5070
EXPOSE 7278

ENV ASPNETCORE_URLS=http://+:5070

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:6.0-focal AS build
WORKDIR /src
COPY ["db-svc.csproj", "service/db-svc/"]
RUN dotnet restore "service/db-svc/db-svc.csproj"
WORKDIR "/src/service/db-svc"
COPY . .
RUN dotnet build "db-svc.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "db-svc.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "db-svc.dll"]
