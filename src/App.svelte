<script>
  // register, restore, login
  let formType = "login";

  let confirmPasswordRef;
  let formRef;

  let formValues = {
    email: "",
    password: "",
    confirmPassword: "",
  };

  function validateForm() {
    if (formType === "register") {
      if (formValues.password !== formValues.confirmPassword) {
        confirmPasswordRef.setCustomValidity("Passwords should match");
      } else {
        confirmPasswordRef.setCustomValidity("");
      }
    }
  }

  function onConfirmPasswordChange() {
    confirmPasswordRef.setCustomValidity("");
  }

  function onSubmit(e) {
    e.preventDefault();

    validateForm();
    const isValid = formRef.checkValidity();

    if (isValid) {
      alert("Success!");
    } else {
      formRef.reportValidity();
    }
  }

  function handleLinkClick(e) {
    e.preventDefault();

    const linkHref = e.target.getAttribute("href");
    const newPage = linkHref.split("/")[1];

    formType = newPage;
  }
</script>

<style>
  :root {
    --base-color: #ffffffbb;
    --active-color: #ffffff;
  }

  .page {
    background: linear-gradient(90deg, #4b6cb7 0%, #182848 100%);
    min-height: 100vh;

    display: grid;
    place-items: center;
  }

  .pane {
    max-width: 100%;
    background: rgba(0, 0, 0, 0.5);
    padding: 4rem 4rem;
    border-radius: 30px;
    overflow: hidden;
    box-shadow: 0 0 15px 0px rgba(0, 0, 0, 0.7);

    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    align-items: center;
  }

  @media (max-width: 600px) {
    .pane {
      height: 100%;
      width: 100%;
      border-radius: 0;
      padding: 2rem 2rem;
    }
  }

  .input {
    background: none;
    border: 0;
    border-bottom: 3px solid var(--base-color);
    padding: 10px 0;
    font-size: 1rem;
    color: #fff;
    max-width: 100%;
    width: 300px;
    transition: all 300ms ease;
  }

  .input:focus {
    outline: 0;
    border-bottom-color: var(--active-color);
  }

  .input:focus::placeholder {
    color: #ffffff44;
  }

  .input::placeholder {
    color: var(--base-color);
  }

  .spacer {
    flex: 0 1 40px;
  }

  .btn-submit {
    font-size: 1.25rem;
    border: 2px solid var(--base-color);
    color: var(--base-color);
    border-radius: 10px;
    background: none;
    padding: 10px 30px;
    transition: all 300ms ease;
    cursor: pointer;
  }

  .btn-submit:hover,
  .btn-submit:focus {
    background: var(--active-color);
    color: rgba(2, 0, 36, 1);
    outline: 0;
  }

  .links {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }

  @media (max-width: 600px) {
    .links {
      flex-flow: column nowrap;
      align-items: center;
    }
    .link + .link {
      margin-top: 1rem;
    }
  }

  .link {
    font-family: Arial, Helvetica, sans-serif;
    white-space: nowrap;
    color: var(--base-color);
  }
</style>

<main class="page">
  <form class="pane" on:submit={onSubmit} bind:this={formRef}>
    <img
      class="logotype"
      src="logo.svg"
      width="100"
      height="100"
      alt=""
      role="presentation" />
    <div class="spacer" />
    <input
      class="input"
      required
      type="email"
      placeholder="Email"
      autocomplete="email"
      autocapitalize="off"
      aria-label="Email"
      bind:value={formValues.email} />
    <div class="spacer" />
    {#if formType === 'login'}
      <input
        class="input"
        required
        type="password"
        placeholder="Password"
        autocomplete="current-password"
        aria-label="Password"
        bind:value={formValues.password} />
      <div class="spacer" />
    {/if}
    {#if formType === 'register'}
      <input
        class="input"
        required
        type="password"
        placeholder="Password"
        autocomplete="new-password"
        aria-label="Password"
        bind:value={formValues.password} />
      <div class="spacer" />
      <input
        class="input"
        required
        type="password"
        placeholder="Confirm your password"
        autocomplete="new-password"
        aria-label="Password confirmation"
        on:input={onConfirmPasswordChange}
        bind:this={confirmPasswordRef}
        bind:value={formValues.confirmPassword} />
      <div class="spacer" />
    {/if}
    {#if formType === 'login'}
      <button class="btn-submit" type="submit"> Sign in </button>
    {/if}
    {#if formType === 'register'}
      <button class="btn-submit" type="submit"> Register </button>
    {/if}
    {#if formType === 'restore'}
      <button class="btn-submit" type="submit"> Send me a link </button>
    {/if}
    <div class="spacer" />
    <div class="links">
      {#if formType !== 'login'}
        <a class="link" on:click={handleLinkClick} href="/login">I have an
          account</a>
      {/if}
      {#if formType !== 'register'}
        <a class="link" on:click={handleLinkClick} href="/register">Create an
          account</a>
      {/if}
      {#if formType !== 'restore'}
        <a class="link" on:click={handleLinkClick} href="/restore">Forgot
          password?</a>
      {/if}
    </div>
  </form>
</main>
