---
seo:
  title: Sandbox Mode
  description: Learn how to use Sandbox Mode when sending mail over SendGrid's Web API v3.
  keywords: sandbox mode, test, validation, v3 mail send
title: Sandbox Mode
weight: 0
layout: page
navigation:
  show: true
---

{% warning %}
**This endpoint is currently in beta!**

Since this is not a general release, we do not recommend POSTing production level traffic through this endpoint or integrating your production servers with this endpoint.

*When this endpoint is ready for general release, your code will require an update in order to use the official URI.*

By using this endpoint, you accept that you may encounter bugs and that the endpoint may be taken down for maintenance at any time. We cannot guarantee the continued availability of this beta endpoint. We hope that you like this new endpoint and we appreciate any <a href="mailto:dx+mail-beta@sendgrid.com">feedback</a> that you can send our way.
{% endwarning %}

{% info %}
Sandbox mode is only used to validate your request. The email will never be delivered while this feature is enabled!
{% endinfo %}

Sandbox mode is an optional parameter within `mail_settings`. Enabling sandbox mode allows you to send a test email to ensure that your request body is formatted correctly without delivering the email to any of your recipients.

When making a request with sandbox mode enabled, we will validate the form, type, and shape of your request. In other words, sandbox mode will validate each parameter you include and the structure of your JSON payload. If you include a `template_id` in your sandbox mode request, it will be assumed that you have included a subject line and body within the template. We will not validate this content.

{% anchor h2 %}
Using Sandbox Mode
{% endanchor %}

{% warning %}
When using sandbox mode, you must include the "enable" parameter and it must be given a boolean value of either true, or false. **Do not enclose the boolean value in quotes**, or you will receive the error:

`The sandbox mode enable param should be a boolean value.`
{% endwarning %}

{% anchor h3 %}
Valid Request Body
{% endanchor %}

When your request validates, you will receive a 200 OK response (as opposed to the 202 ACCEPTED response that is returned for successful non-sandbox requests).

{% anchor h3 %}
Invalid Request Body
{% endanchor %}

If you submit a request with sandbox mode enabled, but your request body is invalid, you will receive one or more error messages with error codes, detailed explanations for the error or errors, and links to any relevant documentation.

{% anchor h3 %}
Example Sandbox Mode JSON
{% endanchor %}


*Please note:* The following is an invalid request body intended to demonstrate the validation behavior of sandbox mode for a bad request.

**Request**
{% codeblock lang:json %}
{
	"personalizations": [{
		"to": [{
			"email": "john@example.com"
		}],
		"subject": "Hello, World!"
	}],
	"from": {
		"email": "John Doe"
	},
	"content": {
		"type": "text",
		"value": "Hello, World!"
	},
	"mail_settings": {
		"sandbox_mode": {
			"enable": true
		}
	}
}
{% endcodeblock %}

**Response**
{% codeblock %}
{
  "errors": [
    {
      "field": "from",
      "message": "The from object must at least have an 'email'
parameter with a valid email address and may also contain a 'name' parameter. e.g. {"email": "example@example.com"} or {"email": "example@example.com", "name": "Example Recipient"}"
    }
  ]
}
{% endcodeblock %}
