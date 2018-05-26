# escape=`
FROM openjdk:8-jdk-nanoserver
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Elasticsearch
ENV ES_VERSION="6.2.2" `
    ES_HOME="c:\elasticsearch" `
    ES_SHA1="AAEF5091BD177371215FFC88C5B9B037FBCD882D" `    
    ES_PLUGIN_SHA1="33A9837C0AD8561D4C602447283E0F5E21F25F67"

RUN Invoke-WebRequest -outfile elasticsearch.zip "https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$($env:ES_VERSION).zip" -UseBasicParsing; `
    if ((Get-FileHash elasticsearch.zip -Algorithm sha1).Hash -ne $env:ES_SHA1) {exit 1} ; `
    Expand-Archive elasticsearch.zip -DestinationPath C:\ ; `
    Move-Item c:/elasticsearch-$($env:ES_VERSION) 'c:\elasticsearch'; `
    Remove-Item elasticsearch.zip

RUN Invoke-WebRequest -outfile elasticsearchplugin.zip "https://artifacts.elastic.co/downloads/packs/x-pack/x-pack-$($env:ES_VERSION).zip" -UseBasicParsing; `
    if ((Get-FileHash elasticsearchplugin.zip -Algorithm sha1).Hash -ne $env:ES_PLUGIN_SHA1) {exit 1} ; `
    Expand-Archive elasticsearchplugin.zip -DestinationPath C:\ ; `
    Move-Item c:/elasticsearchplugin-$($env:ES_VERSION) 'c:\elasticsearchplugin'; `
    Remove-Item elasticsearchplugin.zip

VOLUME c:/data

# REMARKS - DNS and mount tweaks needed for Windows
RUN Set-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord; `
    Set-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices' -Name 'G:' -Value '\??\C:\data' -Type String    

EXPOSE 9200 9300
WORKDIR c:/elasticsearch
COPY config ./config

SHELL ["cmd", "/S", "/C"]
CMD ".\bin\elasticsearch.bat"