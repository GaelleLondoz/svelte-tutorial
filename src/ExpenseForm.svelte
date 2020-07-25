<script>
  //   import { onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";

  //   onMount(() => {});
  import Title from "./Title.svelte";
  export let name = "";
  export let amount = null;
  export let addExpense;
  export let isEditing;
  export let editExpense;
  export let hideForm;

  $: isEmpty = !name || !amount;

  let handleSubmit = () => {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }

    name = "";
    amount = null;
    hideForm();
  };
</script>

<section class="form">
  <Title title="Add expense" />
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}
    <button
      type="submit"
      class="btn btn-block"
      class:disabled={isEmpty}
      disabled={isEmpty}>
      {#if isEditing}Edit expense{:else}Add expense{/if}
    </button>
    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
