<script>
  // import { getContext } from "svelte";
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";

  export let expenses = [];
  // export let expenses = getContext("data");
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <SectionTitle title="expense list" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div
        in:fly={{ x: 200, delay: (index + 1) * 700 }}
        out:fly={{ x: -200 }}
        animate:flip>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>No expenses added to the list</h2>
    {/each}
  </ul>
</section>
