FROM mcr.microsoft.com/dotnet/core/runtime:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/KubePipeline.Agent/KubePipeline.Agent.csproj", "src/KubePipeline.Agent/"]
RUN dotnet restore "src/KubePipeline.Agent/KubePipeline.Agent.csproj"
COPY . .
WORKDIR "/src/src/KubePipeline.Agent"
RUN dotnet build "KubePipeline.Agent.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "KubePipeline.Agent.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "KubePipeline.Agent.dll"]