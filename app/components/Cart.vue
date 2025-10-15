<template>
	<div class="overlay" :class="{ 'show': viewCart }" id="modal-cart">
		<div class="modal">
			<header class="modal-header">
				<h2 class="modal-title">Cart</h2>
				<button class="modal-close" @click="closeCart()">x</button>
			</header>
			<table class="cart-table">
				<colgroup>
					<col class="col-goods">
					<col class="col-price">
					<col class="col-minus">
					<col class="col-qty">
					<col class="col-plus">
					<col class="col-total-price">
					<col class="col-delete">
				</colgroup>
				<thead>
					<tr :class="{ 'hidden': isEmpty}">
						<th>Good(s)</th>
						<th>Price</th>
						<th colspan="3">Qty.</th>
						<th colspan="2">Total</th>
					</tr>
				</thead>
				<tbody class="cart-table__goods">
					<tr class="cart-item" v-for="item in cart" :key="item.id" :class="{ 'hidden': isEmpty}">
						<td>{{ item.name }}</td>
						<td>{{ item.price }}$</td>
						<td><button class="cart-btn-minus" @click="decreaseItemCount(item)" :disabled="item.count <= 1">-</button></td>
						<td>{{ item.count }}</td>
						<td><button class=" cart-btn-plus" @click="increaseItemCount(item)">+</button></td>
						<td>{{ item.price * item.count }}$</td>
						<td><button class="cart-btn-delete" @click="removeCartItem(item)">x</button></td>
					</tr>
					<div :class="{ 'hidden': !isEmpty}">
						Корзина пуста
					</div>
				</tbody>
				<tfoot :class="{ 'hidden': isEmpty}">
					<tr >
						<th colspan=" 5">Total:</th>
						<th class="card-table__total" colspan="2">{{total}}$</th>
					</tr>

				</tfoot>
			</table>
			<form class="modal-form" action="" :class="{ 'hidden': isEmpty}">
				<input class="modal-input" type="text" placeholder="Имя" name="nameCustomer">
				<input class="modal-input" type="text" placeholder="Телефон" name="phoneCustomer">
				<button class="button cart-buy" type="submit">
					<span class="button-text">Checkout</span>
				</button>
			</form>
		</div>
	</div>
</template>

<script setup lang="ts">
import type { CartItem } from '~/models/cart-item.model';

const viewCart = useViewCart()
const cart = useCart()
const total = computed(() => cart.value.reduce((sum, item) => {
	return sum + (item.price * item.count)
}, 0))
let isEmpty = computed(() => cart.value.length === 0);;
const isCartEmpty = computed(() => console.log(cart))


const closeCart = () => { viewCart.value = false };

const increaseItemCount = (item: CartItem) => {
	const findItem = cart.value.find(c => c.id === item.id)
	if (findItem) {
		findItem.count ++
	}
}
const decreaseItemCount = (item: CartItem) => {
		const findItem = cart.value.find(c => c.id === item.id)
	if (findItem) {
		findItem.count --
	}
}
const removeCartItem = (item: CartItem) => {
		const findItem = cart.value.find(c => c.id === item.id)
	if (findItem) {
		cart.value = cart.value.filter(c=> c.id !== item.id)
	}
}
</script>