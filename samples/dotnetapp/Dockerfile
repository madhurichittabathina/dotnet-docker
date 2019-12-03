FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
#COPY /home/runner/work/dotnet-docker/dotnet-docker/samples/dotnetapp/dotnetapp/dotnetapp.csproj ./dotnetapp/
COPY  . ./dotnetapp/
#COPY  es/dotnetapp/dotnetapp.csproj ./dotnetapp/
#COPY utils/*.csproj ./utils/
WORKDIR /app/dotnetapp/samples/dotnetapp
#RUN ls
RUN dotnet restore

# copy and publish app and libraries
#WORKDIR /app/dotnetapp/samples/dotnetapp/
#COPY dotnetapp/. ./dotnetapp/
#COPY utils/. ./utils/
WORKDIR /app/dotnetapp/samples/dotnetapp
#RUN ls
RUN dotnet publish -c Release -o out


# test application -- see: dotnet-docker-unit-testing.md
FROM build AS testrunner
WORKDIR /app/dotnetapp/samples/dotnetapp/tests
RUN ls
#COPY tests/. .
ENTRYPOINT ["dotnet", "test", "--logger:trx"]


FROM mcr.microsoft.com/dotnet/core/runtime:3.0 AS runtime
WORKDIR /app/dotnetapp/samples/dotnetapp
RUN ls
COPY --from=build /app/dotnetapp/samples/dotnetapp/out ./
ENTRYPOINT ["dotnet", "dotnetapp.dll"]
