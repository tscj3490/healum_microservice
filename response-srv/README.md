# Task Server

Task server is a microservice to store response.

## Getting started

1. Install Consul

	Consul is the default registry/discovery for go-micro apps. It's however pluggable.
	[https://www.consul.io/intro/getting-started/install.html](https://www.consul.io/intro/getting-started/install.html)

2. Run Consul
	```
	$ consul agent -server -bootstrap-expect 1 -data-dir /tmp/consul
	```

3. Start a AarangoDB

4. Download and start the service

	```shell
	go get ./src/server/response-srv && response-srv -config ./src/server/config.json --broker=nats --transport=nats --broker_address=127.0.0.1:4222 --transport_address=127.0.0.1:4222
	```

	OR as a docker container

	```shell
	docker run microhq/response-srv -config config.json --broker=nats --transport=nats --broker_address=127.0.0.1:4222 --transport_address=127.0.0.1:4222 --registry_address=YOUR_REGISTRY_ADDRESS
	```
## The API
Task server implements the following RPC Methods

Task
- Create
- Read
- Update
- Delete
- All
- Search
- ByCreator
- ByAssign
- Filter
- CountByUser

### Task.Create

```shell
curl -X POST \
  http://127.0.01:8080/server/responses/response \
  -H 'content-type: application/json' \
  -d '{
    "response": {
		"id":          "111",
		"title":       "response1",
		"org_id":      "orgid",
		"description": "description1",
		"creator_id":  "222",
		"user_id": 	   "333",
        "assignee_id": "333",
		"category":	   "category1",
        "due":         1517891917,
		"tags": 	   ["a","b","c"]
	}
}'
{
    "data": {
        "response": {
            "id": "333",
            "org_id": "orgid",
            "title": "response1",
            "description": "description1",
            "created": 1518139992,
            "updated": 1518139992,
            "user_id": "333",
            "creator_id": "222",
            "assignee_id": "333",
            "category": "category1",
            "tags": [
                "a",
                "b",
                "c"
            ],
            "due": 1517891917
        }
    }
}
```

### Task.Read

```shell
curl -X GET \
  http://127.0.01:8080/server/responses/response/111 \
  -H 'content-type: application/json'
{
    "data": {
        "response": {
            "id": "333",
            "org_id": "orgid",
            "title": "response1",
            "description": "description1",
            "created": 1518139992,
            "updated": 1518139992,
            "user_id": "333",
            "creator_id": "222",
            "assignee_id": "333",
            "category": "category1",
            "tags": [
                "a",
                "b",
                "c"
            ],
            "due": 1517891917
        }
    }
}
```

### Task.Update

```shell
curl -X PUT \
  http://localhost:8080/server/responses/response \
  -H 'content-type: application/json' 
  -d '{
    "response": {
		"id":          "111",
		"title":       "response2",
		"org_id":      "org2",
		"description": "description1",
		"creator_id":  "222",
		"user_id": 	   "333",
        "assignee_id": "333",
		"category":	   "category1",
        "due":         1517891917,
		"tags": 	   ["a","b","c"]
	}
}'
{}
```

### Task.Delete
```shell
curl -X DELETE \
  http://127.0.0.1:8080/server/responses/response/111 \
  -H 'content-type: application/json'
{}
```

### Task.All

```shell
curl -X GET \
  http://127.0.0.1:8080/server/responses/all \
  -H 'content-type: application/json'
{
    "data": {
        "responses": [
            {
                "id": "111",
                "org_id": "orgid",
                "title": "response1",
                "description": "description1",
                "created": 1518139992,
                "updated": 1518139992,
                "user_id": "333",
                "creator_id": "222",
                "assignee_id": "333",
                "category": "category1",
                "tags": [
                    "a",
                    "b",
                    "c"
                ],
                "due": 1517891917
            }
        ]
    }
}
```

### Task.Search


```shell
curl -X POST \
  http://127.0.0.1:8080/server/responses/search \
  -H 'content-type: application/json' \
  -d '{
	"name":"response1"
}'
{
    "data": {
        "responses": [
            {
                "id": "111",
                "org_id": "orgid",
                "title": "response1",
                "description": "description1",
                "created": 1518139992,
                "updated": 1518139992,
                "user_id": "333",
                "creator_id": "222",
                "assignee_id": "333",
                "category": "category1",
                "tags": [
                    "a",
                    "b",
                    "c"
                ],
                "due": 1517891917
            }
        ]
    }
}
```

### Task.ByCreator
```shell
curl -X GET \
  http://127.0.0.1:8080/server/responses/creator/222 \
  -H 'content-type: application/json'
{
    "data": {
        "responses": [
            {
                "id": "111",
                "org_id": "orgid",
                "title": "response1",
                "description": "description1",
                "created": 1518139992,
                "updated": 1518139992,
                "user_id": "333",
                "creator_id": "222",
                "assignee_id": "333",
                "category": "category1",
                "tags": [
                    "a",
                    "b",
                    "c"
                ],
                "due": 1517891917
            }
        ]
    }
}
```

### Task.ByAssign
```shell
curl -X GET \
  http://127.0.0.1:8080/server/responses/assign/333 \
  -H 'content-type: application/json'
{
    "data": {
        "responses": [
            {
                "id": "111",
                "org_id": "orgid",
                "title": "response1",
                "description": "description1",
                "created": 1518139992,
                "updated": 1518139992,
                "user_id": "333",
                "creator_id": "222",
                "assignee_id": "333",
                "category": "category1",
                "tags": [
                    "a",
                    "b",
                    "c"
                ],
                "due": 1517891917
            }
        ]
    }
}
```

### Task.Filter

```shell
curl -X POST \
  http://127.0.0.1:8080/server/responses/filter \
  -H 'content-type: application/json' \
  -d '{
    "category": ["category1", "category2"],
    "status": [1,2],
    "priority": [1,2],
}'
{
    "data": {
        "responses": [
            {
                "id": "111",
                "org_id": "orgid",
                "title": "response1",
                "description": "description1",
                "created": 1518139992,
                "updated": 1518139992,
                "user_id": "333",
                "creator_id": "222",
                "assignee_id": "333",
                "category": "category1",
                "tags": [
                    "a",
                    "b",
                    "c"
                ],
                "due": 1517891917
            }
        ]
    }
}
```

### Task.CountByUser

```shell
curl -X GET \
  http://127.0.0.1:8080/server/responses/count/333 \
  -H 'content-type: application/json'
{
    "response_count": {
        "expired": 1,
        "assigned": 1
    }
}
```