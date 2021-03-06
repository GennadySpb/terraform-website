---
layout: enterprise2
page_title: "Private Terraform Enterprise Settings - API Docs - Terraform Enterprise"
sidebar_current: "docs-enterprise2-api-admin-settings"
---

# Private Terraform Enterprise Settings API

-> **Note**: These API endpoints are in beta and are subject to change.

-> **Note**: These endpoints are only available in Private Terraform instances and only accessible by site administrators.

-> **Pre-release**: These API endpoints are not yet available in the current Private Terraform Enterprise release.

## List General Settings
`GET /api/v2/admin/general-settings`

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | [JSON API document][] (`type: "general-settings"`) | Successfully listed General settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request GET \
  https://app.terraform.io/api/v2/admin/general-settings
```

### Sample Response

```json
{
  "data": {
    "id": "general",
    "type": "general-settings",
    "attributes": {
      "limit-user-organization-creation": true
    }
  }
}
```

## Update General Settings
`PATCH /api/v2/admin/general-settings`

Status  | Response                                     | Reason
--------|----------------------------------------------|----------
[200][] | [JSON API document][] (`type: "general-settings"`) | Successfully updated the General settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action
[422][] | [JSON API error object][]                    | Malformed request body (missing attributes, wrong types, etc.)

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[422]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Request Body

This PATCH endpoint requires a JSON object with the following properties as a request payload.

Key path                    | Type   | Default | Description
----------------------------|--------|---------|------------
`data.attributes.limit-user-organization-creation`| bool     | `true` | When set to `true`, limits the ability to create organizations to users with the `site-admin` permission only.

### Sample Payload

```json
{
  "data": {
    "attributes": {
      "limit-user-organization-creation": true
    }
  }
}
```

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request PATCH \
  --data @payload.json \
  https://app.terraform.io/api/v2/admin/general-settings
```

### Sample Response

```json
{
  "data": {
    "id":"general",
    "type":"general-settings",
    "attributes": {
      "limit-user-organization-creation": true
    }
  }
}
```

## List SAML Settings
`GET /api/v2/admin/saml-settings`

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | [JSON API document][] (`type: "saml-settings"`) | Successfully listed SAML settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request GET \
  https://app.terraform.io/api/v2/admin/saml-settings
```

### Sample Response

```json
{
  "data": {
    "id": "saml",
    "type": "saml-settings",
    "attributes": {
      "enabled": true,
      "debug": false,
      "idp-cert": "SAMPLE-CERTIFICATE",
      "slo-endpoint-url": "https://example.com/slo",
      "sso-endpoint-url": "https://example.com/sso",
      "attr-groups": "MemberOf",
      "attr-site-admin": "SiteAdmin",
      "site-admin-role": "site-admins",
      "sso-api-token-session-timeout": 1209600,
      "acs-consumer-url": "https://example.com/users/saml/auth",
      "metadata-url": "https://example.com/users/saml/metadata"
    }
  }
}
```

## Update SAML Settings
`PATCH /api/v2/admin/saml-settings`

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | [JSON API document][] (`type: "saml-settings"`) | Successfully updated SAML settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action
[422][] | [JSON API error object][]                    | Malformed request body (missing attributes, wrong types, etc.)

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[422]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Request Body

This PATCH endpoint requires a JSON object with the following properties as a request payload.

If `data.attributes.enabled` is set to `true`, all remaining attributes must have valid values. You can omit attributes if they have a default value, or if a value was set by a previous update. Omitted attributes keep their previous values.

Key path                    | Type   | Default | Description
----------------------------|--------|---------|------------
`data.attributes.enabled`    | bool   | `false` | Allows SAML to be used. If true, all remaining attributes are required.
`data.attributes.debug`    | bool   | `false` | Enables a SAML debug dialog that allows an admin to see the SAMLResponse XML and processed values during login.
`data.attributes.idp-cert`   | string |         | Identity Provider Certificate specifies the PEM encoded X.509 Certificate as provided by the IdP configuration.
`data.attributes.slo-endpoint-url` | string |         | Single Log Out URL specifies the HTTPS endpoint on your IdP for single logout requests. This value is provided by the IdP configuration.
`data.attributes.sso-endpoint-url` | string |         | Single Sign On URL specifies the HTTPS endpoint on your IdP for single sign-on requests. This value is provided by the IdP configuration.
`data.attributes.attr-groups`| string | `"MemberOf"` | Team Attribute Name specifies the name of the SAML attribute that determines team membership.
`data.attributes.attr-site-admin`| string |`"SiteAdmin"`| Specifies the role for site admin access, overriding the "Site Admin Role" method.
`data.attributes.site-admin-role`| string |`"site-admins"`| Specifies the role for site admin access, provided in the list of roles sent in the Team Attribute Name attribute.
`data.attributes.sso-api-token-session-timeout`| integer | 1209600 | Specifies the Single Sign On session timeout in seconds, defaulting to 14 days.

```json
{
  "data": {
    "attributes": {
      "enabled": true,
      "debug": false,
      "idp-cert": "SAMPLE-CERTIFICATE",
      "slo-endpoint-url": "https://example.com/slo",
      "sso-endpoint-url": "https://example.com/sso",
      "attr-groups": "MemberOf",
      "attr-site-admin": "SiteAdmin",
      "site-admin-role": "site-admins",
      "sso-api-token-session-timeout": 1209600
    }
  }
}
```

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request PATCH \
  --data @payload.json \
  https://app.terraform.io/api/v2/admin/saml-settings
```

### Sample Response

```json
{
  "data": {
    "id":"saml",
    "type":"saml-settings",
    "attributes": {
      "enabled": true,
      "debug": false,
      "idp-cert": "SAMPLE-CERTIFICATE",
      "slo-endpoint-url": "https://example.com/slo",
      "sso-endpoint-url": "https://example.com/sso",
      "attr-groups": "MemberOf",
      "attr-site-admin": "SiteAdmin",
      "site-admin-role": "site-admins",
      "sso-api-token-session-timeout": 1209600,
      "acs-consumer-url": "https://example.com/users/saml/auth",
      "metadata-url": "https://example.com/users/saml/metadata"
    }
  }
}
```

## List SMTP Settings
`GET /api/v2/admin/smtp-settings`

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | [JSON API document][] (`type: "saml-settings"`) | Successfully listed SMTP settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request GET \
  https://app.terraform.io/api/v2/admin/smtp-settings
```

### Sample Response

```json
{
  "data": {
    "id": "smtp",
    "type": "smtp-settings",
    "attributes": {
      "enabled": true,
      "host": "example.com",
      "port": 25,
      "sender": "sample_user@example.com",
      "auth": "login",
      "username": "sample_user"
    }
  }
}
```

## Update SMTP Settings
`PATCH /api/v2/admin/smtp-settings`

When this request is submitted, a test message will be sent to the specified `test-email-address` address. If the test message delivery fails, the API will return an error code indicating the reason for the failure.

Status  | Response                                     | Reason
--------|----------------------------------------------|----------
[200][] | [JSON API document][] (`type: "smtp-settings"`) | Successfully updated the SMTP settings
[401][] | [JSON API error object][]                    | SMTP user credentials are invalid
[404][] | [JSON API error object][]                    | User unauthorized to perform action
[422][] | [JSON API error object][]                    | Malformed request body (missing attributes, wrong types, etc.)
[500][] | [JSON API error object][]                    | SMTP server server error
[504][] | [JSON API error object][]                    | SMTP server timeout

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[401]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[422]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422
[500]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504
[504]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Request Body

This PATCH endpoint requires a JSON object with the following properties as a request payload.

If `data.attributes.enabled` is set to `true`, all remaining attributes must have valid values. You can omit attributes if they have a default value, or if a value was set by a previous update. Omitted attributes keep their previous values.

Key path                    | Type   | Default | Description
----------------------------|--------|---------|------------
`data.attributes.enabled`   | bool   | `false` | Allows SMTP to be used. If true, all remaining attributes are required.
`data.attributes.host`      | string |         | The host address of the SMTP server.
`data.attributes.port`      | integer|         | The port of the SMTP server.
`data.attributes.sender`    | string |         | The desired sender address.
`data.attributes.auth`      | string | `"none"`| The authentication type. Valid values are `"none"`, `"plain"`, and `"login"`.
`data.attributes.username`  | string |         | The username used to authenticate to the SMTP server. Only required if `data.attributes.auth` is set to `"login"` or `"plain"`.
`data.attributes.password`  | string |         | The username used to authenticate to the SMTP server. Only required if `data.attributes.auth` is set to `"login"` or `"plain"`.
`data.attributes.test-email-address`| string     |  | The email address to send a test message to. Not persisted and only used during testing.

### Sample Payload

```json
{
  "data": {
    "attributes": {
      "enabled": true,
      "host": "example.com",
      "port": 25,
      "sender": "sample_user@example.com",
      "auth": "login",
      "username": "sample_user",
      "password": "sample_password",
      "test-email-address": "test@example.com"
    }
  }
}
```

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request PATCH \
  --data @payload.json \
  https://app.terraform.io/api/v2/admin/smtp-settings
```

### Sample Response

```json
{
  "data": {
    "id":"smtp",
    "type":"smtp-settings",
    "attributes": {
      "enabled": true,
      "host": "example.com",
      "port": 25,
      "sender": "sample_user@example.com",
      "auth": "login",
      "username": "sample_user"
    }
  }
}
```

## List Twilio Settings
`GET /api/v2/admin/twilio-settings`

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | [JSON API document][] (`type: "twilio-settings"`) | Successfully listed Twilio settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request GET \
  https://app.terraform.io/api/v2/admin/twilio-settings
```

### Sample Response

```json
{
  "data": {
    "id": "twilio",
    "type": "twilio-settings",
    "attributes": {
      "enabled": true,
      "account-sid": "12345abcd",
      "from-number": "555-555-5555"
    }
  }
}
```

## Update Twilio Settings
`PATCH /api/v2/admin/twilio-settings`

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | [JSON API document][] (`type: "twilio-settings"`) | Successfully listed Twilio settings
[404][] | [JSON API error object][]                    | User unauthorized to perform action
[422][] | [JSON API error object][]                    | Malformed request body (missing attributes, wrong types, etc.)

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[422]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Request Body

This PATCH endpoint requires a JSON object with the following properties as a request payload.

If `data.attributes.enabled` is set to `true`, all remaining attributes must have valid values. You can omit attributes if they have a default value, or if a value was set by a previous update. Omitted attributes keep their previous values.

Key path                    | Type   | Default | Description
----------------------------|--------|---------|------------
`data.attributes.enabled`    | bool   | `false` | Allows Twilio to be used. If true, all remaining attributes are required.
`data.attributes.account-sid`  | string   |  | The Twilio account id.
`data.attributes.auth-token` | string   |  | The Twilio authentication token.
`data.attributes.from-number`| string     |  | The Twilio registered phone number which will be used to send the message.

```json
{
  "data": {
    "attributes": {
      "enabled": true,
      "account-sid": "12345abcd",
      "auth-token": "sample_token",
      "from-number": "555-555-5555"
    }
  }
}
```

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request PATCH \
  --data @payload.json \
  https://app.terraform.io/api/v2/admin/twilio-settings
```

### Sample Response

```json
{
  "data": {
    "id":"twilio",
    "type":"twilio-settings",
    "attributes": {
      "enabled": true,
      "account-sid": "12345abcd",
      "from-number": "555-555-5555"
    }
  }
}
```

## Verify Twilio Settings
`POST /api/v2/admin/twilio-settings/verify`

Uses the `test-number` attribute to send a test SMS when Twilio is enabled.

Status  | Response                                         | Reason
--------|--------------------------------------------------|-------
[200][] | none | Twilio test message sent successfully
[400][] | [JSON API error object][]                    | Verification settings invalid (missing test number, Twilio disabled, etc.)
[404][] | [JSON API error object][]                    | User unauthorized to perform action

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[400]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[JSON API document]: https://www.terraform.io/docs/enterprise/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

### Request Body

This POST endpoint requires a JSON object with the following properties as a request payload.

Key path                    | Type   | Default | Description
----------------------------|--------|---------|------------
`data.attributes.test-number`| string     |  | The target phone number for the test SMS. Not persisted and only used during testing.

```json
{
  "data": {
    "attributes": {
      "test-number": "555-555-0000"
    }
  }
}
```

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request POST \
  --data @payload.json \
  https://app.terraform.io/api/v2/admin/twilio-settings/verify
```
