<script lang="ts">
  import ChecIcon from "../assets/ChekIcon.svg";
  import jwt_decode from "jwt-decode";

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
          e.target.reset()
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
      <button class="formik__btn" on:click={HandleLogOut}>Log out</button>
    </div>
  {/if}

  {#if !decoded}
    {#if error == true}
      <span class="error">{err_message}</span>
    {/if}
    <h1 class="title">Sign Up</h1>
    <form class="formik" on:submit|preventDefault={HandleRegister}>
      <div>
        <label class="formik_label" for="username">Username</label>
        <input
          class="formik__input"
          id="username"
          bind:value={username}
          name="username"
          type="text"
          placeholder="Your Username"
        />
      </div>

      <div>
        <label class="formik_label" for="password">Password</label>
        <input
          class="formik__input"
          id="password"
          bind:value={password}
          name="password"
          type="password"
          placeholder="Your Password"
        />
      </div>

      <div>
        <label class="formik_label" for="password">Repeat Password</label>
        <input
          class="formik__input"
          id="repeat_password"
          bind:value={repeat_password}
          name="password"
          type="password"
          placeholder="Repeat Password"
        />
      </div>

      <div>
        <label class="formik_label" for="email">Your Email</label>
        <input
          class="formik__input"
          id="email"
          bind:value={email}
          name="email"
          type="email"
          placeholder="Your Email"
        />
      </div>

      <button class="formik__btn">Sign Up</button>
    </form>
  {/if}
</div>

<style>
  .box {
    padding: 20px;
    width: 400px;
    min-height: 350px;
    border-radius: 32px;
    background-color: #ffffff;
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  .error {
    color: red;
  }

  .formik {
    display: flex;
    flex-direction: column;
    gap: 25px;
    width: 70%;
  }

  .formik div {
    display: flex;
    flex-direction: column;
    align-items: start;
    gap: 10px;
  }

  .formik_label {
    font-weight: 600;
    font-size: 16px;
    line-height: 19px;
    text-transform: capitalize;
    font-feature-settings: "pnum" on, "lnum" on;
    color: #151435;
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

  .formik__input {
    display: block;
    min-height: 30px;
    border: 1px solid #cccccc;
    border-radius: 4px;
    outline: none;
    font-weight: 500;
    font-size: 16px;
    padding-left: 10px;
    width: 95%;
  }

  .formik__btn {
    border: 1px solid #cccccc;
    background-color: #151435 !important;
    border-radius: 6px !important;
    color: #ffffff !important;
  }

  .formik__btn:hover {
    background-color: #ffffff !important;
    border: 1px solid #000000;
    color: #000000 !important;
  }

  @media screen and (max-width: 470px) {
    .box {
      min-width: 270px;
      min-height: 350px;
    }
  }
</style>
