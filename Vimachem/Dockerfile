FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 5000
EXPOSE 5001

# Set environment variables
ENV ASPNETCORE_ENVIRONMENT="Development"
ENV ASPNETCORE_URLS="http://*:5000;https://*:5001"
ENV ASPNETCORE_HTTPS_PORTS=5001

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["Vimachem.csproj", "."]
RUN dotnet restore "./Vimachem.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "Vimachem.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Vimachem.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Vimachem.dll"]
