Types of operation and related info in graphql

Operation Types
- query
- mutation
- subscription



# Root Query
- Entry point of the data querying. (server)
```
type Query  {
    fields..
}

```

# Fields
- GraphQL has fields with type which are queried by field name
- GraphQL server traverse through these nested items and returns the corresponding values.

- Query
```
query {
    data {
        id
        val
        data2 {
            id
            val
        }
    }
}
```

- Mutation
```
mutation {
    mutationName (key: "val") {
        retVal
    }
}
```

# Arguments 
- We can pass arguments to fields to filter them
- Arguments can be passed to implement data transformation in server
- args will be received in the server.
  
```
{
    attribute(id: "value) {
        value
        value2(key: "value")
    }
}

```

- In server attribute resolver gets args.id = "value" and value2.resolver gets args.key = "value"

# Aliases
- If we need different results for same object field, we can use aliases to get it renamed
- Aliasing will avoid collission
  
eg: 
```
{
    attrOne: fieldName(id: "value1"){
        value
    }
    attrTwo: fieldName(id: "value2"){
        value
    }
}
```
- The above will return attrOne and attrTwo, and their values where fieldName's id = value1 and value2 respectievely

# Fragments
- Set of data requirements / fields grouped to be reused.
- the data fields will all be available by calling ...fragmentName
  
eg: 
```
{
  leftComparison: hero(episode: EMPIRE) {
    ...fragmentName
  }
  rightComparison: hero(episode: JEDI) {
    ...fragmentName
  }
}

fragment fragmentName on ObjectName {
  name
  appearsIn
  friends {
    name
  }
}
```

## Variables in Fragments
- We can use variables inside fragements with $variable 

eg: 
```
query HeroComparison($first: Int = 3) {
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on <Type> {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

# Operation Name
- We can assign names to an operation (query and mutation)
- query and mutation keyword will work without a name assigned

eg: 
```
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
```

# Variables
- queries can accept variables to sub their args
eg: 
```
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```
- default variables can be assigned with = sign

eg: 
```
query HeroNameAndFriends($episode: Episode = "one") {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

# Directives
- Programatically change structure of query based on argument
eg:
```
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}
```

