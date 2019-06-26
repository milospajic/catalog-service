# Product catalog

REST API enabling product manipulation

### How to use the app

The API is protected with JWT token. To use the api you can use long living JWT: 

`Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6NDcxNDkyNDAwOSwiaWF0IjoxNTYxMzI0MDA5fQ.U6arTWzxzu-Fl2AhLEloYBZ8wNdHPqUu1ffWjQp6vnSc36dOoRLLyggO7KdvNPGbnLxlKQbXbRzbCtMc5eleqA`

It was not requirement, but this application can also generate tokens for you. After you start your app either by docker or locally, to get valid token just execute the script: 

`./scripts/get-jwt-token.sh`

In real life scenarios this is not good microservices design and this functionality of providing tokens should be responsibility of another microservice, but for simplicity I kept it here. For the same reason ( and also to save some time) I didn't write any unit tests for security package. 

With either of this tokens you'll be able to make API calls

Best you start with batch import of products. The script

`./scripts/batch-import-products.sh`

will import 100 products for you ( or more, if you run it more times). From that point on, you have following endpoints:

`GET /catalog-api/v1/products` - to get all products, default pagination is 20. This endpoint accepts different request parameters so you can search by title, description, sort, define pagination ( number of records per page, or pick specific page). Here are some examples of endpoints for this:

```
GET /catalog-api/v1/products?size=50 - get first page with 50 records 
GET /catalog-api/v1/products?page=1&sort=brand,desc - get second page, with default 20 records and sorted by brand, descending
GET /catalog-api/v1/products?page=1&size=5 - get second page with 5 records
GET /catalog-api/v1/product/2 - get product with id=2
GET /catalog-api/v1/products?description=Description 3 - get products having description property equal to "Description 3"
GET /catalog-api/v1/products?title=Title 1&description=Description 1 - get products having title=Jacket 1 and description=Description 1

POST /catalog-api/v1/products - to batch create products. It's what /script/batch-import-products.sh does
POST /catalog-api/v1/product - to create one product

PUT /catalog-api/v1/product/1 - to override a product with id=1, or if there is no product with this id, to create a new one with first available id

PATCH /catalog-api/v1/product/1 - to partially update product with id=1

DELETE /catalog-api/v1/product/1 - to delete product with id=1
```

### How to run the service locally

Open terminal and go to project's home directory

To start the app run:

`./gradlew bootRun`

### Run as docker container

You can get already built docker image from dockerhub by executing:

`docker pull milospajic/catalog-service:gfg`

If you want to build it yourself, then please open terminal and go to project's home directory. To create docker image run:

`./scripts/create-docker-image.sh`

to run the image you've built yourself, please execute:

`./scripts/run-docker-image.sh`

If you're having slower machine, it can take a while for the container to start, also image creation can timeout, in which case you should execute the script again, or download the one from dockerhub

### How to run the tests locally

In terminal, in project's home directory execute: 
`./gradlew test`

