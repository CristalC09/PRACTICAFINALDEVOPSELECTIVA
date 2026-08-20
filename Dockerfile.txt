FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["HolaApp/HolaApp.csproj", "HolaApp/"]
RUN dotnet restore "HolaApp/HolaApp.csproj"
COPY . .
WORKDIR "/src/HolaApp"
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "HolaApp.dll"]