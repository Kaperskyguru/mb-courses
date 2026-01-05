<template>
  <div className="space-y-2">
    <Button
      variant="outline"
      className="w-full"
      @click="openCheckout('pri_01kc68wvad2nfdd5a7s5zphemt', {})"
    >
      Click here to proceed with payment
    </Button>
  </div>
</template>
<script setup>
import { initializePaddle } from '@paddle/paddle-js'

definePageMeta({
  layout: 'payment',
})

const paddle = ref(null)
const config = useRuntimeConfig()

function isDev() {
  return ['dev', 'development', 'staging'].includes(process.env.NODE_ENV)
}

onMounted(() => {
  const PADDLE_ENVIRONMENT = isDev() ? 'sandbox' : 'production'

  initializePaddle({
    token: config.public.PADDLE_TOKEN,
    eventCallback: function (data) {
      switch (data.name) {
        case 'checkout.loaded':
          console.log('Checkout loaded', data)
          break
        case 'checkout.closed':
          console.log('Checkout closed')
          break
        case 'checkout.completed':
          const c_data = data?.custom_data
          // Track payment (GA or Google)
          break
      }
    },
    environment: PADDLE_ENVIRONMENT,
  }).then((paddleInstance) => {
    if (paddleInstance) {
      paddle.value = paddleInstance
    }
  })
})

// Callback to open a checkout
const openCheckout = (priceId, data) => {
  console.log('Opening checkout for priceId:', priceId)
  paddle.value?.Checkout.open({
    settings: {
      allowedPaymentMethods: [
        'alipay',
        'apple_pay',
        'bancontact',
        'card',
        'google_pay',
        'ideal',
        'paypal',
      ],
      theme: 'dark',
    },
    discountCode: 'AIEARLYBIRD',
    items: [{ priceId }],
    customData: data,
  })
}

openCheckout('pri_01kc68wvad2nfdd5a7s5zphemt', {})
</script>