#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Akeem.Web.ToolBox/Akeem.Web.ToolBox.csproj", "Akeem.Web.ToolBox/"]
RUN dotnet restore "Akeem.Web.ToolBox/Akeem.Web.ToolBox.csproj"
COPY . .
WORKDIR "/src/Akeem.Web.ToolBox"
RUN dotnet build "Akeem.Web.ToolBox.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Akeem.Web.ToolBox.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
RUN apt-get update -y && apt-get install -y libgdiplus && apt-get clean && ln -s /usr/lib/libgdiplus.so /usr/lib/gdiplus.dll
ENTRYPOINT ["dotnet", "Akeem.Web.ToolBox.dll"]
