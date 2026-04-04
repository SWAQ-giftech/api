# SWAQ API Documentation

API integration documentation for [SWAQ](https://swaq.co/).

This API will allow you to generate unique qr codes as digital images for "self-printing" or to assign pre-printed qr codes to customer email addresses and order references.

You can request access to our demo environment [here](https://swaq.co/sell-swaq/#contact).

We have a [Swagger UI](https://api.stg.swaq.ddn.amalgama.co/api-docs/index.html?urls.primaryName=Merchant%20API%20V1%20Docs) where you can try the endpoints.

## Table of Contents

  * [Language](https://www.google.com/search?q=%23language)
  * [API key](https://www.google.com/search?q=%23api-key)
  * [Self printed integration](https://www.google.com/search?q=%23self-printed-integration)
      * [Create and activate](https://www.google.com/search?q=%23create-and-activate)
      * [Generate qr code](https://www.google.com/search?q=%23generate-qr-code)
  * [Pre-printed integration](https://www.google.com/search?q=%23pre-printed-integration)
      * [List qr codes](https://www.google.com/search?q=%23list-qr-codes)
      * [Activate qr code](https://www.google.com/search?q=%23activate-qr-code)
  * [Swagger UI](https://www.google.com/search?q=%23swagger-ui)

## Language

  - **Sender**: The user (the gift-buying customer) responsible for creating and uploading the video linked to the QR code.
  - **Recipient**: The person receiving the gift who scans the QR code to watch the video.
  - **Card**: The unique QR code system.
  - **Activation**: The process of sending an email to the `Sender` containing an **activation link**. This link allows the Sender to record or upload their video message for the Recipient.
  - **Locale**: The language used for the activation email and the Sender's video upload experience. Supported values: `en`, `fr`, and `en_ar` (English/Arabic bilingual).

## API key

To make use of the endpoints we will provide an API key. You need to send the api key in the `Authorization: Bearer` header.

## Self printed integration

### Create and activate

For the self-printed integration, send a `POST` request to `/1/cards/create_and_activate`. This creates a card and triggers an activation email to the `Sender`.

**Parameters:**

  * `activation_email`: (Required) The Sender's email.
  * `recipient_name`: (Optional) The name of the gift receiver; this is included in the email sent to the Sender.
  * `locale`: (Optional) Language for the email/experience (`en`, `fr`, `en_ar`). Defaults to `en`.

<!-- end list -->

```sh
curl -X POST \
  'https://api.stg.swaq.ddn.amalgama.co/1/cards/create_and_activate' \
  --header 'Accept: */*' \
  --header 'Authorization: Bearer {your_api_key}' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "activation_email": "sender@example.com",
    "recipient_name": "John Doe",
    "locale": "en_ar"
  }' 
```

This will return an `url` (to generate the QR) and an `id`.

### Generate qr code

To generate a QR code image for the `url` using the `card_id`, use `/1/cards/{card_id}/qrcode`:

```sh
curl -X GET \
  'https://api.stg.swaq.ddn.amalgama.co/1/cards/{card_id}/qrcode' \
  --header 'Accept: */*' \
  --header 'Authorization: Bearer {your_api_key}'
```

This QR code should be printed and sent with the gift so the **Recipient** can scan it.

## Pre-printed integration

We provide pre-printed QR codes for fulfillment. You can request them [here](https://swaq.co/sell-swaq/#contact).

### List qr codes

This endpoint lists available QR codes assigned to your merchant account.

```sh
curl -X GET \
  'https://api.stg.swaq.ddn.amalgama.co/1/cards?per_page=1&filter[available_for_activation_eq]=true' \
  --header 'Accept: */*' \
  --header 'Authorization: Bearer {your_api_key}'
```

### Activate qr code

Assign a specific `card_id` to a customer's order. This triggers the activation email to the `Sender`.

```sh
curl -X POST \
  'https://api.stg.swaq.ddn.amalgama.co/1/cards/{card_id}/activate' \
  --header 'Accept: */*' \
  --header 'Authorization: Bearer {your_api_key}' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "activation_email": "sender@example.com",
    "recipient_name": "John Doe",
    "order_reference": "ORD-0072",
    "locale": "en_ar"
  }'
```

## Swagger UI

You can try these endpoints [here](https://api.stg.swaq.ddn.amalgama.co/api-docs/index.html?urls.primaryName=Merchant%20API%20V1%20Docs).

Press the `Authorize` button, enter your `API key`, and you are ready to go.
