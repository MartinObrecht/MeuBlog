FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /App

EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /src

COPY MeuBlog.Api/*.csproj ./MeuBlog.Api/
COPY MeuBlog.Application/*.csproj ./MeuBlog.Application/
COPY MeuBlog.Domain/*.csproj ./MeuBlog.Domain/
COPY MeuBlog.Infra.Repository/*.csproj ./MeuBlog.Infra.Repository/
COPY MeuBlog.Infra.CrossCutting/*.csproj ./MeuBlog.Infra.CrossCutting/


RUN dotnet restore MeuBlog.Api/MeuBlog.Api.csproj
COPY . .
WORKDIR /src/MeuBlog.Api
RUN dotnet build MeuBlog.Api.csproj -c Release -o /App/build

FROM build AS publish
RUN dotnet publish MeuBlog.Api.csproj -c Release -o /App/publish

FROM base AS final
WORKDIR /App
COPY --from=publish /App/publish .
ENTRYPOINT ["dotnet", "MeuBlog.Api.dll"]