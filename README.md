# Micro Action Protocol

Micro Action Protocol is based on HTTP.

### Request

- method: POST
- headers: {'Content-Type': 'application/json'}
- body: {cmd: String, input: Any}

Any params or args should be put into `input`.

### Response

- headers: {'Content-Type': 'application/json'}
- body: {ok: Boolean, code: Any, output: Any, error: Any}
 
If action succeeds, `ok=true`, else (fails or throws) `ok=false`.
 
`code` is used to help identify the cases of this response.
 
`output` is the result data carried in the response.
 
`error` may be included to help caller reason about the problem.
 
- if `ok=true, code=undefined`, this is the only possible ok response.
- if `ok=true`, `output` is the result data.
- if `ok=false, code=undefined`, this is a non-ok response with unknown error.
- if `ok=false`, `output` can be used to complement `code` to provide further details.
- if `ok=false`, `error` may be included to help the **developer** reason about what happened, but it should not be 
used to branch application logic since the content is arbitrary.

### Cmd Pattern

`cmd` is the conjunction of handler definition and invocations.

When you call `route` function and pass in a map of handlers, the key of each handler becomes its cmd pattern.

When you request a service with a `cmd`, it will be used to match predefined handlers.
 
The cmd pattern is simply a relative url string like `find/user?by=id`.
The path part is used to define the action and subject.
And the query pairs are just tags used to refine the action definition.

The query pairs are order-insensitive so if you have a cmd pattern `find/user?by=id&fields=all`,
both `find/user?by=id&fields=all` and `find/user?fields=all&by=id` will match it.

Common cmd patterns are like these:

- create
- create/user
- get/user
- get/user?by=id
- get/users?sortBy=name&sortBy=createdAt

Note: `/get/user` and `get/user` are different.

## License

ISC
