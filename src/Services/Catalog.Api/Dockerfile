FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 7136

ENV ASPNETCORE_URLS=http://+:7136

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Services/Catalog.Api/Catalog.Api.csproj", "src/Services/Catalog.Api/"]
RUN dotnet restore "src\Services\Catalog.Api\Catalog.Api.csproj"
COPY . .
WORKDIR "/src/src/Services/Catalog.Api"
RUN dotnet build "Catalog.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Catalog.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Catalog.Api.dll"]
