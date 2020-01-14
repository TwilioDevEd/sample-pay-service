<a  href="https://www.twilio.com">
<img  src="https://static0.twilio.com/marketing/bundles/marketing/img/logos/wordmark-red.svg"  alt="Twilio"  width="250"  />
</a>
 
# Payment over the phone with TwiML\<Pay>

[![Actions Status](https://github.com/twilio-labs/sample-pay-service/workflows/Node%20CI/badge.svg)](https://github.com/twilio-labs/sample-pay-service/actions)

## About

This sample application shows how to do a bill payment over the phone using TwiML\<Pay> with a credit card.

Implementations in other languages:

| .NET | Java | Python | PHP | Ruby |
| :--- | :--- | :----- | :-- | :--- |
| TBD  | TBD  | TBD    | TBD | TBD  |

### How it works

It uses the TwiML\<Pay> to prompt the user to enter a credit card's information to pay a bill that can be configured on the UI, then it will process the payment through Stripe (which needs to be added as a connector on the Twilio console) and will let the user know if the payment was successful or not.

![Pay Diagram](https://twilio-cms-prod.s3.amazonaws.com/images/pay-diagram-1-final.width-1600.png)

## Features

- Node.js web server using [Express.js](https://npm.im/express)
- Basic web user interface using [Pug](https://npm.im/pug) for templating and Bootstrap for UI
- User interface to configure payment details.
- Payment details can be stored in a JSON database using lowdb.
- Unit tests using [`mocha`](https://npm.im/mocha) and [`chai`](https://npm.im/chai)
- [Automated CI testing using GitHub Actions](/.github/workflows/nodejs.yml)
- Linting and formatting using [ESLint](https://npm.im/eslint) and [Prettier](https://npm.im/prettier)
- Project specific environment variables using `.env` files and [`dotenv-safe`](https://npm.im/dotenv-safe) by comparing `.env.example` and `.env`.
- One click deploy buttons for Heroku and Glitch

## Set up
### Requirements

- [Node.js](https://nodejs.org/)
- A Twilio account - [sign up](https://www.twilio.com/try-twilio)

It is recommended to follow the tutorial on [how to capture your first payment using \<Pay>](https://www.twilio.com/docs/voice/tutorials/how-capture-your-first-payment-using-pay) as it already describes in detail all of the following steps:

1. Create a [Stripe's account](https://dashboard.stripe.com/register).
2. [Purchase a voice-enabled Twilio phone number](https://www.twilio.com/console/phone-numbers/incoming) (or use one of yours if you already have it).
3. Configure [Stripe \<Pay> Connector](https://www.twilio.com/console/voice/pay-connectors) on the your Twilio console.

### Twilio Account Settings

This application should give you a ready-made starting point for writing your
own payment over the phone application. Before we begin, we need to collect
all the config values we need to run the application:

| Config&nbsp;Value | Description                                                                                                                                                  |
| :---------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Auth&nbsp;Token   | Used to authenticate - [You'll find it on your Twilio's console](https://www.twilio.com/console).                                                         |
| Payment&nbsp;connector | This has to be configured on your [Twilio's console](https://www.twilio.com/console/voice/pay-connectors). There you should select the Stripe connector and link it to your Stripe's account                                                         |

**NOTE:** The Auth token will validate the requests are coming from the authorized account, otherwise you will get a `403 Forbidden` response.

### Local development

After the above requirements have been met:

1. Clone this repository and `cd` into it

```bash
git clone git@github.com:twilio-labs/sample-pay-service.git

cd sample-pay-service
```

2. Install dependencies

```bash
npm install
npm install -g ngrok
```

3. Set your environment variables


```bash
npm run setup
```

See [Twilio Account Settings](#twilio-account-settings) to locate the necessary environment variables.

4. Run the application

```bash
npm start
```

Alternatively, you can use this command to start the server in development mode. It will reload whenever you change any files.

```bash
npm run dev
```

5. Once you have your server running, you need to expose your `localhost` to a public domain so the Twilio webhook can reach the expected endpoint. This is easy using `ngrok`:
```
ngrok http 3000
``` 
This will generate a url similar to: `https://cd2ef758.ngrok.io`. Copy that link (you must use `https` or Twilio will reject the request).

6. Now, you must take that public URL and configure this as a webhook for one of your phone numbers in the console. [Here is small a guide](https://www.twilio.com/docs/voice/quickstart/node#allow-twilio-to-talk-to-your-application) on how to that.

**IMPORTANT!** Don't forget to add `/pay` (so it looks like `https://cd2ef758.ngrok.io/pay`) at the end of the url since that is the endpoint in which the application will listen for the call.

7. Navigate to [http://localhost:3000](http://localhost:3000) to see some sample credit card details to test the payment.

8. You can also navigate to [http://localhost:3000/config](http://localhost:3000/config) to override the default payment details.

9. That's it! Now call the Twilio phone number you configured and follow the instructions to complete the payment.

10. You can see if the payment was charged on your Stripe dashboard. Take a look at [this](https://www.twilio.com/docs/voice/tutorials/how-capture-your-first-payment-using-pay#test-your-application) for more details.

### Tests

You can run the tests locally by typing:

```bash
npm test
```

### Cloud deployment

Additionally to trying out this application locally, you can deploy it to a variety of host services. Here is a small selection of them.

Please be aware that some of these might charge you for the usage or might make the source code for this application visible to the public. When in doubt research the respective hosting service first.

*Don't forget to set the environmental variables on each hosting service!*

| Service                           |                                                                                                                                                                                                                           |
| :-------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Heroku](https://www.heroku.com/) | [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://www.heroku.com/deploy/?template=https://github.com/twilio-labs/sample-pay-service/tree/master)                                                                                                                                       |
| [Glitch](https://glitch.com)      | [![Remix on Glitch](https://cdn.glitch.com/2703baf2-b643-4da7-ab91-7ee2a2d00b5b%2Fremix-button.svg)](https://glitch.com/edit/#!/remix/clone-from-repo?REPO_URL=https://github.com/twilio-labs/sample-pay-service.git) |

Here are some notes about the services:
- **Heroku**: Very straightforward, just create an account and after clicking the deploy button you need to follow the instructions and that's it.
- **Glitch**: It requirers an additional step. Once you click on the deploy button, you need to manually create the file `.env` and add set `TWILIO_AUTH_TOKEN` variable. You can duplicate the `.env.example` file and edit it accordingly.

## Resources
- This project was generated using this [sample NodeJS template](https://github.com/twilio-labs/sample-template-nodejs)

- [GitHub's repository template](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template) functionality

## Contributing

This template is open source and welcomes contributions. All contributions are subject to our [Code of Conduct](https://github.com/twilio-labs/.github/blob/master/CODE_OF_CONDUCT.md).

[Visit the project on GitHub](https://github.com/twilio-labs/sample-pay-service)

## License

[MIT](http://www.opensource.org/licenses/mit-license.html)

## Disclaimer

No warranty expressed or implied. Software is as is.

[twilio]: https://www.twilio.com
