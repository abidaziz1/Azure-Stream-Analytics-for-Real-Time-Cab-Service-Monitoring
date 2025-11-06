FROM mcr.microsoft.com/dotnet/core/sdk:2.1 AS build

# Use the archived repositories for Debian Stretch and remove stretch-updates
RUN sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list && \
    sed -i 's|http://security.debian.org/debian-security|http://archive.debian.org/debian-security|g' /etc/apt/sources.list && \
    sed -i '/stretch-updates/d' /etc/apt/sources.list && \
    apt-get update

RUN apt-get install -y git
RUN git clone --recursive https://github.com/mspnp/azure-stream-analytics-data-pipeline.git && \
    cd azure-stream-analytics-data-pipeline && git fetch && git checkout master

WORKDIR azure-stream-analytics-data-pipeline/onprem/DataLoader
RUN dotnet build -c Release
RUN dotnet publish -f netcoreapp2.0 -c Release

FROM mcr.microsoft.com/dotnet/core/runtime:2.1 AS runtime
WORKDIR DataLoader
COPY --from=build azure-stream-analytics-data-pipeline/onprem/DataLoader/bin/Release/netcoreapp2.0/publish .
ENTRYPOINT ["dotnet", "taxi.dll"]

    
