## Networking
The three aspects of networking that we are interested in when it comes to responding to the network, are a loading state, the data that's returned, and an error object when an error has occurred.

```ts
interface Network {
  loading: boolean
  data: any
  error: any
}
```

Or alternatively, `error` could be a boolean, with the error payload being embodied in the data object (similar to FSA).

```ts
interface Network {
  loading: boolean
  data: any
  error: boolean
}
```

Most of the time we're mapping a resource to an UI, therefore it makes sense to have an API design that's hook based. This ensures that the lifecycle of the network request is contigent on the lifecycle of the component.

```js
const { loading, data, error } = useResource(resources => resources.resource(params), options)
```

This would execute the network request as soon as the component is mounted. There are scenarious though where you may wish to delay this, that's where `options` come in.

One option that we can have is a `defer` flag: `{ defer: true }`. This would result in our network request having to be trigerred manually, and therefore expanding our interface to the following:

```ts
interface DeferedNetwork extends Network {
  fetchResource/executeResource : () => void
}
```

This is perfect for using the same hook based design for `POST` requests, as well as `GET` requests that we wish to delay.

```js
function MyComponent () {
  const { executeResource } = useResource(resources => resources.postResource(), { defer: true })
  
  return (
   <button onClick={() => executeResource()} />
  )
}
```

