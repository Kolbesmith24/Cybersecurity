# APIs
Many APIs are used to interact with a database, such that we would be able to specify the requested table and the requested row within our API query, and then use an HTTP method to perform the operation needed.

We can easily specify the table and the row we want to perform an operation on through such APIs with cURL. 

For example, for the `api.php` endpoint in our example, if we wanted to update the `city` table in the database, and the row we will be updating has a city name of `london`, then the URL would look something like this:

```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london
```
## CRUD
We can easily specify the table and the row we want to perform an operation on through such APIs. Then we may utilize different HTTP methods to perform different operations on that row. In general, APIs perform 4 main operations on the requested database entity:

| Operation | HTTP Method | Description                                        |
| --------- | ----------- | -------------------------------------------------- |
| Create    | POST        | Adds the specified data to the database table      |
| Read      | GET         | Reads the specified entity from the database table |
| Update    | PUT         | Updates the data of the specified database table   |
| Delete    | DELETE      | Removes the specified row from the database table  |
We can perform all 4 CRUD operations with cURL.
### Read
We can simply specify the table name after the API (e.g. `/city`) and then specify our search term (e.g. `/london`), as follows:
```bash
curl http://<SERVER_IP>:<PORT>/api.php/city/london
```

To have it properly formatted in JSON format, we can pipe the output to the `jq` utility.
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq
```
- Add the `-s` to silent any unneeded cURL output.
- We can change the `london` to any search term and we can see all the matching results.
- Or we can leave it blank to retrieve all entries in the table
### Create
To add a new entry, we can use an HTTP POST request.

We can simply POST our JSON data, and it will be added to the table. As this API is using JSON data, we will also set the `Content-Type` header to JSON, as follows:
```bash
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

Now, we can read the content of the city we added (`HTB_City`):
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq
```
### Update
`PUT` (a.k.a. update) is used to update API entries and modify their details.

Using `PUT` is quite similar to `POST` in this case, with the only difference being that we have to specify the name of the entity we want to edit in the URL, otherwise the API will not know which entity to edit.

So, all we have to do is specify the `city` name in the URL, change the request method to `PUT`, and provide the JSON data like we did with POST, as follows:
```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

We see in the example above that we first specified `/city/london` as our city, and passed a JSON string that contained `"city_name":"New_HTB_City"` in the request data. So, the london city should no longer exist, and a new city with the name `New_HTB_City` should exist
### DELETE
We simply specify the city name for the API and use the HTTP `DELETE` method, and it would delete the entry, as follows:
```bash
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```
# How are these operations vulnerable?
Tt would be considered a vulnerability if anyone can modify or delete any entry. Each user would have certain privileges on what they can `read` or `write`, where `write` refers to adding, modifying, or deleting data. 

To authenticate our user to use the API, we would need to pass a cookie or an authorization header (e.g. JWT), as done in [[POST Requests]].

Other than that, the operations are similar to what we practiced in this section.