# HTMX

## Attributes

### `hx-delete`

Issues a `DELETE` request to the given URL and swap the returned HTML into the DOM using a swap strategy.

#### Example

```html
<button hx-delete="/account" hx-target="body">
    Delete Your Account
</button>
```

### `hx-get`

Issues a `GET` request to the given URL and swap the returned HTML into the DOM using a swap strategy.

#### Example

```html
<div hx-get="/example">
    Get Some HTML
</div>
```

### `hx-headers`

Adds headers to an AJAX request. Attribute's value is in JSOM.

#### Example

```html
<form hx-post="/endpoint/" hx-headers='{"X-CSRFToken": "token"}' hx-target="#result">
    ...
</form>
```

### `hx-params`

TODO

### `hx-patch`

Issues a `PATCH` request to the given URL and swap the returned HTML into the DOM using a swap strategy.                        

#### Example

```html
<button hx-patch="/account" hx-target="body">
    Patch Your Account
</button>
```

### `hx-post`

Issues a `POST` request to the given URL and swap the returned HTML into the DOM using a swap strategy.                        

#### Example

```html
<button hx-post="/account/enable" hx-target="body">
    Enable Your Account
</button>
```

### `hx-prompt`

Shows a prompt before sending a request. The prompt and therefore the request as well can be cancelled.

#### Example

```html
<button hx-delete="/account" hx-prompt="Enter your account name to confirm deletion">
    Delete My Account
</button>
```

### `hx-push-url`

TODO

### `hx-put`

Issues a `PUT` request to the given URL and swap the returned HTML into the DOM using a swap strategy.                        
#### Example

```html
<button hx-put="/account" hx-target="body">
    Put Money In Your Account
</button>

```
### `hx-swap`

TODO

### `hx-target`

TODO

### `hx-trigger`

TODO

