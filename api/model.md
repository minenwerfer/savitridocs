# Model

In Savitri, Model has the same meaning as in any ODM/ORM (it uses Mongoose ODM in the background, so a Savitri model is essentially a Mongoose model). By default, the model of a collection is automatically generated at runtime as long as the description file is placed correctly. If though you have to access the model layer of a collection to leverage virtuals, middlewares, etc, then you may do it by creating a `collectionId.model.{js,ts}` file.

Savitri exposes a `createModel` function to create a Mongoose model from a description. Its second argument is a object with the following properties:

```typescript
import { createModel } from '@savitri/api'
import PersonDescription from './person.description'

export default createModel(PersonDescription, {
  schemaCallback: (schema) => {
    schema.virtual('first_name').get(function() {
      return this.name.split(' ').pop()
    })
  }
})
```


## Virtuals

In order to avoid conflict errors when using virtuals with existing properties on description and still be able to use it on UI or type generation you have to set `s$meta` to `true` inside the property on the description.
