<script lang="ts">
  import ChecIcon from "../assets/ChekIcon.svg";
  import jwt_decode from "jwt-decode";
  import { Button, Textfield } from "svelte-mui";

  const decoded = localStorage.getItem("token")
    ? jwt_decode(localStorage.getItem("token"))
    : null;

  let username: string = "";
  let password: string = "";
  let repeat_password: string = "";
  let email: string = "";

  let err_message: string = "";
  let error: boolean = false;

  function HandleRegister(e) {
    fetch("https://extension-b.onrender.com/register", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        username: username,
        password: password,
        repeat_password: repeat_password,
        email: email,
      }),
    })
      .then((response) => response.json())
      .then((data) => {
        if (data.status == 201) {
          localStorage.setItem("token", data.token);
          location.reload();
        } else {
          err_message = data.message;
          error = true;
          e.target.reset();
        }
      });
  }

  function HandleLogOut() {
    localStorage.removeItem("token");
    location.reload();
  }
</script>

<div class="box">
  {#if decoded}
    <div class="succses__card">
      <img src={ChecIcon} alt="" />
      <h4 class="card__title">Succsefuly</h4>
      <Button raised color="primary" on:click={HandleLogOut}>Log Out</Button>
    </div>
  {/if}

  {#if !decoded}
    {#if error == true}
      <span class="error">{err_message}</span>
    {/if}
    <h1 class="title">Sign Up</h1>
    <form class="formik" on:submit|preventDefault={HandleRegister}>
        <Textfield
          outlined
          id="username"
          bind:value={username}
          type="text"
          placeholder="Your Username"
        />

        <Textfield
          outlined
          id="password"
          bind:value={password}
          type="password"
          placeholder="Your Password"
        />

        <Textfield
          outlined
          id="repeat_password"
          bind:value={repeat_password}
          type="password"
          placeholder="Repeat Password"
        />

      <Textfield
        outlined
        placeholder="Your Email"
        id="email"
        type="email"
        bind:value={email}
      />

      <Button raised color="primary">Sign Up</Button>
    </form>
  {/if}
</div>

<style>
  .box {
    padding: 20px;
    width: 300px;
    min-height: 300px;
    border-radius: 32px;
    background-color: #ffffff;
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  .error {
    color: red;
  }

  .title {
    font-size: 40px;
    margin-top: 10px;
    margin-bottom: 20px;
    color: #000000;
  }

  .succses__card {
    margin-top: 50px;
    display: flex;
    flex-direction: column;
    gap: 30px;
  }

  .card__title {
    margin-top: 15px;
    color: #2ecc71;
    font-weight: bold;
  }
</style>
