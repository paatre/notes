# HTTP

## HTTP headers

### Content-Disposition

Indicates if the content should be displayed as part of the page or downloaded as an attachment. It's also used as header for a `multipart/form-data` to provide information about each subpart of the form.

#### Syntax

```
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"
Content-Disposition: form-data; name="fieldName"
Content-Disposition: form-data; name="fieldName"; filename="filename.jpg"
```

