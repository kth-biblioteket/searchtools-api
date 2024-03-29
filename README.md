#
KTHB Searchtools API

Meilisearch server med sökbara data för UG-users, KTH-Anställda(historik) samt HR

##
```bash

### KTH UG

#### Skapa index
curl \
  -X POST 'http://localhost:7700/indexes' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "uid": "ugusers",
    "primaryKey": "sAMAccountName"
  }' \
  -H 'Authorization: Bearer xxxxxxxxxxxxxxx'

#### Definiera facetter/filter
curl \
  -X PATCH 'http://localhost:7700/indexes/ugusers/settings' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxxxx' \
  --data-binary '{
      "filterableAttributes": [
          "ugPrimaryAffiliation",
          "title",
          "sn",
          "kthPAGroupMembership",
          "ugClass"
      ]
  }'

#### Ladda data
curl \
  -X POST 'http://localhost:7700/indexes/ugusers/documents' \
  -H 'Content-Type: application/json' \
  --data-binary @ugusers.json \
 -H 'Authorization: Bearer xxxxxx'

#### Uppdatera data
curl \
  -X PUT 'http://localhost:7700/indexes/ugusers/documents' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxxxx' \
  --data-binary @ugusers.json \
#### Ta bort index
 curl \
  -X DELETE 'http://localhost:7700/indexes/ugusers' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxx'

#### Ta bort data
 curl \
  -X DELETE 'http://localhost:7700/indexes/ugusers/documents' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxxx'

#### Visa updates
 curl \
  -X GET 'http://localhost:7701/indexes/ugusers/updates' \
  -H 'Authorization: Bearer xxxxx'

#### Visa data
 curl \
  -X GET 'http://localhost:7700/indexes/ugusers/documents' \
  -H 'Authorization: Bearer xxxxx'



### KTH Anställda

curl \
  -X POST 'http://localhost:7700/indexes' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "uid": "kthanst",
    "primaryKey": "id"
  }' \
  -H 'Authorization: Bearer xxx'

curl \
  -X POST 'http://localhost:7700/indexes/kthanst/documents' \
  -H 'Content-Type: application/json' \
  --data-binary @kthanst.json \
  -H 'Authorization: Bearer xxx'

curl \
  -X PATCH 'http://localhost:7700/indexes/kthanst/settings' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxx' \
  --data-binary '{
      "filterableAttributes": [
          "Befattning",
          "Orgnamn",
          "Skola"
      ]
  }'

  curl \
  -X GET 'http://localhost:7700/indexes/kthanst/updates' \
  -H 'Authorization: Bearer xxxx'


##### Hr
  #### Skapa index
curl \
  -X POST 'http://localhost:7700/indexes' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "uid": "hr",
    "primaryKey": "kthid"
  }' \
  -H 'Authorization: Bearer xxxxx'

#### Definiera facetter/filter
curl \
  -X PATCH 'http://localhost:7700/indexes/hr/settings' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxxx' \
  --data-binary '{
      "filterableAttributes": [
          "unit_name",
          "lastname",
          "emp_desc"
      ]
  }'

#### Ladda data
curl \
  -X POST 'http://localhost:7700/indexes/hr/documents' \
  -H 'Content-Type: text/csv' \
  --data-binary @hr.csv \
  -H 'Authorization: Bearer xxxx'


##### Databaslistan
  #### Skapa index
curl \
  -X POST 'http://localhost:7700/indexes' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "uid": "dbl",
    "primaryKey": "id"
  }' \
 -H 'Authorization: Bearer xxxxxx'

#### Definiera facetter/filter
curl \
  -X POST 'http://localhost:7701/indexes/dbl/settings' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer xxxxxxx'
  --data-binary '{
      "filterableAttributes": [
          "publisher",
          "types",
          "categories",
          "startletter",
          "access"
      ]
  }'

#### Ladda data
curl \
  -X POST 'http://localhost:7700/indexes/dbl/documents' \
  -H 'Content-Type: application/json' \
  --data-binary @dbl.json \
  -H 'Authorization: Bearer xxxxxx'

#### Visa data
 curl \
  -X GET 'http://localhost:7700/indexes/dbl/documents' \
  -H 'Authorization: Bearer xxxxxx'

curl \
  -X GET 'http://localhost:7701/indexes/dbl/updates'

 ### General

#### Hämta nycklar
curl \
  -X GET 'http://localhost:7700/keys' \
  -H "X-Meili-API-Key: xxxxxxxxxxxxxxxx" 

#### Hämta index
  curl \
  -X GET 'http://localhost:7700/indexes' \
  -H 'Authorization: Bearer xxxx'


#### Docker

Dockerfile build:
kthb@ub-ref:~/docker/meilidbl$ sudo docker build -t meilidbl_7 .

kthb@ub-ref:~/docker/meilidbl$ tail startup.sh
#!/bin/bash

kthb@ub-ref:~/docker/meilidbl$ tail Dockerfile
# syntax=docker/dockerfile:1
FROM meilidbl:6.0
COPY startup.sh /startup.sh
RUN ["chmod", "+x", "/startup.sh"]
ENTRYPOINT ["/startup.sh"]


meilisearch --db-path /meilifiles --http-addr '0.0.0.0:7700'


Run med volume:
sudo docker run  -dp 127.0.0.1:7701:7700 -v meilidbl:/meilifiles --restart unless-stopped meilidbl_7

curl \
  -X POST 'http://127.0.0.1:7701/indexes/movies/documents' \
  -H 'Content-Type: application/json' \
  --data-binary @movies.json
```