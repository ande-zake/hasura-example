# start
docker-compose up -d

## signup endpoint with hasura
```
mutation {
  Signup(name: "ande8", password: "ande8", username: "action-ande8") {
    id
    token
  }
}
```



