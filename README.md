# cims-documentation

## Architecture

![Architecture Diagram](/ArchitectureDiagram.png)


# Room Service Schema
```
scalar Date

type Query {
    rooms(inp: RoomFilterInput): [Room]!
    posts(inp: PostFilterInput!): [Post]!
}

type Mutation {
    createRoom(inp: CreateRoomInput!): Room!
    updateRoom(inp: UpdateRoomInput!): Room!
    saveTasks(roomId: ID!, task: [TaskItemInput!]!): [Task!]!
    addMember(roomId: ID!, member: String!): Room!
    removeMember(roomId: ID!, member: String!): Room!
    joinRoom(roomCode: String!): Room!
    createPost(post: PostInput!): Post!
}

type Room {
    id: ID!
    name: String!
    code: String!
    members: [String!]!
    posts: [Post]!
    tasks: [Task]
    createdBy: String!
    createdAt: Date!
}

type Task {
    id: String!
    name: String!
    customAttrs: String
    room: Room# Maybe not needed?
}

input TaskInput {
    id: String
    name: String!
    customAttrs: String
}

type Post {
    id: ID!
    type: PostType!
    room: Room!
    attachments: [File]!
    text: String
    labels: [String!]!
    parentPost: Post
    childPosts: [Post]
    createdBy: String!
    createdAt: Date!
}

type File {
    id: ID!
    name: String!
    file: String!
}

input CreateRoomInput {
    name: String!
    members: [String!]
}

input UpdateRoomInput {
    id: ID!
    name: String
}


input RoomFilterInput {
    id: ID
    name: String
    code: String
    member: [String]
    createdBy: String
}

input PostFilterInput {
    id: ID
    type: PostType
    text: String
    room: String
    parentPost: ID
    createdBy: String
}

input PostInput {
    room: ID!
    type: PostType!
    attachments: [Upload]!
    text: String
    labels: [String!]!
    parentPost: ID
}
```

# File Storage Service APIs
## POST: file-upload
**ContentType:** multipart-formdata
**Params:**
    - file: *mutipart File*
    - name?: *string*
    - directory?: *string*
    - labels: *JSON Array (String)*
    - mimetype: *String*

## POST: search
**ContentType:** application/json
**Params:**
    - labels: *Array<String>*

## GET: /get/:id
**Params:** *id* **NOTE**: This is a URL parameter
**Description:** *Returns the file specified by the id*
