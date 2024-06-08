FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/TaskQueue.Worker/TaskQueue.Worker.Presentation/TaskQueue.Worker.Presentation.csproj", "src/TaskQueue.Worker/TaskQueue.Worker.Presentation/"]
RUN dotnet restore "src/TaskQueue.Worker/TaskQueue.Worker.Presentation/TaskQueue.Worker.Presentation.csproj"
COPY . .
WORKDIR "/src/src/TaskQueue.Worker/TaskQueue.Worker.Presentation"
RUN dotnet build "TaskQueue.Worker.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "TaskQueue.Worker.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TaskQueue.Worker.Presentation.dll"]
