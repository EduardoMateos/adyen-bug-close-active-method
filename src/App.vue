<template>
  <div id="app">
    <div>Adyen dropin:</div>
    <div ref="dropin"></div>
    <button @click="variableRandom.closeActivePaymentMethod()">
      Close active methods
    </button>
  </div>
</template>

<script>
import AdyenCheckout from "@adyen/adyen-web";
import "@adyen/adyen-web/dist/adyen.css";
export default {
  name: "App",
  data() {
    return {
      variableRandom: null,
    }
  },
  async created() {
    const json = {
      paymentData: {
        paymentMethods: [
          {
            brands: ["visa", "mc", "amex"],
            name: "Tarjeta de Crédito",
            type: "scheme",
          },
          {
            configuration: { merchantId: "UKPK5VNTUX6G6", intent: "authorize" },
            name: "PayPal",
            type: "paypal",
          },
          {
            configuration: {
              merchantId: "50",
              gatewayMerchantId: "TESTECOM",
            },
            name: "Google Pay",
            type: "paywithgoogle",
          },
        ]
      },
      apiKeyFront: "YOUR CLIENT KEY",
    };
    const adyenConfig = {
      paymentMethodsResponse: json.paymentData,
      clientKey: "YOUR CLIENT KEY",
      showPayButton: true,
      locale: "es-ES",
      environment: "test",
      onSubmit: (state, dropin) => {
        // submit
      },
      onAdditionalDetails: async (state, dropin) => {
        // onAdditionalDetails
      },
      paymentMethodsConfiguration: {
        card: {
          hasHolderName: true,
          holderNameRequired: true,
          enableStoreDetails: true,
          hideCVC: false,
        },
      },
    };

    const adyenCheckout = await AdyenCheckout(adyenConfig);
    this.variableRandom = adyenCheckout.create("dropin").mount(this.$refs.dropin);
  },
};
</script>
