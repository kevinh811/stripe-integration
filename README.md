# Stripe Integration

This example features a sample e-commerce store that uses [Stripe Elements](https://stripe.com/docs/elements), [PaymentIntents](https://stripe.com/docs/payments/payment-intents) for [dynamic authentication](https://stripe.com/docs/payments/3d-secure), and the [Sources API](https://stripe.com/docs/sources) to illustrate how to accept card payments on the web.

## Overview

<img src="public/images/screenshots/demo-chrome.png" alt="Demo on Google Chrome" width="610"><img src="public/images/screenshots/demo-iphone.png" alt="Demo on Safari iPhone X" width="272">

## Environment Setup
### Requirements

You’ll need the following:

- [Node.js](http://nodejs.org) >=10.0.0
- Modern browser that supports ES6 (Chrome to see the Payment Request)
- Stripe account to accept payments ([sign up](https://dashboard.stripe.com/register) for free).
- Stripe CLI for webhook testing.  Follow [these installation steps](https://github.com/stripe/stripe-cli#installation) to install the Stripe CLI

After the installation has finished, authenticate the CLI with your Stripe account:

    stripe login --project-name=stripe-integration
Some payment methods require receiving a real-time webhook notification to complete a charge. We're using the [Stripe CLI](https://github.com/stripe/stripe-cli#listen) to forward webhook events to our local development server. Additionally this app is bundled with [ngrok](https://ngrok.com/), to serve the app locally via HTTPS, which is required for the Payment Request API.

## Project Setup
Copy the environment variables file from the root of the repository:

    cp .env.example .env

Update `.env` with your own [Stripe API keys](https://dashboard.stripe.com/account/apikeys) and any other configuration details. These environment variables are loaded and used in [`server/node/config.js`](/server/node/config.js), where you can review and edit other options such as the app currency and your Stripe account country.

Install dependencies using npm:

    npm install

To start the webhook forwarding run:

    npm run webhook

The Stripe CLI will let you know that webhook forwarding is ready and output your webhook signing secret:

    > Ready! Your webhook signing secret is whsec_xxx

Please copy the webhook signing secret (`whsec_xxx`) to your `.env` file.


## Payments Integration

The frontend code for the demo is in the `public/` directory.

The core logic of the Stripe integration is mostly contained within two files:

1.  [`public/javascripts/payments.js`](public/javascripts/payments.js) creates the payment experience on the frontend using Stripe Elements.
2.  [`server/node/routes.js`](server/node/routes.js) defines the routes on the backend that create Stripe charges and receive webhook events.

### Running the Node Server

In a separate terminal window, start the local server:

    npm run start

Lastly, you will see the ngrok URL to serve our app via HTTPS. For example:

    https://<example>.ngrok.io

Use this URL in your browser to start the app.  Use test card numbers to test the implementation. (https://stripe.com/docs/testing)

## Credits
- Code: [Romain Huet](https://twitter.com/romainhuet) and [Thorsten Schaeff](https://twitter.com/thorwebdev)
- Design: [Tatiana Van Campenhout](https://twitter.com/tatsvc)
