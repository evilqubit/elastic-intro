###Indices and types 
Every time you store data in _Elasticsearch_ it get's saved inside an **index** which has a **type**. compared to _MongoDB_ an index is similar to a database, and a type similar to a collection. Compared to _SQL_ an index would be like a database, and a type like a table.

convention: 
```
localhost:9200/index/type/
```

**Important note** different types living in the same index cannot have the same field name with different config or field type


For example the following two documents can't co-exist since they share the same index, and both have a _city_ attribute of different types, _string_ and _object_ respectively 

```
localhost:9200/test/users/1

{
    "city": "cityID123"
}

localhost:9200/test/city/1

{
    "city": {
        "name": "Toronto"
    }
}
```

When developing with elasticsearch there are 3 main steps we have to consider. **Mapping**, **Indexing**, and **Querying** data

###1. Mapping

Mapping is used to define how elastic should store and index a particular document and it's fields. 

However if no mapping was introduced to a specific field on pre-index time, elastic will [dynamically](https://www.elastic.co/guide/en/elasticsearch/guide/current/dynamic-mapping.html) add a **generic** type to that field. Although this may sound tempting, it is not! since generic types are very basic and do not meet queries expectations most of the time.

Moving forward with this tutorial we will base our examples on the following data schema: 

```
{
    "first_name": "bam",
    "last_name": "margera",
    "gender": "male",
    "age": 36
}
``` 

So to make things more efficient we're gunna create the index, type and mapping for the schema in one shot. Something that looks like the following: 

```
PUT localhost:9200/test/

{
    "mappings": {
        "users": {
            "properties": {
                "age": {
                    "type": "long"
                },
                "first_name": {
                    "type": "string"
                },
                "gender": {
                    "type": "string"
                },
                "last_name": {
                    "type": "string"
                }
            }
        }
    }
}

```
so creating an Index called test, a type called users with 4 fields that it contains.
note that field types can have the following values: _string_, _date_, _long_, _double_, _boolean_, _ip_, _object_, _nested_, _geo_point_, _geo_shape_
