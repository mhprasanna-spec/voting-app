# Global Scope: Declare arguments BEFORE the FROM instruction
ARG BUILDPLATFORM=linux/amd64
ARG TARGETARCH=amd64
ARG TARGETPLATFORM=linux/amd64

# ---- Build Stage ----
FROM --platform=${BUILDPLATFORM} mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Build Scope: Re-declare arguments so they can be used in RUN commands
ARG TARGETPLATFORM
ARG TARGETARCH
ARG BUILDPLATFORM
RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM"

WORKDIR /source
COPY *.csproj .
RUN dotnet restore -a $TARGETARCH

COPY . .
RUN dotnet publish -c release -o /app -a $TARGETARCH --self-contained false --no-restore

# ---- Production App Stage ----
FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "Worker.dll"]

