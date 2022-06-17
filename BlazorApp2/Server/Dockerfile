#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["BlazorApp2/Server/BlazorApp2.Server.csproj", "BlazorApp2/Server/"]
COPY ["BlazorApp2/Shared/BlazorApp2.Shared.csproj", "BlazorApp2/Shared/"]
COPY ["BlazorApp2/Client/BlazorApp2.Client.csproj", "BlazorApp2/Client/"]
RUN dotnet restore "BlazorApp2/Server/BlazorApp2.Server.csproj"
COPY . .
WORKDIR "/src/BlazorApp2/Server"
RUN dotnet build "BlazorApp2.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorApp2.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorApp2.Server.dll"]