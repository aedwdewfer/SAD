<script setup>
import axios from 'axios'
import { ref, computed, inject } from 'vue'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { cart, closeDrawer } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true

    const { data } = await axios.post(`https://27dc5fc0a7ef15c4.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value,
      nameInput: nameInput.value,
      emailInput: emailInput.value,
      addressInput: addressInput.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const nameInput = ref('')
const emailInput = ref('')
const addressInput = ref(String('Kotelniki'))
const buttonDisabled = computed(
  () =>
    isCreating.value ||
    cartIsEmpty.value ||
    !nameInput.value ||
    !emailInput.value ||
    !addressInput.value
)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8" style="overflow: auto">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы один товар, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} успешно оформлен. `"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>

        <input
          type="text"
          class="py-2 px-3 border rounded-md outline-none"
          v-model="nameInput"
          placeholder="Введите имя"
        />
        <input
          type="text"
          class="py-2 px-3 border rounded-md outline-none"
          v-model="emailInput"
          placeholder="Введите номер телефона или почту"
        />
        <!-- <input
          type="text"
          class="address-input"
          v-model="addressInput"
          placeholder="Выберите адрес получения заказа"
        /> -->
        <select v-model="addressInput" class="py-2 px-3 border rounded-md outline-none">
          <option value="Kotelniki">Метро Котельники 7 вход</option>
        </select>

        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
