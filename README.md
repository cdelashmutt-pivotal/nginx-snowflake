A tiny example to deploy a static site via Tanzu Application Platform

# Deploy from Github
tanzu apps workload create --file https://raw.githubusercontent.com/cdelashmutt-pivotal/nginx-snowflake/config/workload.yaml 

# Deploy from local
tanzu apps workload create --file config/workload.yaml --local-path . --source-image registry.you.can.write.to/repository-path