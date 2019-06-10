# prepare final image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS base
WORKDIR /app
EXPOSE 80

# prepare build image
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src

COPY Matrix.Agent.Directory/Matrix.Agent.Directory.csproj Matrix.Agent.Directory/
COPY Matrix.Agent.Directory.Business/Matrix.Agent.Directory.Business.csproj Matrix.Agent.Directory.Business/
COPY Matrix.Agent.Directory.Database/Matrix.Agent.Directory.Database.csproj Matrix.Agent.Directory.Database/
COPY Matrix.Agent.Directory.Model/Matrix.Agent.Directory.Model.csproj Matrix.Agent.Directory.Model/
COPY . .

# restore
RUN dotnet restore "Matrix.Agent.Directory/Matrix.Agent.Directory.csproj"

# build
WORKDIR /src/Matrix.Agent.Directory
RUN dotnet build "Matrix.Agent.Directory.csproj" -c Release -o /app

# publish
FROM build AS publish
RUN dotnet publish "Matrix.Agent.Directory.csproj" -c Release -o /app

# finalize
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Matrix.Agent.Directory.dll"]