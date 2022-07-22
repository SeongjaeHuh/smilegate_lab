### Create a table with a JSON column
```
create table jsonRecord(jsonRecord variant);
```
```
INSERT INTO JSONRECORD (jsonrecord) select PARSE_JSON('{"customer": "Walker"}');
INSERT INTO JSONRECORD (jsonrecord) select PARSE_JSON('{"customer": "Stephen"}');
INSERT INTO JSONRECORD (jsonrecord) select PARSE_JSON('{"customer": "Aphrodite", "age": 32}');

INSERT INTO JSONRECORD (jsonrecord) select PARSE_JSON(' {
            "customer": "Aphrodite",
            "age": 32,
            "orders": [{
                                    "product": "socks",
                                    "quantity": 4
                        },
                        {
                                    "product": "shoes",
                                    "quantity": 3
                        }
            ]
 }');
INSERT INTO JSONRECORD (jsonrecord) select PARSE_JSON(' {
            "customer": "Nina",
            "age": 52,
            "orders": [{
                                    "product": "socks",
                                    "quantity": 3
                        },
                        {
                                    "product": "shirt",
                                    "quantity": 2
                        }
            ]
 }');
 
 INSERT INTO JSONRECORD (jsonrecord) select PARSE_JSON(' {
            "customer": "Maria",
            "age": 22,
     "address" : { "city": "Paphos", "country": "Cyprus"},                                                   
            "orders": [{
                                    "product": "socks",
                                    "quantity": 3
                        },
                        {
                                    "product": "shirt",
                                    "quantity": 2
                        }
            ]
 }');
```
![image](https://user-images.githubusercontent.com/52474199/162948064-213dccad-2bb5-47bb-a8ae-6121db732b54.png)

### How to select JSON data in Snowflake

The format for selecting data includes all of the following:

tableName:attribute
tableName.attribute.JsonKey
tableName.attribute.JsonKey[arrayIndex]
tableName.attribute[‘JsonKey’]
get_path(tableName, attribute)
 
```
 select jsonrecord:customer from JSONRECORD;
```

![image](https://user-images.githubusercontent.com/52474199/162948197-3b461865-8694-44cf-8f4f-91f641e9b8cd.png)

We can also use the get_path() function:
```
select get_path(jsonrecord, 'address') from JSONRECORD;

```
![image](https://user-images.githubusercontent.com/52474199/162949156-29e4f44c-fae5-4737-984d-f655ab611a6c.png)

```
select jsonrecord:address.city from JSONRECORD where jsonrecord:customer = 'Maria';
```
![image](https://user-images.githubusercontent.com/52474199/162949350-5a86922e-45e9-4503-8ebc-40d352c0c798.png)

```
select jsonrecord['address']['city'] from JSONRECORD where jsonrecord:customer = 'Maria';
```
![image](https://user-images.githubusercontent.com/52474199/162949905-32058695-3808-433b-bed1-027fce34c9ac.png)

```
select jsonrecord['orders'][0] from JSONRECORD where jsonrecord:customer = 'Maria';
```
![image](https://user-images.githubusercontent.com/52474199/162949989-e222b58c-823b-4dbd-aa1b-bb44bffa2b7d.png)

```
select jsonrecord:customer, jsonrecord:orders  from JSONRECORD ,
   lateral flatten(input => jsonrecord:orders) prod ;
```
![image](https://user-images.githubusercontent.com/52474199/162950228-5e67af61-7a50-4b59-bde3-5a41210c76fe.png)
