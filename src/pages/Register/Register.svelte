<script>
  import { Link, navigate } from "svelte-routing";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import PasswordStrength from "../../shared/lib/PasswordStrength/PasswordStrength.svelte";
  import config from "../../../environment.json";
  import {
    Button,
    Modal,
    ModalBody,
    ModalFooter,
    ModalHeader,
    Input,
    Label,
  } from "sveltestrap";
  let error = undefined;
  let responseMessage = "";

  let signupObject = {
    first_name: "",
    last_name: "",
    email: "",
    password: "",
    telephone: "",
    address: "",
    is_seller: 1, //seller not buyer
    role: 0, //non admin
    is_individual: false,
    tax_number: 123,
    user_number: 123,
    otp: null,
  };

  let verify_password = "";
  let tosAgree = false;

  let placeholder = "User Identification Number";
  function changePlaceHolder() {
    if (!signupObject.is_individual) {
      placeholder = "Company Identification Number";
    } else placeholder = "User Identification Number";
  }

  function onSignup() {
    if (!activity) {
      activity = true;
      error = undefined;
      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");
      var raw = JSON.stringify(signupObject);
      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
      };
      fetch(
        `${config.SERVER_IP}${config.SERVER_PORT}/user/signupa`,
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => {
          // quvume@mailinator.com
          responseMessage = result.message;
          if (result.error) {
            error = true;
            activity = false;
          } else {
            error = false;
            activity = false;
          }
        })
        .catch((error) => error);
    }
  }

  let showPassword = false;

  let emailVerified = false;

  let enteredVerificationCode = "";

  let activity = false;
</script>

<Modal isOpen={error === true}>
  <ModalHeader>Resolve following</ModalHeader>
  <ModalBody>
    {responseMessage}
  </ModalBody>
  <ModalFooter>
    <Button
      on:click={() => {
        error = undefined;
      }}
    >
      Dismiss
    </Button>
  </ModalFooter>
</Modal>

<Modal isOpen={error === false}>
  <ModalHeader class="row">
    <div class="row col-12">
      <span class="col-11">Verify Email</span>
      <button
        class="col-1 btn-close btn"
        on:click={() => {
          error = undefined;
        }}
      />
    </div>
  </ModalHeader>

  <ModalBody>
    {responseMessage}
    <input
      bind:value={enteredVerificationCode}
      placeholder="Vefication Code"
      class="form-control mt-3"
    />
  </ModalBody>
  <ModalFooter>
    {#if !emailVerified}
      <Button on:click={onSignup}>Resend</Button>
      <Button
        on:click={() => {
          if (!activity) {
            activity = true;
            var myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");

            var raw = JSON.stringify({
              email: signupObject.email,
              code: enteredVerificationCode,
            });

            var requestOptions = {
              method: "POST",
              headers: myHeaders,
              body: raw,
            };

            fetch(
              `${config.SERVER_IP}${config.SERVER_PORT}/user/verify`,
              requestOptions
            )
              .then((response) => response.json())
              .then((result) => {
                responseMessage = result.message;
                if (result.error) {
                  error = true;
                } else {
                  error = false;
                  emailVerified = true;
                }
              })
              .catch((error) => error);
          }
        }}
      >
        {#if activity}
          <span class="spinner-border text-success spinner-border-sm" />
        {:else}
          Verify
        {/if}
      </Button>
    {:else}
      <Link
        class="col-md-6 mb-3"
        to="/login"
        style="text-decoration:none;color:inherit;"
      >
        <button class="btn ps-5 pe-5 col-12">Login Now</button>
      </Link>
    {/if}
  </ModalFooter>
</Modal>

<div class="">
  <Navbar color="text-black" btnTheme={""} />
  <div
    class="d-flex align-items-center justify-content-center"
    style="min-height: 89vh;"
  >
    <div class="container shadow p-5" style="background-color: #F9F8FC;">
      <div class="row">
        <div class="col-md-6">
          <div class="text-center">
            <h3>Register</h3>
            <br />
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
            type="email"
            class="form-control mb-3"
            placeholder="Email"
            bind:value={signupObject.email}
          />
          <div />
          {#if showPassword}
            <input
              type="text"
              class="form-control mb-3"
              placeholder="Password"
              bind:value={signupObject.password}
            />
          {:else}
            <input
              type="password"
              class="form-control mb-3"
              placeholder="Password"
              bind:value={signupObject.password}
            />
          {/if}
          {#if signupObject.password !== verify_password}
            <div class="mb-1 ps-2 text-danger">Password Must Match</div>
          {/if}
          {#if showPassword}
            <input
              type="text"
              class="form-control mb-3"
              placeholder="Verify Password"
              bind:value={verify_password}
              style="border-color: {signupObject.password !== verify_password
                ? 'red'
                : ''};"
            />
          {:else}
            <input
              type="password"
              class="form-control mb-3"
              placeholder="Verify Password"
              bind:value={verify_password}
              style="border-color: {signupObject.password !== verify_password
                ? 'red'
                : ''};"
            />
          {/if}
          <div class="d-flex align-items-center">
            <input
              id="showPass"
              style="transform: scale(1.3);"
              type="checkbox"
              bind:checked={showPassword}
              class="me-2"
            />
            <label for="showPass">
              {showPassword ? "Hide Password" : "Show Password"}
            </label>
          </div>
          <br />
          <div class="mb-3">
            <b>Password Strength</b>
          </div>

          <PasswordStrength password={signupObject.password} />
          <div class="pt-4 d-flex align-items-start">
            <input
              class="me-3 mt-1"
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

          <div class="mt-4 row">
            <div class="col-md-6 mb-3">
              {#if tosAgree}
                <button class="btn ps-5 pe-5 col-12" on:click={onSignup}>
                  {#if activity}
                    <span
                      class="spinner-border text-success spinner-border-sm"
                    />
                  {:else}
                    Sign up
                  {/if}
                </button>
              {:else}
                <button disabled class="btn ps-5 pe-5 col-12"> Sign up </button>
              {/if}
            </div>
            <Link
              class="col-md-6 mb-3"
              to="/login"
              style="text-decoration:none;color:inherit;"
            >
              <button class="btn ps-5 pe-5 col-12"> Login </button>
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
  .switch-input {
    margin-bottom: 20px;
  }
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
    width: 100%;
    height: 100%;
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
