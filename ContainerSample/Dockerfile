FROM mcr.microsoft.com/dotnet/aspnet:5.0-alpine AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# remove the build stage
FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine AS publish
WORKDIR /src
COPY ContainerSample.csproj ./
# specify target runtime for destroy
RUN dotnet restore "./ContainerSample.csproj" --runtime alpine-x64
COPY . .

# add --no-restore flag to dotnet publish
RUN dotnet publish "ContainerSample.csproj" -c Release -o /app/publish \
  --no-restore \  
  --runtime alpine-x64 \
  --self-contained true \
  /p:PublishTrimmed=true \
  /p:PublishSingleFile=true

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["./ContainerSample"]