<script>
  import { Link, navigate } from "svelte-routing";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import config from "../../../environment.json";
  export let location;
  let error = undefined;
  let responseMessage = "";
  import {
    Button,
    Input,
    Modal,
    ModalBody,
    ModalFooter,
    ModalHeader,
  } from "sveltestrap";
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

  let showPassword = false;

  function onLogin() {
    sessionStorage.setItem("loggedIn", "123abc");

    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    var raw = JSON.stringify({
      ...signupObject,
    });

    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw,
    };

    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/user/login`, requestOptions)
      .then((response) => response.text())
      .then((result) => {
        let object = JSON.parse(result);
        if (object.loggedIn) {
          sessionStorage.setItem("loggedIn", "1");
          sessionStorage.setItem("first_name", object.first_name);
          sessionStorage.setItem("last_name", object.last_name);
          sessionStorage.setItem("di", object.id);
          navigate("/");
        } else {
          toggle();
        }
      })
      .catch((error) => error);
  }
  let isOpen = false;
  const toggle = () => (isOpen = !isOpen);

  let forgotPassword = false;
  let forgotPasswordLinkSent = false;
  let forgotPasswordCode = "";
  let forgotPasswordCodeVerified = false;
  let NewPassword = "";
  let verifiedNewPassword = "";
</script>

<Modal body header="Invalid credentials" {isOpen} {toggle}>
  Sorry, you've entered incorrect email or password, You can try again.
</Modal>
<div class="fixed-top">
  <Navbar color="text-black" btnTheme={""} />
</div>
{#if !forgotPassword}
  <div
    class="d-flex align-items-center justify-content-center"
    style="min-height: 99vh;"
  >
    <div class="container shadow " style="background-color: #F9F8FC;">
      <div class="row">
        <div
          class="col-md-6 d-flex justify-content-center flex-column p-2 p-sm-5 align-self-center pt-5"
          style="min-height: 50vh;"
        >
          <div class="text-center">
            <h3>Login</h3>
          </div>
          <input
            type="text"
            class="form-control mb-3"
            placeholder="Email"
            bind:value={signupObject.email}
          />
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
          <div class="d-flex align-items-center">
            <Input
              type="switch"
              color="success"
              on:change={(e) => {
                showPassword = !showPassword;
              }}
              class={"me-2"}
            />
            <label for="showPass">
              {showPassword ? "Hide Password" : "Show Password"}
            </label>
          </div>
          <div class="text-end">
            <button
              class="btn-link text-primary bg-transparent border-0"
              style="cursor: pointer;"
              on:click={() => (forgotPassword = true)}
            >
              Forgot Password
            </button>
          </div>
          <br />
          <div class="mt-1">
            <div class="row">
              <div class="col-md-6 mb-2">
                {#if signupObject.email === "" || signupObject.password === ""}
                  <button
                    disabled
                    class="btn col-12 ps-5 pe-5"
                    on:click={onLogin}
                  >
                    Login
                  </button>
                {:else}
                  <button class="btn col-12 ps-5 pe-5" on:click={onLogin}>
                    Login
                  </button>
                {/if}
              </div>
              <div class="col-md-6 mb-2">
                <Link
                  to="/register"
                  style="text-decoration:none;color:inherit;"
                >
                  <button class="btn col-12 ps-5 pe-5"> Sign up </button>
                </Link>
              </div>
            </div>
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
{/if}
<Modal centered backdrop="static" isOpen={forgotPassword}>
  <ModalHeader>Forgot Password</ModalHeader>
  <ModalBody>
    {#if !forgotPasswordLinkSent}
      <div class="row me-1 mb-3">
        <div class="col-9">
          <input
            type="email"
            placeholder="Enter your email"
            class="form-control col-12"
            bind:value={signupObject.email}
          />
        </div>

        <button
          class="btn col-3"
          on:click={() => {
            var myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");
            var raw = JSON.stringify(signupObject);
            var requestOptions = {
              method: "POST",
              headers: myHeaders,
              body: raw,
            };
            fetch(
              `${config.SERVER_IP}${config.SERVER_PORT}/user/signupr`,
              requestOptions
            )
              .then((response) => response.json())
              .then((result) => {
                responseMessage = result.message;
                if (result.error) {
                  error = true;
                } else {
                  error = undefined;
                  forgotPasswordLinkSent = true;
                }
              })
              .catch((error) => error);
          }}
        >
          Send Code
        </button>
      </div>
    {:else if !forgotPasswordCodeVerified && signupObject.email !== ""}
      <div class="row me-1 mb-3">
        <div class="mb-3">{`Enter code sent to ${signupObject.email}`}</div>
        <div class="col-9">
          <input
            type="text"
            placeholder={`Verification Code`}
            class="form-control col-12"
            bind:value={forgotPasswordCode}
          />
        </div>
        <button
          class="btn col-3"
          on:click={() => {
            var myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");

            var raw = JSON.stringify({
              email: signupObject.email,
              code: forgotPasswordCode,
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
                  forgotPasswordCodeVerified = true;
                }
              })
              .catch((error) => error);
          }}
        >
          Verify
        </button>
      </div>
    {:else}
      <div class="row me-1 mb-3">
        <div class="col-12">
          <input
            type="text"
            placeholder={`New Password for ${signupObject.email}`}
            class="form-control col-12"
            bind:value={NewPassword}
          />
        </div>
      </div>
      <div class="row me-1 mb-3">
        <div class="col-12">
          <input
            type="text"
            placeholder={`Re-enter new password`}
            class="form-control col-12"
            bind:value={verifiedNewPassword}
          />
        </div>
      </div>
    {/if}
  </ModalBody>
  <ModalFooter>
    <Button on:click={() => (forgotPassword = false)}>Close</Button>
    {#if forgotPasswordCodeVerified}
      {#if NewPassword === verifiedNewPassword}
        <Button
          on:click={() => {
            var myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");

            var raw = JSON.stringify({
              email: signupObject.email,
              code: forgotPasswordCode,
              password: NewPassword,
            });

            var requestOptions = {
              method: "POST",
              headers: myHeaders,
              body: raw,
            };

            fetch(
              `${config.SERVER_IP}${config.SERVER_PORT}/user/up`,
              requestOptions
            )
              .then((response) => response.json())
              .then((result) => {
                responseMessage = result.message;
                if (result.error) {
                  error = true;
                } else {
                  error = false;
                  forgotPassword = false;
                  forgotPasswordLinkSent = false;
                  forgotPasswordCode = "";
                  forgotPasswordCodeVerified = false;
                  NewPassword = "";
                  verifiedNewPassword = "";
                  signupObject.email == "";
                }
              })
              .catch((error) => {
                forgotPassword = false;
                forgotPasswordLinkSent = false;
                forgotPasswordCode = "";
                forgotPasswordCodeVerified = false;
                NewPassword = "";
                verifiedNewPassword = "";
                signupObject.email == "";
              });
          }}
        >
          Update Password
        </Button>
      {:else}
        <Button disabled>Password Unmatched</Button>
      {/if}
    {/if}
  </ModalFooter>
</Modal>

<Modal isOpen={error === false}>
  <ModalHeader class="row">
    <div class="row col-12">
      <span class="col-11">Update Password</span>
      <button
        class="col-1 btn-close btn"
        on:click={() => {
          error = undefined;
        }}
      />
    </div>
  </ModalHeader>
  <ModalBody>Password Updated Successfully</ModalBody>
</Modal>

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
    width: 100%;
    height: 250px;
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
</style>
