# django-oidc-provider

## Installation

1. Install with pip: `pip install django-oidc-provider`
2. Add `'oidc_provider'` app to `INSTALLED_APPS`
3. Include OIDC urls to `urlpatterns`: `path('openid/', include('oidc_provider.urls', namespace='oidc_provider'))`
4. Run migrations: `python manage.py migrate`
5. Generate a server RSA key: `python manage.py creatersakey`
6. Add `LOGIN_URL = '/accounts/login/'` to settings

## RSA keys

Server RSA keys are used to sign and encrypt ID Tokens. They are stored in the `RSAKey` model. At least one RSA key needs to be created for the server. The keys can be created either in the Django admin or with the `python manage.py creatersakey` command. Public keys are exposed via `jwks_uri` endpoint:

```http
GET /openid/jwks HTTP/1.1
Host: localhost:8000
{
    "keys":[
        {
            "use":"sig",
            "e":"AQAB",
            "kty":"RSA",
            "alg":"RS256",
            "n":"3Gm0pS7ij_SnY96wkbaki74MUYJrobXecO6xJhvmAEEhMHGpO0m4H2nbOWTf6Jc1FiiSvgvhObVk9xPOM6qMTQ5D5pfWZjNk99qDJXvAE4ImM8S0kCaBJGT6e8JbuDllCUq8aL71t67DhzbnoBsKCnVOE1GJffpMcDdBUYkAsx8",
            "kid":"a38ea7fbf944cc060eaf5acc1956b0e3"
        }
    ]
}
```

The keys are primarily used withing `encode_id_token` and `decode_id_token` functions in `oidc_provider.lib.utils.token.py`:

```python
def encode_id_token(payload, client):
    """
    Represent the ID Token as a JSON Web Token (JWT).
    Return a hash.
    """
    keys = get_client_alg_keys(client)
    _jws = JWS(payload, alg=client.jwt_alg)
    return _jws.sign_compact(keys)


def decode_id_token(token, client):
    """
    Represent the ID Token as a JSON Web Token (JWT).
    Return a hash.
    """
    keys = get_client_alg_keys(client)
    return JWS().verify_compact(token, keys=keys)
```

## Clients

Clients (RPs) are created and managed in Django admin's Client model admin. Following information can be entered about the clients:
- Main information: client name, client type, 
