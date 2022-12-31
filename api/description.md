# Description

A Description is the JSON-representable structure that describes the properties of a collection. It's a intersection of [JSON Schema](https://json-schema.org), meaning it has a subset of its features while adding some others. In the filesystem pattern it is saved as `collectionId.description.{json,js,ts}`.

The minimost possible JSON-type description contained the two required properties ($id, properties) is as follows:

```json
{
  "$id": "person",
  "properties": {
    "name": {
      "type": "string"
    },
    "jobs": {
      "type": "array",
      "items": {
        "enum": [
          "pilot",
          "doctor",
          "programmer"
        ]
      }
    }
  }
}
```

Once passed as an `init` property or properly placed on the filesystem, a description serves the following purposes:

- Database model definition
- Runtime validation with `context.validate`
- Form building (if used alongside `@savitri/ui`)
- Static typing (on Typescript projects)


## Static typing

If you're running a Typescript project, then you'll likely want to type check against your description and build a type from your schema for further usage. Both goals can be achieved, but since Typescript can't compare literals and perform type checking at the same time, we have to rely on the object splitting strategy, declaring properties as a separate read-only object. Then we can build our schema type using Schema auxiliary type and merge the description together with the makeDescription function.

```typescript
import { makeDescription, Schema } from '@savitri/api'

export type Person = Schema<typeof schema>

const schema = {
  $id: 'person',
  properties: {
    name: {
      type: 'string'
    },
    items: {
      type: 'array',
      items: {
        enum: [
          'pilot',
          'doctor',
          'programmer'
        ]
      }
    }
  }
}

export default makeDescripion(schema)
```
