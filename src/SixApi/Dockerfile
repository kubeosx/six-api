#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["SixApi.csproj", "SixApi/"]
RUN dotnet restore "SixApi/SixApi.csproj"
WORKDIR "/src/SixApi"
COPY . .
RUN dotnet build "SixApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SixApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app

#required for readonlyfilesystem in ks
ENV COMPlus_EnableDiagnostics=0

COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS http://+:8080
ENTRYPOINT ["dotnet", "SixApi.dll"]
