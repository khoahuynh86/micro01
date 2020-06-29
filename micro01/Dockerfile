#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base

WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["micro01/micro01.csproj", "micro01/"]
COPY ["micro01.data/micro01.data.csproj", "micro01.data/"]

RUN dotnet restore "micro01/micro01.csproj"
COPY . .
WORKDIR "/src/micro01"
RUN dotnet build "micro01.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "micro01.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "micro01.dll"]