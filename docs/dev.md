### Noco DB

On the first access, you'll be asked to configure the administrator account. Suggested password for developers is _nocoDBAndBeyong29@_

Launch Noco DB:
`
docker run -d --name nocodb \
-v "$(pwd)"/nocodb:/usr/app/data/ \
-p 8080:8080 \
nocodb/nocodb:latest
`