<script lang="ts">
  import type { Cart, Items } from "./types"
  import { fixPrice } from "./utils.ts"

  type Props = {
    cart: Cart
  }

  const { cart }: Props = $props()

  const orderTotal = $derived.by(() => {
    const total = cart.items.reduce((acc, curr) => acc + (curr.price * curr.amount), 0)
    return fixPrice(total)
  })

  const getSum = (item) => {
    return fixPrice(item.price * item.amount)
  }
</script>

<div class="cart-card">
  <div class="cart-title">Your Cart ({cart.amount})</div>

  {#if cart.amount === 0}
    <div class="cart-content-empty">
      <img alt="Empty Cart Illustration" src="./assets/images/illustration-empty-cart.svg"/>
      <div class="cart-message">Your added items will appear here</div>
    </div>
  {/if}

  {#if cart.amount > 0}
    <div class="cart-content">

      <div class="cart-item-list">
        {#each cart.items as item}
          <div class="cart-item">
            <div class="item-left">
              <div class="item-name">{item.name}</div>

              <div class="item-details">
                <span class="item-amount">{item.amount}x</span>
                <span class="item-price"> $ {item.price} = </span>
                <span class="item-sum">$ {getSum(item)}</span>
              </div>
            </div>

            <button class="item-del">
              <img alt="" src="assets/images/icon-remove-item.svg"/>
            </button>
          </div>
        {/each}
      </div>

      <div class="cart-total">
        <span>Order total</span>
        <span class="cart-total-price">$ {orderTotal}</span>
      </div>

      <div class="cart-info">
        <img alt="" src="assets/images/icon-carbon-neutral.svg"/>
        <span>This is a <b>carbon-neutral</b> delivery</span>
      </div>

      <button class="cart-confirm">Confirm Order</button>
    </div>
  {/if}
</div>

<style>
.cart-card {
    background-color: var(--clr-rose-50);
    background-color: white;
    width: 386px;
    height: fit-content;
    margin: 0 auto;
    padding: 1.6rem;
    border-radius: 0.6rem;
}

.cart-title {
    color: var(--clr-red);
    font-weight: var(--fw-bolder);
    font-size: var(--fs-large);
    margin-bottom: 1.8rem;
}

/* Empty cart */
.cart-content-empty {
    padding-bottom: 1.275rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.6rem;
}

.cart-message {
    font-size: var(--fs-regular);
    color: var(--clr-rose-600);
}

/* Filled cart */
.cart-content {
}

.cart-item-list {
    display: flex;
    flex-direction: column;
}

.cart-item {
    font-size: var(--fs-small);
    padding-bottom: 1.2rem;
    border-bottom: 1px solid var(--clr-rose-100);
    margin-bottom: 1.0rem;

    display: flex;
    justify-content: space-between;
    align-items: center;
}

.item-name {
    font-weight: var(--fw-bold);
    margin-bottom: 0.6rem;
}

.item-details {
    display: flex;
    gap: 0.5rem;
}

.item-amount {
    color: var(--clr-red);
    font-weight: var(--fw-bold);
}

.item-price {
    color: var(--clr-rose-500);
}

.item-sum {
    color: var(--clr-rose-500);
    font-weight: var(--fw-bold);
}

.item-del {
    border: 2px solid var(--clr-rose-300);
    border-radius: 50%;
    background: none;
    height: 24px;
    width: 24px;
    padding: 0;
    cursor: pointer;

    display: flex;
    align-items: center;
    justify-content: center;
}

.item-del img {
    height: 12px;
    width: 12px;
}

.cart-total {
    font-size: var(--fs-small);
    margin-bottom: 2rem;

    display: flex;
    justify-content: space-between;
    align-items: center;
}

.cart-total-price {
    font-size: var(--fs-large);
    font-weight: var(--fw-bolder);
}

.cart-info {
    font-size: var(--fs-small);
    background-color: var(--clr-rose-50);
    padding: 0.875rem 0;
    border-radius: 0.4rem;
    margin-bottom: 1.6rem;

    display: flex;
    gap: 0.5rem;
    align-items: center;
    justify-content: center;
}

.cart-confirm {
    background-color: var(--clr-red);
    color: var(--clr-rose-100);
    border: 0;
    border-radius: 2rem;
    width: 100%;
    padding: 1.2rem 0;
    font-size: 1rem;
    font-weight: var(--fw-bold);
}
</style>
