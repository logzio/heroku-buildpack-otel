# UNDER WIP - dont use

# heroku-buildpack-otel

Based on [Skrud work](https://github.com/skrud/heroku-buildpack-telegraf)

A simple heroku buildpack to download, deploy and launch Open Telemetry on your dynos.

This buildpack downloads the latest Otel release (at the time of writing, 1.19.0), extracts it on your dyno and starts it via a .profile.d script.

You can use this buildpack for other observability platform (which support otel agent), just change the exporter in the config.

## Installation
From your heroku git directory download [this config.yaml](https://raw.githubusercontent.com/logzio/heroku-buildpack-otel/master/config.yaml).

    wget -O otel_conf.yaml https://raw.githubusercontent.com/logzio/heroku-buildpack-otel/master/config.yaml

## Enable enviroment variable

Repelace the following with the relevant paramters:

| Variable | Value |
|---|---|
| <<HEROKU_APP_NAME>> | your heroku app name ( for example obscure-earth-56999 ) |
| <<LOGZIO_ACCOUNT_REGION_CODE>> | your Logzio region ( the deualt value is us )|
| <<LOGZIO_TRACING_TOKEN>> | your Logzio metrics token |
    
and run the following commands inside your heroku app directory:

    heroku labs:enable runtime-dyno-metadata -a <<HEROKU_APP_NAME>>
    
<!--    heroku config:set LOGZIO_REGION=https://<<LOGZIO_LISTENER>>:8053   -->
    
    heroku config:set LOGZIO_TRACING_TOKEN=<<LOGZIO_TRACING_TOKEN>>
    
    git add .
    
    git commit -m "Otel config" 
    
    git push heroku main
    
Then add the buildpack to the list of heroku buildpacks:

    heroku buildpacks:add --index 1 https://github.com/logzio/heroku-buildpack-otel.git
    
    git commit --allow-empty -m "Rebuild slug"
    
    git push heroku main
    
Wait a few seconds and you will see your heroku app traces in Logz.io platform


