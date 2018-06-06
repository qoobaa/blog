---
layout: post
title: Setting up GCS, encrypted credentials and ActiveStorage
hacker_news_id: 17250655
---

I've spent a while today trying to set up ActiveStorage in a new Rails
application properly. The current Rails guides suggest to declare the
following service `config/storage.yml` file:

```
google:
  service: GCS
  credentials:
    type: "service_account"
    project_id: ""
    private_key_id: <%= Rails.application.credentials.dig(:gcs, :private_key_id) %>
    private_key: <%= Rails.application.credentials.dig(:gcs, :private_key) %>
    client_email: ""
    client_id: ""
    auth_uri: "https://accounts.google.com/o/oauth2/auth"
    token_uri: "https://accounts.google.com/o/oauth2/token"
    auth_provider_x509_cert_url: "https://www.googleapis.com/oauth2/v1/certs"
    client_x509_cert_url: ""
  project: ""
  bucket: ""
```

It looks pretty straightforward, but there's a good chance that you'll
see an `OpenSSL::PKey::RSAError (Neither PUB key nor PRIV key: nested
asn1 error)`, or a YAML parser error when you try to launch the
app. The main issue is the `private_key` line, that tries to inject a
multiline RSA key into the `storage.yml`. The easiest workaround that
I've found so far looks like this:

```
google:
  …
  credentials:
    private_key: "<%= Rails.application.credentials.dig(:gcs, :private_key).lines.join("\\n") %>"
  …
```

I haven't found this issue mentioned anywhere in the docs, so I think
this little trick may be handy until it's is solved officialy in a
more elegant way.
