# #############################################################################
# Development docker image with support for Visual Studio debugging integration
#

# #############################################################################
# BASE IMAGE
FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80

# vscode debugging support
WORKDIR /vsdbg
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        unzip \
    && rm -rf /var/lib/apt/lists/* \
    && curl -sSL https://aka.ms/getvsdbgsh | bash /dev/stdin -v latest -l /vsdbg


# #############################################################################
# BUILDER IMAGE
FROM microsoft/dotnet:2.1-sdk AS builder
ENV NUGET_XMLDOC_MODE skip

# publish
COPY . /app
WORKDIR /app
RUN dotnet publish -f netcoreapp2.1 -c Debug -o /publish # -v quiet # -r linux-x64 


# #############################################################################
# PRODUCTION IMAGE
FROM base AS production
WORKDIR /app
COPY --from=builder /publish .

# Kick off a container just to wait debugger to attach and run the app
ENTRYPOINT ["/bin/bash", "-c", "if [ \"$REMOTE_DEBUGGING\" = \"enabled\" ]; then sleep infinity; else dotnet HelloWorld.dll; fi"]

# #############################################################################
