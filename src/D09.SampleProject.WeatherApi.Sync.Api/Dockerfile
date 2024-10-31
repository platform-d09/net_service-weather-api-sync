FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["D09.SampleProject.WeatherApi.Sync.Api/D09.SampleProject.WeatherApi.Sync.Api.csproj", "D09.SampleProject.WeatherApi.Sync.Api/"]
RUN dotnet restore "D09.SampleProject.WeatherApi.Sync.Api/D09.SampleProject.WeatherApi.Sync.Api.csproj"
COPY . .
WORKDIR "/src/D09.SampleProject.WeatherApi.Sync.Api"
RUN dotnet build "D09.SampleProject.WeatherApi.Sync.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "D09.SampleProject.WeatherApi.Sync.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "D09.SampleProject.WeatherApi.Sync.Api.dll"]
