FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR /src

COPY ca.daviviendacorredores.goskope.com.crt ca.daviviendacorredores.goskope.com.crt
ADD ca.daviviendacorredores.goskope.com.crt /usr/local/share/ca-certificates/ca.daviviendacorredores.goskope.com.crt
ADD ca.daviviendacorredores.goskope.com.crt /etc/ssl/certs/ca.daviviendacorredores.goskope.com.crt
RUN chmod 644 /usr/local/share/ca-certificates/ca.daviviendacorredores.goskope.com.crt
RUN update-ca-certificates

COPY MultiStage.csproj .
RUN curl https://api.nuget.org/v3/index.json
RUN dotnet restore 
COPY . .


RUN dotnet build "MultiStage.csproj" -c Release -o /app/build

RUN dotnet publish -c Release -o /app

FROM mcr.microsoft.com/dotnet/sdk:6.0
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "MultiStage.dll"]