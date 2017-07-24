## Welcome to Open APIs Guard

Open APIs Guard is an Open Source API gateway and Microservices management solutions. 

### Overview

Open APIs Guard is built on top of Java, Spring framework, Apache Cassandra and other open source projects which can be deployed to most of the Java Servlet containers such as Apache Tomcat, Jboss, etc.

### API Doc
```markdown

# create api
curl -X POST \
  http://localhost:8080/apiguard/apis \
  -H 'content-type: application/json' \
  -d '{
  "id": "f8157910-d857-11e6-8bc0-2d9f23b1a052",
  "creationDate": 1484178301729,
  "request_uri": "/google/(.*)/abc",
  "name": "google",
  "downstream_uri": "http://www.google.com"
  }'

# update api (downstream_uri)
curl -X PATCH \
  http://localhost:8080/apiguard/apis \
  -H 'content-type: application/json' \
  -d '{
  "id": "f8157910-d857-11e6-8bc0-2d9f23b1a052",
  "creationDate": 1484178301729,
  "request_uri": "/google/(.*)/abc",
  "name": "google",
  "downstream_uri": "http://www.msn.com"
}'

# create client
curl -X POST \
  http://localhost:8080/apiguard/clients \
  -H 'content-type: application/json' \
  -d '{"id":"Jason"}'

# add key auth
curl -X POST \
  http://localhost:8080/apiguard/clients/Jason/key-auth \
  -H 'content-type: application/json' \
  -d '{"request_uri":"/google/(.*)/abc", "key":"33eb155c090a41d5c1b68d9d3d835ca1t"}'

# invoke with key auth
curl -X GET \
  http://localhost:8080/apiguard/apis/google/123/abc \
  -H 'apikey: 33eb155c090a41d5c1b68d9d3d835ca1t'

# add basic auth
curl -X POST http://localhost:8080/apiguard/clients/Jason/basic-auth -H 'content-type: application/json' -d '{"request_uri":"/google/(.*)/abc", "password":"helloworld"}'

# invoke with basic auth - [Authorization: basic base64(username:password)]
curl -X GET \
  http://localhost:8080/apiguard/apis/google/123/abc \
  -H 'Authorization: basic SmFzb246aGVsbG93b3JsZAo='

# add http signature auth
curl -X POST \
  http://localhost:8080/apiguard/clients/Jason/signature-auth \
  -H 'content-type: application/json' \
  -d '{
	"client_alias":"abc-nprod-20170321",
	"secret" : "70335ca6-081f-11e7-93ae-92361f003251",
	"request_uri" : "/google/(.*)/abc"
    }'

 # invoke with http signature
 /Users/slam/Work/apiguard/sign.sh --key Jason:abc-nprod-20170321 --secret 70335ca6-081f-11e7-93ae-92361f003251 -X GET http://localhost:8080/apiguard/apis/google/123/abc


# add ldap auth
curl -v -X POST http://localhost:8080/apiguard/clients/tesla/ldap-auth -H 'content-type: application/json' -d '{"request_uri":"/google/(.*)/[0-9]+", "ldap_url":"ldap://ldap.forumsys.com:389","admin_dn":"cn=read-only-admin,dc=example,dc=com","admin_Password":"password","user_base":"dc=example,dc=com","user_attribute":"uid","cache_expire_seconds":60}'

# invoke with ldap auth - [Authorization: ldap base64(username:password)]
curl -v -X GET \
  http://localhost:8080/apiguard/apis/google/ho1/1 \
  -H 'Authorization: ldap dGVzbGE6cGFzc3dvcmQ='

 # add jwt auth (return secret)
 curl -v -X POST http://localhost:8080/apiguard/clients/tesla/jwt-auth -H 'content-type: application/json' -d '{"request_uri":"/google/(.*)/[0-9]+"}'

 # invoke with jwt - issuer:7ceb15c7-7e69-4094-95e6-973e13b4af56 "secret": "0ce5f35e-a9b9-4717-93bd-4d3daa6b60d3"
 # https://www.jsonwebtoken.io/ - paste secret, header then body to create jwt
curl http://localhost:8080/apiguard/apis/google/ho1/1 -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiI3Y2ViMTVjNy03ZTY5LTQwOTQtOTVlNi05NzNlMTNiNGFmNTYiLCJleHAiOjE0OTkxMjMzMTcsIm5iZiI6MTQ0MjQyNjQ1NCwiaWF0IjoxNDQyNDI2NDU0LCJqdGkiOiI1NWRjN2Y4ZC1mZmMzLTRmODEtOTE3Mi0zMDZjYTVlMzlkYmQifQ.c6Gp4btTpu68yC5_GrAYAMdaeUWGf_xtVP0c9vtXxvY'
```
