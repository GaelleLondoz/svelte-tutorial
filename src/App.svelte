<script>
  // import GithubAwait from "./GithubAwait.svelte";
  // import Github from "./Github.svelte";

  import { setContext, onMount, afterUpdate } from "svelte";

  // const state = {
  //   name: "simple name",
  //   removeExpense
  // };
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";

  // data
  // import expensesData from "./expenses";

  //variables
  // let expenses = [...expensesData];
  let expenses = [];
  // set editing variables

  let setName = "";
  let setAmount = null;
  let setId = null;

  //toggle form variable
  let isFormOpen = false;
  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((accumulator, current) => {
    return (accumulator += current.amount);
  }, 0);

  // functions

  let showForm = () => {
    isFormOpen = true;
  };

  let hideForm = () => {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = null;
  };
  let removeExpense = id => {
    expenses = expenses.filter(expense => expense.id !== id);
  };

  let clearExpenses = () => {
    expenses = [];
  };

  let addExpense = ({ name, amount }) => {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  };

  let setModifiedExpense = id => {
    let expense = expenses.find(expense => expense.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  };
  let editExpense = ({ name, amount }) => {
    expenses = expenses.map(expense => {
      return expense.id === setId
        ? { ...expense, name, amount }
        : { ...expense };
    });
    setId = null;
    setAmount = null;
    setName = "";
  };

  // context
  // setContext("state", state);
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);

  // setContext("data", expenses);

  // local storage
  let setLocalStorage = () => {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  };

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<Navbar {showForm} />
<main class="content">
  <!-- <Github /> -->
  <!-- <GithubAwait /> -->
  <Totals title="Total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    Clear expenses
  </button>
</main>
{#if isFormOpen}
  <Modal>
    <ExpenseForm
      {addExpense}
      name={setName}
      amount={setAmount}
      {isEditing}
      {editExpense}
      {hideForm} />
  </Modal>
{/if}
