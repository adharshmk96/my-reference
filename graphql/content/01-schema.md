GraphQL Schema

# Type system
- GraphQL has a typesystem to define data

## Scalar Types
  1. Int
  2. Float
  3. String
  4. Boolean
  5. ID (represents unique ids)
- Scalars accepts the following args 
  - name
  - description
  - required
  - deprecation_reason
  - default_value
- Scalars are mounted in 
  - Object Types
  - Interface
  - Mutation

### List and NonNull
- List of Scalar
- NonNull value of Scalar
- NonNull List of Scalar

## Object types and fields
- We can define GraphQL objects ( which are also types ) with scalar or other type as fields
- Basic component of representing kinds of Data
```
type <TypeName> {
    <field>: <fieldType>
}
```
eg:
```
type Book {
  title: String
  author: Author
}

type Author {
  name: String
  books: [Book]
}
```

## Query Type
- For data read operations, to be queried 
- resembles an object type with Query keyword
- defines top level entry point
```
type Query {
  books: [Book]
  authors: [Author]
}
```


## Mutation Types
- Type defines entry point for write operations
- single request can include multiple mutations
- These mutations are executed serially to avoid race conditions.
```
type Mutation {
  addBook(title: String, author: String): Book
}
```

## Subscription Type
- Socket subscription ( similar to query )

## Input Type
- Allows to pass objects as arguments to queries and mutations (as opposed to passing only scalar types).
- Input types are special object types 

```
type Mutation {
  createPost(title: String, body: String, mediaUrls: [String]): Post
}

// can be changed into

type Mutation {
  createPost(post: PostAndMediaInput): Post
}

input PostAndMediaInput {
  title: String
  body: String
  mediaUrls: [String]
}
```
- Mutations can change multiple fields, (liking a post = user and post are modified)

## Annotations
- Types can have annotations
```
input PostAndMediaInput {
  """
  Multi line annotation
  """

  "A main title for the post"
  title: String
  "The text body of the post."
  body: String
  "A list of URLs to render in the post."
  mediaUrls: [String]
}

"Description for the type"
type MyObjectType {
  """
  Description for field
  Supports **multi-line** description for your [API](http://example.com)!
  """
  myField: String!

  otherField(
    "Description for argument"
    arg: Int
  )
}


```

## Enum
- Defined set of values for a field
- Only allowed choices will be usable for this field

```
enum AllowedColor {
  RED
  GREEN
  BLUE
}
```

## Interface
- Predefined set of fields which can be inherited or implemented
```
interface MutationResponse {
  code: String!
  success: Boolean!
  message: String!
}

type UpdateUserEmailMutationResponse implements MutationResponse {
  code: String!
  success: Boolean!
  message: String!
  user: User
}

```
