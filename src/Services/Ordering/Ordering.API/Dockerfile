#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/Services/Ordering/Ordering.API/Ordering.API.csproj", "src/Services/Ordering/Ordering.API/"]
COPY ["src/BuildingBlocks/EventBus.Message/EventBus.Message.csproj", "src/BuildingBlocks/EventBus.Message/"]
COPY ["src/Services/Ordering/Ordering.Application/Ordering.Application.csproj", "src/Services/Ordering/Ordering.Application/"]
COPY ["src/Services/Ordering/OrderingDomain/Ordering.Domain.csproj", "src/Services/Ordering/OrderingDomain/"]
COPY ["src/Services/Ordering/Ordering.Infastructure/Ordering.Infrastructure.csproj", "src/Services/Ordering/Ordering.Infastructure/"]
RUN dotnet restore "src/Services/Ordering/Ordering.API/Ordering.API.csproj"
COPY . .
WORKDIR "/src/src/Services/Ordering/Ordering.API"
RUN dotnet build "Ordering.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Ordering.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Ordering.API.dll"]