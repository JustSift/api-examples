# API Examples

### Querying users:

```js
{
  users(query: "jeff") {
    id
    firstName
    lastName
    email
    skills
    interests
  }
}
```

### Pagination:

_In this example, we are searching for users with 'cooking' listed as a skill. Useful for creating groups around common interests!_

```js
{
  users(query: "cooking", page: 3, pageSize: 6) {
    email
    skills
  }
}
```

### Sorting:

_This example returns new hires._

```js
{
  users(query: "", sortBy: anniversaryDate, sortDirection: descending) {
    email
    anniversaryDate
  }
}
```

### Specifying nested types with nested sorting:

```js
{
  users(query: "engineer", sortBy: education_school_name) {
    email
    title
    education {
      degreeType
      fieldsOfStudy
      minors
      school {
        name
      }
    }
  }
}
```

### Grouping multiple queries into one:

_`remoteTeamMembers ` and `recruiters ` are just aliases that can be changed to anything_

```js
{
  remoteTeamMembers: users(query: "", sortBy: isRemote) {
    displayName
    email
    isRemote
  }
  recruiters: users(query: "recruiter") {
    displayName
    email
    title
  }
}
```

### Querying information on a specific Type (in this case, `User`):

```js
{
  __type(name: "User") {
    fields {
      name
      type {
        name
        kind
        ofType {
          name
        }
      }
    }
  }
}
```

### Filtering

First, start off with a query. In the in-browser playground, paste the following into the big white box on the left side of the screen:

```graphql
query ($filters: [UsersFilter]) {
  users(filters: $filters) {
    # add your desired fields here
    email
  }
}

```

Then, add query variables. In the in-browser playground, click the `QUERY VARIABLES` link on the bottom left side of the screen then paste in one of the following JSON objects:

##### Single filter

Example 1:

```js
{
  "filters": {
    "subTeam": "team burt"
  }
}
```

Example 2:

```js
{
  "filters": {
    "firstName_starts_with": "jeff"
  }
}
```

##### Applying multiple `and` filters

Example 1:

```js
{
  "filters": {
    "education_school_name_not_contains": "michigan",
    "education_school_exists": true
  }
}
```

Example 2:

```js
{
  "filters": {
    "anniversaryDate_gte": "1/1/2018",
    "teamLeader": "John Smith"
  }
}
```

##### Applying multiple `or` filters

```js
{
  "filters": {
    "or": {
      "interests_item_contains": "popcorn",
      "skills_item_contains": "popcorn"
    }
  }
}
```

##### Applying multiple `or` filters using the same field

Use an array!

```js
{
  "filters": {
    "or": [
      {
      	"title_contains": "engineer"
      },
      {
      	"title_contains": "designer"
      }
    ]
  }
}
```

##### Nesting filters together

```js
{
  "filters": {
    "interests_exist": false,
    "skills_exist": false,
    "or": {
      "directReportCount_gte": 1,
      "anniversaryDate_gte": "1/1/2018"
    }
  }
}
```

### Helpful tips and tricks:

- As you type in the playground, a hint box will show up to give you autocomplete suggestions. If you want the box to appear without typing anything, press `Ctrl` + `Spacebar`.
- On the right side of the playground, you'll see a green `SCHEMA` button. If you click this, and search for `User` (then click the first search result), you will see a detailed list of all fields that can be added to a `users` query.

### Need help? Have suggestions?

Simply email us at api@justsift.com

### Have an example you'd like to add to this list?

Feel free to fork this repository and create a pull request!