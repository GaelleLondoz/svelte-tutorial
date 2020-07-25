<script>
  let getUsers = async () => {
    let userData = await fetch("https://api.github.com/users");
    let githubUsers = await userData.json();
    return githubUsers;
  };
</script>

<style>
  h2 {
    text-align: center;
  }
</style>

<section>
  {#await getUsers()}
    <h2>Loading ...</h2>
  {:then users}
    {#each users as user}
      <article class="user">
        <img src={user.avatar_url} alt={user.login} />
        <div class="user-info">
          <h3>User: {user.login}</h3>
          <a href={user.html_url} target="_blank" class="btn-primary">
            Github url
          </a>
        </div>
      </article>
    {/each}
  {:catch error}
    <p>Something went wrong: {error.message}</p>
  {/await}
</section>
