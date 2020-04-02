# Integrations



![Navigation Integrations](img/nav-integrations.png)

Integrations allow exchange data between the Lobaro Platform and other applications.

## REST

Navigate to: [Integrations -> REST](https://platform.lobaro.com/#/organisation/integrations/rest/){: target="_blank"}

The API is located and documented at:  
 [https://backend.lobaro.com/api](https://backend.lobaro.com/api){: target="_blank"}

### Access Tokens

All API calls must be authenticated with an access token.
Access tokens can be managed on the REST Integration page.

Each token has a list of roles assigned to it. 

**Token roles:**

* **network-server**: Allows to write device data. Required for LoRaWAN Network Server Integrations.

Add the Token into the HTTP request Header:

* Field: `Authorization`
* Value: `Bearer eyJhbGciOiJIU...`

#### Receive data from a LoRaWAN network server

1. Create an Access Token in the Lobaro Platform with the role `network-server` to allow write access for device data.
2. Configure your network server to send data to the corresponding endpoint (see list below)
using the token (e.g. `Bearer eyJhbGciOiJ...`) in the `Authorization` header field.  
Please consult the documentation of your network server for further instructions.

* Chirpstack: https://backend.lobaro.com/api/loraserver/uplink
* Everynet: https://backend.lobaro.com/api/everynet/data

We also support other network servers on request, e.g. TTN, Element-IoT, Firefly, Actility, Loriot, ... Just ask!

### Filter query parameters

Filters parameters can be appended to some requests in the form of `f:<parameter>=<op>:<value>` 
e.g. `f:createdAt=gt:<timestamp>` to filter by createdAt date. 

The Value must be URL encoded e.g. a timestamp might look like `gt:2000-01-01T02:37:00%2B01:00`


The allowed `<parameter>` is specified for each endpoint separately.

`<op>` must be one of the following operators:

| In Query | Meaning |
|----------|---------|
| `eq` | `=` |
| `lte` | `<=` |
| `lt` | `<` |
| `gte` | `>=` |
| `gt` | `>` |

If no operator is given the default `eq` operator will be used.

## HTTP (forwarding)

Navigate to: [Integrations -> HTTP](https://platform.lobaro.com/#/organisation/integrations/http/){: target="_blank"}

Forward data from the Lobaro Platform to any HTTP(s) endpoint.

**HTTP Method**: The HTTP method to use for the request.

**Target URL**: Any reachable HTTP server endpoint.

**Authentication**: One of the following authentication schemes can be used:

* Basic Auth: Adds Username & password to request headers.
* Bearer Token: Adds a Bearer Token to request headers. Just put the Token (without `Bearer` prefix).
* OAuth: Fetch a OAuth token with the given scope from a Token URL using a ClientId and Secret as BasicAuth credentials.
The fetched token is used for all API calls and automatically refreshed when invalid. 

**Headers**: List of headers to be set on HTTP request in the format: `<FieldName>: <Value>` (e.g. `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`)

## MQTT (forwarding)

Navigate to: [Integrations -> MQTT](https://platform.lobaro.com/#/organisation/integrations/mqtt/){: target="_blank"}

Forward data from the Lobaro Platform via MQTT Publish to any MQTT broker.

**Broker URL**: must be in one of the following formats:

* `mqtt[s]://host.domain[:port]`
* `tcp[s]://host.domain[:port]`

Default port for mqtt/tcp is `1883`. Default port for mqtts/tcps is `8883 `. 

**ClientId**: Used on MQTT connect. Default is: `lobaro-{RND}`.
`{RND}` will be replaced by a unique random string. 

**Username / Password**: Used to authenticate with the MQTT broker.

**Topic**: The MQTT topic to publish sensor data.


!!! note
    Currently we only support username/password authentication for MQTT yet.
