---
seo:
  title: cURL Examples for Common Use Cases
  description:
  keywords: migration, v2 mail send, v3 mail send, upgrade
title: cURL Examples for Common Use Cases
weight: 0
layout: page
navigation:
  show: true
---

Below are some cURL examples for several basic use cases to get you sending email through SendGrid's v3 Mail Send endpoint right away!

{% warning %}
**This endpoint is currently in beta!**

Since this is not a general release, we do not recommend POSTing production level traffic through this endpoint or integrating your production servers with this endpoint.

*When this endpoint is ready for general release, your code will require an update in order to use the official URI.*

By using this endpoint, you accept that you may encounter bugs and that the endpoint may be taken down for maintenance at any time. We cannot guarantee the continued availability of this beta endpoint. We hope that you like this new endpoint and we appreciate any <a href="mailto:dx+mail-beta@sendgrid.com">feedback</a> that you can send our way.
{% endwarning %}

{% anchor h2 %}
Hello, World!
{% endanchor %}

{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/send/beta \
  --header 'Authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"personalizations": [{"to": [{"email": "YOU@sendgrid.com"}]}],"from": {"email": "dx@sendgrid.com"},"subject": "Hello, World!","content": [{"type": "text/plain", "value": "Heya!"}]}'
{% endcodeblock %}

{% anchor h2 %}
Sending a Basic Email to Multiple Recipients
{% endanchor %}

{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/send/beta \
  --header 'authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"personalizations": [{"to": [{"email": "recipient@example.com"}],"cc": [{"email":"recipient2@example.com"}, {"email": "recipient3@example.com"}, {"email ":"recipient4@example.com"}]}], "from": {"email": "dx@sendgrid.com"},"subject":"Hello, World!", "content": [{"type": "text/plain", "value": "Heya!"}]}'
{% endcodeblock %}

{% anchor h2 %}
Sending a Basic Email to Multiple Recipients
{% endanchor %}

{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/send/beta \
  --header 'authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"personalizations": [{"to": [{"email": "recipient@example.com"}],"cc": [{"email":"recipient2@example.com"}, {"email": "recipient3@example.com"}, {"email ":"recipient4@example.com"}]}], "from": {"email": "dx@sendgrid.com"},"subject":"Hello, World!", "content": [{"type": "text/plain", "value": "Heya!"}]}'
{% endcodeblock %}

{% anchor h2 %}
Sending a Basic Email Using a Template
{% endanchor %}

{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/send/beta \
  --header 'authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"personalizations": [{"to": [{"email": "recipient@example.com"}]}],"from": {"email": "sender@example.com"},"subject":"Hello, World!","content": [{"type": "text/plain","value": "Heya!"}], "template_id" : "YOUR_TEMPLATE_ID"}'
{% endcodeblock %}

{% anchor h2 %}
Sending a Basic Email at a Scheduled Time
{% endanchor %}

{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/send/beta \
  --header 'authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"personalizations": [{"to": [{"email": "recipient@example.com"}]}],"from": {"email": "sender@example.com"},"subject":"Hello, World!","content": [{"type": "text/plain","value": "Heya!"}], "send_at" : "UNIX_TIMESTAMP_HERE"}'
{% endcodeblock %}

{% anchor h2 %}
Scheduling and Cancelling an Email
{% endanchor %}

You may schedule an email to be sent up to 72 hours in the future by using the "send_at" parameter. You may cancel this same scheduled email by using the [Cancel Scheduled Sends endpoint]({{root_url}}/API_Reference/Web_API_v3/cancel_schedule_send.html).

**Step 1: Generate a batch ID**
{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/batch \
  --header 'authorization: Basic YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
{% endcodeblock %}

**Step 2: Schedule the email to be sent, using your new batch ID**
{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/mail/send/beta \
  --header 'authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"personalizations": [{"to": [{"email": "recipient@example.com"}]}],"from": {"email": "sender@example.com"},"subject":"Hello, World!","content": [{"type": "text/plain","value": "Heya!"}], "send_at" : "UNIX_TIMESTAMP_HERE", "batch_id" : "YOUR_BATCH_ID"}'
{% endcodeblock %}

**Step 3: Cancel the scheduled email**
{% codeblock lang:bash %}
curl --request POST \
  --url https://api.sendgrid.com/v3/user/scheduled_sends \
  --header 'authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{"batch_id":"YOUR_BATCH_ID","status":"cancel"}'
{% endcodeblock %}
