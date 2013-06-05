###Methods

`get_me(path)`
Returns the currently authenticated user's profile information located by
`path`. `path` is an array containing path identifiers. For example, to get to
profile["phoneNumbers"]["mobile"]["phoneNumber"] you would pass in `["phoneNumbers",
, "mobile", "phoneNumber"];


`get_all_me()`

Returns the currently authenticated user's profile information. The returned
information should be a JSON object that fits the following format:

```
{
  "phoneNumbers": {
    "home": {
      "type": "home",
      "phoneNumber": "555-555-1234"
    },
    "mobile": {
      "type": "mobile",
      "phoneNumber": "555-555-4321"
    },
  },
  
  "addresses": {
    "home": {
      "type": "home",
      "name": "John Doe",
      "postal": "12345"
    },
    "work": {
      "type": "work",
      "name": "Johnathan H. Doe",
      "postal": "12345"
      ...
    },
    "vacation": {
      "type": "vacation",
      "name": "John Doe",
      "postal": "12345"
    },
  },

  "emails": {
    "home": {
      "type": "home",
      "email": "johndoe@example.com"
    },
    "work": {
      "type": "work",
      "email": "johnathandoe@examplework.com"
    }
  }
}
```

###Events:

`pds new_profile_item_available`
