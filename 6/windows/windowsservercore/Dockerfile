# escape=`
FROM sixeyed/openjdk:windowsservercore-ltsc2019
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
# Elasticsearch
ENV ES_VERSION="6.5.3"
ENV ES_HOME="c:\elasticsearch"

RUN Invoke-WebRequest -outfile elasticsearch.zip "https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$($env:ES_VERSION).zip" -UseBasicParsing;
RUN Expand-Archive elasticsearch.zip -DestinationPath C:\ ;
RUN Move-Item c:/elasticsearch-$($env:ES_VERSION) 'c:\elasticsearch';
RUN Remove-Item elasticsearch.zip

VOLUME c:/data

# REMARKS - DNS and mount tweaks needed for Windows
#RUN Set-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord; 
# Set-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices' -Name 'G:' -Value '\??\C:\data' -Type String 

EXPOSE 9200 9300
WORKDIR c:/elasticsearch
COPY config ./config
SHELL ["cmd", "/S", "/C"]
CMD ".\bin\elasticsearch.bat"