# Wallarm API

Wallarm API provides interaction between components of the Wallarm system. You can use Wallarm API methods to create, get or update the following instances:

* vulnerabilities,
* attacks,
* incidents,
* users,
* clients,
* filter nodes, etc.

Description of API methods is given in the API Reference by the link:

* https://apiconsole.eu1.wallarm.com/ for the [EU cloud](../quickstart-en/qs-intro-en.md#eu-cloud)
* https://apiconsole.us1.wallarm.com/ for the [US cloud](../quickstart-en/qs-intro-en.md#us-cloud)

## API Endpoint

API requests are sent to the following URL:

* `https://api.wallarm.com/` for the [EU cloud](../quickstart-en/qs-intro-en.md#eu-cloud)
* `https://us1.api.wallarm.com/` for the [US cloud](../quickstart-en/qs-intro-en.md#us-cloud)

## Authentication of API Requests

The method of API requests authentication depends on the client sending the request:

* [API Reference UI](#api-reference-ui)
* [Your own client](#your-own-client)

### API Reference UI

Token is used for requests authentication. The token is generated after successful authentication in your Wallarm account.

1. Sign in to your Wallarm account by the link:
    * https://my.wallarm.com/ for the EU cloud,
    * https://us1.my.wallarm.com/ for the US cloud.
2. Refresh the API Reference page by the link:
    * https://apiconsole.eu1.wallarm.com/ for the EU cloud,
    * https://apiconsole.us1.wallarm.com/ for the US cloud.
3. Go to the required API method > the **Try it out** section, input parameter values and **Execute** the request.

### Your Own Client

Your UUID and secret key are used.

1. Sign in to your Wallarm account by the link:
    * https://my.wallarm.com/ for the EU cloud,
    * https://us1.my.wallarm.com/ for the US cloud.
2. Refresh the API Reference page by the link:
    * https://apiconsole.eu1.wallarm.com/ for the EU cloud,
    * https://apiconsole.us1.wallarm.com/ for the US cloud.
3. Send the `POST /v1/user` request from the API Reference UI and copy the `uuid` value from the response.
4. Send the `POST /v1/user/renew_secret` request from the API Reference UI and copy the `secret` value from the response.
5. Send the required request from your own client passing the following values:
    * `uuid` in the `X-WallarmAPI-UUID` header parameter,
    * `secret` in the `X-WallarmAPI-Secret` header parameter.

## API Restrictions

Currently Wallarm does not limit the rate of API calls. At the same time Wallarm closely monitors the API usage and may introduce a rate limiting for abusing users.

## API Request Examples

* Get first 50 attacks detected in the last 24 hours

--8<-- "en/include-en/api-request-examples/get-attacks-en.md"

* Get first 50 incidents confirmed in the last 24 hours

--8<-- "en/include-en/api-request-examples/get-incidents-en.md"

* Create the rule to block all requests sent to `/my/api/*`

--8<-- "en/include-en/api-request-examples/create-rule-en.md"