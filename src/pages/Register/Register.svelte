<script>
  import { Link, navigate } from "svelte-routing";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import PasswordStrength from "../../shared/lib/PasswordStrength/PasswordStrength.svelte";
  import config from "../../../environment.json";

  let signupObject = {
    first_name: "",
    last_name: "",
    email: "",
    password: "",
    telephone: "",
    address: "",
    is_seller: 1, //seller not buyer
    role: 0, //non admin
  };

  let verify_password = "";
  let tosAgree = false;

  function onSignup() {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    var raw = JSON.stringify(signupObject);
    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw,
    };
    fetch(`${config.SERVER_IP}:${config.SERVER_PORT}/user`, requestOptions)
      .then((response) => response.text())
      .then((result) => {
        navigate("/login");
      })
      .catch((error) => console.log("error", error));
  }
</script>

<div class="">
  <Navbar color="text-black" btnTheme={""} />
  <div
    class="d-flex align-items-center justify-content-center"
    style="height: 90vh;"
  >
    <div class="container shadow p-5" style="background-color: #F9F8FC;">
      <div class="row">
        <div class="col-md-6">
          <div class="text-center">
            <h3>Register</h3>
          </div>
          <input
            type="text"
            class="form-control mb-3"
            placeholder="First Name"
            bind:value={signupObject.first_name}
          />
          <input
            type="text"
            class="form-control mb-3"
            placeholder="Last Name"
            bind:value={signupObject.last_name}
          />
          <input
            type="text"
            class="form-control mb-3"
            placeholder="Phone Number"
            bind:value={signupObject.telephone}
          />
          <input
            type="text"
            class="form-control mb-3"
            placeholder="Email"
            bind:value={signupObject.email}
          />
          <input
            type="password"
            class="form-control mb-3"
            placeholder="Password"
            bind:value={signupObject.password}
          />
          {#if signupObject.password !== verify_password}
            <div class="mb-1 ps-2 text-danger">Password Must Match</div>
          {/if}
          <input
            type="password"
            class="form-control mb-3"
            placeholder="Verify Password"
            bind:value={verify_password}
            style="border-color: {signupObject.password !== verify_password
              ? 'red'
              : ''};"
          />
          <div class="mb-3">
            <b>Password Strength</b>
          </div>
          <PasswordStrength password={signupObject.password} />
          <div class="pt-4 d-flex align-items-center">
            <input
              class="me-2"
              style="transform: scale(1.2);"
              id="tos_register"
              type="checkbox"
              bind:checked={tosAgree}
            />
            <label for="tos_register"
              >By creating an account, you agree to our <b
                style="color: #9427f7;">Term and Conditions</b
              ></label
            >
          </div>
          <div class="mt-4 d-flex align-items-center">
            {#if tosAgree}
              <button class="btn ps-5 pe-5" on:click={onSignup}>
                Sign up
              </button>
            {:else}
              <button disabled class="btn ps-5 pe-5"> Sign up </button>
            {/if}
            <span class="ps-3 pe-3">|</span>
            <Link to="/login" style="text-decoration:none;color:inherit;">
              <button class="btn ps-5 pe-5"> Login </button>
            </Link>
          </div>
        </div>
        <div
          class="col-md-6 text-center d-flex align-items-center justify-content-center flex-column p-5"
        >
          <div
            class="image"
            style="background-image: url(/assets/static/images/Register.png);"
          />
          <div class="p-5">
            <h3><b>Adventure Travel Simplifed.</b></h3>
            <span
              >Browse and book tours and activities so incredible, you’ll want
              to tell your friends.</span
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .btn {
    border: 1px solid #9427f7;
    background-color: transparent;
    color: #700ad0;
  }
  .btn:nth-child(n):hover {
    background-color: #9427f7;
    color: white;
  }
  .image {
    height: 50%;
    width: 100%;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
  }
  span {
    background-color: transparent;
  }
  h3 {
    background-color: transparent;
  }
  input[type="checkbox"] {
    transform: scale(1.3);
    accent-color: #9427f7;
  }
</style>
