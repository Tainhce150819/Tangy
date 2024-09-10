#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["TangY_Gateway/TangY_Gateway.csproj", "TangY_Gateway/"]
COPY ["Tangy_Business/Tangy_Business.csproj", "Tangy_Business/"]
COPY ["Tangy_Common/Tangy_Common.csproj", "Tangy_Common/"]
COPY ["Tangy_DataAccess/Tangy_DataAccess.csproj", "Tangy_DataAccess/"]
COPY ["Tangy_Models/Tangy_Models.csproj", "Tangy_Models/"]
RUN dotnet restore "TangY_Gateway/TangY_Gateway.csproj"
COPY . .
WORKDIR "/src/TangY_Gateway"
RUN dotnet build "TangY_Gateway.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TangY_Gateway.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TangY_Gateway.dll"]