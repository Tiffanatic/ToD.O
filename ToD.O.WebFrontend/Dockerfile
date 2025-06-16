FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ToD.O.WebFrontend/ToD.O.WebFrontend.csproj", "ToD.O.WebFrontend/"]
RUN dotnet restore "ToD.O.WebFrontend/ToD.O.WebFrontend.csproj"
COPY . .
WORKDIR "/src/ToD.O.WebFrontend"
RUN dotnet build "./ToD.O.WebFrontend.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ToD.O.WebFrontend.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ToD.O.WebFrontend.dll"]
