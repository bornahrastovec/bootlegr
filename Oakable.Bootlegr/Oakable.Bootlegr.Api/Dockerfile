FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Oakable.Bootlegr.Api/Oakable.Bootlegr.Api.csproj", "Oakable.Bootlegr.Api/"]
RUN dotnet restore "Oakable.Bootlegr.Api/Oakable.Bootlegr.Api.csproj"
COPY . .
WORKDIR "/src/Oakable.Bootlegr.Api"
RUN dotnet build "Oakable.Bootlegr.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Oakable.Bootlegr.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Oakable.Bootlegr.Api.dll"]
