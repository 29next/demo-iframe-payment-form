# Demo iFrame Payment Form

A single HTML page that tokenizes a bankcard with `NextPayment`, the class exposed by `payment.js`. The card number and security code live in secure iFrames, so the page never sees raw card data. What comes back is a payment method token you pass to the Admin API as `card_token`.

[Open the demo](https://nextcommerceco.github.io/demo-iframe-payment-form/)

The full integration reference is the [Bankcard guide](https://developers.nextcommerce.com/docs/admin-api/guides/payment-methods/bankcard) on the developer docs. This repo is the runnable companion to that guide, and the guide's code samples are copied from `index.html`.

## What the page shows

- The card number and security code iFrames styled to match the surrounding Bootstrap inputs, using the `styling` option.
- A card brand icon that appears next to the number as you type. The brand comes from the `cardType` value on `onFieldStateChange`, and the icons are in `img/cardbrands`.
- Field level validation errors from `onValidation`, plus a general error block for anything reported through `onError`.
- The token and card details from `onTokenized`, with click to copy on each value.

## Run it locally

Serve the folder over HTTP. Opening `index.html` straight from disk does not work, because a `file://` page has a `null` origin and the Spreedly SDK cannot message its iFrames. The fields then keep the browser's default styling and the Pay Now button never enables.

```
python3 -m http.server 8000
```

Then open http://localhost:8000/.

## Use your own store

The script tag in `index.html` loads `payment.js` with the demo store's environment key:

```html
<script src="https://payments.29next.com/js/v1/payment.js?env_key=1o1MFKHBX0TCK28M2eszkRhmdHa"></script>
```

Replace the `env_key` value with your store's Payments Environment Key. You can find it in your dashboard under Settings > Payments, or read it from `payments.environment_key` on the Admin API store detail endpoint. A key that does not match a store still loads the script, but every tokenization attempt will fail.

## Test cards

| Card | Result |
| --- | --- |
| 4111 1111 1111 1111 | Tokenizes |
| 4223 2332 3232 3233 | Rejected as invalid |

Use any name, any future expiry, and any 3 digit security code (4 digits for Amex). The expiry year must be four digits.

## Files

- `index.html` is the whole demo: markup, styling, and the `NextPayment` wiring.
- `img/cardbrands/` holds the brand SVGs used for card type detection.
