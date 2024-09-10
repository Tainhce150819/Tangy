#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["TangyWeb_API/TangyWeb_API.csproj", "TangyWeb_API/"]
COPY ["Tangy_Business/Tangy_Business.csproj", "Tangy_Business/"]
COPY ["Tangy_Common/Tangy_Common.csproj", "Tangy_Common/"]
COPY ["Tangy_DataAccess/Tangy_DataAccess.csproj", "Tangy_DataAccess/"]
COPY ["Tangy_Models/Tangy_Models.csproj", "Tangy_Models/"]
RUN dotnet restore "TangyWeb_API/TangyWeb_API.csproj"
COPY . .
WORKDIR "/src/TangyWeb_API"
RUN dotnet build "TangyWeb_API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TangyWeb_API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TangyWeb_API.dll"]