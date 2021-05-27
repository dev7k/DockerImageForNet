FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine AS publish
WORKDIR /src
COPY ContainerSample.csproj ./

RUN dotnet restore "./ContainerSample.csproj" --runtime alpine-x64
COPY . .
RUN dotnet publish "ContainerSample.csproj" -c Release -o /app/publish \
  --no-restore \
  --runtime alpine-x64 \
  --self-contained true \
  /p:PublishTrimmed=true \
  /p:PublishSingleFile=true

# use different image
FROM mcr.microsoft.com/dotnet/runtime-deps:5.0-alpine AS final
WORKDIR /app

# just expose port 80
EXPOSE 80
COPY --from=publish /app/publish .

ENTRYPOINT ["./ContainerSample"]