FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/TaskQueue.Client/TaskQueue.Client.API/TaskQueue.Client.API.csproj", "src/TaskQueue.Client/TaskQueue.Client.API/"]
COPY ["src/TaskQueue.Client/TaskQueue.Client.Infrastructure/TaskQueue.Client.Infrastructure.csproj", "src/TaskQueue.Client/TaskQueue.Client.Infrastructure/"]
COPY ["src/TaskQueue.Client/TaskQueue.Client.Application/TaskQueue.Client.Application.csproj", "src/TaskQueue.Client/TaskQueue.Client.Application/"]
COPY ["src/TaskQueue.Client/TaskQueue.Client.Domain/TaskQueue.Client.Domain.csproj", "src/TaskQueue.Client/TaskQueue.Client.Domain/"]
COPY ["src/SharedKernel/SharedKernel.Abstractions/SharedKernel.Abstractions.csproj", "src/SharedKernel/SharedKernel.Abstractions/"]
RUN dotnet restore "src/TaskQueue.Client/TaskQueue.Client.API/TaskQueue.Client.API.csproj"
COPY . .
WORKDIR "/src/src/TaskQueue.Client/TaskQueue.Client.API"
RUN dotnet build "TaskQueue.Client.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "TaskQueue.Client.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TaskQueue.Client.API.dll"]