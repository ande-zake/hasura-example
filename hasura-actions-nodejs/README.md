# start
docker-compose up -d

### Note login and signup
docker yg masih bisa digunakan untuk kedua endpoint ini masih docker-compose-without-password

#### signup endpoint with hasura
```
mutation {
  Signup(name: "ande8", password: "ande8", username: "action-ande8") {
    id
    token
  }
}
```

#### login endpoint with hasura
```
{
  Login(password: "ande7", username: "action-ande7") {
    name
    token
    username
    id
  }
}
```

