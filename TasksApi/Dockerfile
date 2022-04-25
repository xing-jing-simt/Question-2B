FROM mcr.microsoft.com/dotnet/sdk:6.0.200 AS base
WORKDIR /app
EXPOSE 5000
ENV RABBITMQ_HOST localhost
ENV RABBITMQ_PORT 5672 
ENV ASPNETCORE_URLS=http://+:5000

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:6.0.200 AS build
WORKDIR /src
COPY ["TasksApi.csproj", "./"]
RUN dotnet restore "TasksApi.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "TasksApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TasksApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TasksApi.dll"]
