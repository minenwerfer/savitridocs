# Function

When used within the context of this document, a Function (or API Function when required to be more explicit) refers to an endpoint function. Funcions can be used by entities of both collection and controllabe types. To register a function using the filesystem use the `collectionId@functionName.{ts,js}` naming pattern.

Example:
```
.
├── collections
│   └── person
│       ├── functions
│       │   └── person@hello.ts
│       └── person.description.ts
├── controllables
│   └── functions
│       └── dashboard
│           └── dashboard@get.ts
```

A function has the following signature in Typescript:

```typescript
export type ApiFunction<Props=unknown, Return={}> = (
  props: Props,
  context: ApiContext
) => Return
```

Whereas `ApiContext` is declared as following:

```typescript
export type ApiContext = {
  apiConfig: ApiConfig
  accessControl: AccessControl
  injected: Record<string, any>
  token: DecodedToken

  validate: <T>(what: T, required?: Array<keyof T>) => void
  collection: CollectionFunctions
  entity: AnyFunctions
  log: (message: string, details?: Record<string, any>) => Promise<Log>
  collections: Record<string, AnyFunctions>
  controllables: Record<string, AnyFunctions>

  descriptions?: Record<string, MaybeCollectionDescription>
}
```
