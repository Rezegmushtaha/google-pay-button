# Google Pay Angular button

This is the Angular component for the Google Pay button.

## Installation

```sh
npm install @google-pay/button-angular
```

## Example usage: Angular website

```html
<google-pay-button
  environment="TEST"
  button-type="buy"
  button-color="black"
  [paymentRequest]="{
    apiVersion: 2,
    apiVersionMinor: 0,
    allowedPaymentMethods: [
      {
        type: 'CARD',
        parameters: {
          allowedAuthMethods: ['PAN_ONLY', 'CRYPTOGRAM_3DS'],
          allowedCardNetworks: ['AMEX', 'VISA', 'MASTERCARD']
        },
        tokenizationSpecification: {
          type: 'PAYMENT_GATEWAY',
          parameters: {
            'gateway': 'stripe'
          }
        }
      }
    ],
    merchantInfo: {
      merchantId: '12345678901234567890',
      merchantName: 'Demo Merchant'
    },
    transactionInfo: {
      totalPriceStatus: 'FINAL',
      totalPriceLabel: 'Total',
      totalPrice: '100.00',
      currencyCode: 'USD',
      countryCode: 'US'
    }
  }"
  (loadpaymentdata)="onLoadPaymentData($event)"
></google-pay-button>
```

More Angular examples can be found in the
[examples folder](https://github.com/google-pay/google-pay-button/tree/main/examples/angular/src/app) of this
repository.

## Documentation

Visit the [Google Pay developer site](https://developers.google.com/pay/api/web/overview) for more information about
integrating Google Pay into your website.

### Properties

<table>
  <tr>
    <th align="left">Property</th>
    <th align="left">Type</th>
    <th align="left">Remarks</th>
  </tr>
  <tr>
    <td><p>buttonColor</p></td>
    <td><p><code>"default" | "black" | "white"</code></p></td>
    <td>
      <p>Optional.</p>
      <p><code>"default"</code>/<code>"black"</code> buttons are suitable to be used on light colored backgrounds, with <code>"white"</code> being appropriate for dark colored backgrounds.</p>
      <p>Default value <code>"default"</code>.</p>
    </td>
  </tr>
  <tr>
    <td><p>buttonSizeMode</p></td>
    <td><p><code>"static" | "fill"</code></p></td>
    <td>
      <p>Optional.</p>
      <p><code>"static"</code> buttons will be sized according to the translated <code>buttonType</code>.</p>
      <p><code>"fill"</code> buttons will be styled according the the element's size. Use this mode when resizing the button with CSS <code>width</code> and <code>height</code> properties.</p>
      <p>Default value <code>"static"</code>.</p>
    </td>
  </tr>
  <tr>
    <td><p>buttonType</p></td>
    <td><p><code>"buy" | "plain" | "donate" | "long" | "short"</code></p></td>
    <td>
      <p>Optional.</p>
      <p><code>"buy"</code> and <code>"donate"</code> display their respective prompts (localized based on the user's browser settings) with the Google Pay logo. The <code>"plain"</code> button only displays the Google Pay logo.</p>
      <p><code>"long"</code> and <code>"short"</code> button types have been renamed to <code>"buy"</code> and <code>"plain"</code>, but are still valid button types for backwards compatability.</p>
      <p>Default value <code>"buy"</code>.</p>
    </td>
  </tr>
  <tr>
    <td><p>environment</p></td>
    <td><p><code>"TEST" | "PRODUCTION"</code></p></td>
    <td>
      <p>Required.</p>
      <p>The Google Pay environment to target.</p>
      <p>Note: in the <code>"TEST"</code> environment, fake payment credentials are returned. In order to use the <code>"PRODUCTION"</code> environment, your website must be registered with Google Pay. This can be done through the <a href="https://pay.google.com/business/console/">Google Pay Business Console</a>.</p>
    </td>
  </tr>
  <tr>
    <td><p>existingPaymentMethodRequired</p></td>
    <td><p><code>boolean</code></p></td>
    <td>
      <p>Optional.</p>
      <p>When set to <code>true</code> (and <code>environment</code> is <code>Production</code>), the Google Pay button will only be displayed if the user already has an existing payment that they can use to make a purchase.</p>
      <p>Default value <code>false</code>.</p>
    </td>
  </tr>
  <tr>
    <td><p>paymentRequest</p></td>
    <td><p><a href="https://developers.google.com/pay/api/web/reference/request-objects#PaymentDataRequest"><code>PaymentDataRequest</code></a></p></td>
    <td>
      <p>Required.</p>
      <p>Request parameters that define the type of payment information requested from Google Pay.</p>
      <p>See <a href="https://developers.google.com/pay/api/web/reference/request-objects#PaymentDataRequest"><code>PaymentDataRequest</code> reference</a> for more information.</p>
    </td>
  </tr>
</table>

### Callbacks/events

<table>
  <tr>
    <th align="left">Callback</th>
    <th align="left">Remarks</th>
  </tr>
  <tr>
    <td>
      <p>cancelCallback</p>
    </td>
    <td>
      <p>Invoked when a user cancels or closes the Google Pay payment sheet.</p>
      <p>Also raised as event <code>"cancel"</code>.</p>
    </td>
  </tr>
  <tr>
    <td>
      <p>errorCallback</p>
    </td>
    <td>
      <p>Invoked an error is encountered in the process of presenting and collecting payment options from the Google Pay payment sheet.</p>
      <p>Also raised as event <code>"error"</code>.</p>
    </td>
  </tr>
  <tr>
    <td>
      <p>paymentAuthorizedCallback</p>
    </td>
    <td>
      <p>Invoked when a user chooses a payment method. This callback should be used to validate whether or not the payment method can be used to complete a payment.</p>
      <p>This would be typically used to perform pre-authorization to ensure that the card is valid and has sufficient funds.</p>
      <p>Note that in order to use this callback <code>paymentRequest.callbackIntents</code> must include <code>PAYMENT_AUTHORIZATION</code>.</p>
      <p>See <a href="https://developers.google.com/pay/api/web/reference/client#onPaymentAuthorized">payment authorization reference</a> for more information.</p>
    </td>
  </tr>
  <tr>
    <td>
      <p>paymentDataChangedCallback</p>
    </td>
    <td>
      <p>Invoked when payment the user changes payment data options including payment method, shipping details, and contact details. This callback can be used to dynamically update <code>transactionInfo</code> when payment details, shipping address, or shipping options change.</p>
      <p>Note that in order to use this callback <code>paymentRequest.callbackIntents</code> must include either <code>SHIPPING_ADDRESS</code> or <code>SHIPPING_OPTION</code>.</p>
      <p>See <a href="https://developers.google.com/pay/api/web/reference/client#onPaymentDataChanged">payment data changed reference</a> for more information.</p>
    </td>
  </tr>
  <tr>
    <td>
      <p>readyToPayChangeCallback</p>
    </td>
    <td>
      <p>Invoked when the user's <code>isReadyToPay</code> state changes. This callback can be used to change the application's behaviour based on whether or not the user is ready to pay.</p>
      <p>Also raised as event <code>"readytopaychange"</code>.</p>
    </td>
  </tr>
  <tr>
    <td>
      <p>loadPaymentDataCallback</p>
    </td>
    <td>
      <p>Invoked when a user has successfully nominated payment details. This callback receives the <a href="https://developers.google.com/pay/api/web/reference/response-objects#PaymentData"><code>PaymentData</code> response</a> which includes the <a href="https://developers.google.com/pay/api/web/reference/response-objects#PaymentMethodData"><code>PaymentMethodData</code></a> that can be sent to <a href="https://developers.google.com/pay/api#participating-processors">supported payment processors</a>.</p>
      <p>Also raised as event <code>"loadpaymentdata"</code>.</p>
    </td>
  </tr>
</table>

## About this package

Note that this folder does not contain a `package.json` file. The `package.json` file is generated during the build
process using the `package-template.json` where the version number is read from the [`package.json`](../../package.json)
file defined in the root of this repository.