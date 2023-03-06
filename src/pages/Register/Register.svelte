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
  let open = false;
  let emailExists=false
  function toggle() {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    var raw = JSON.stringify({"email":signupObject.email});
    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw,
    };
    console.log
    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/user/generate-token`, requestOptions)
      .then((response) => response.json())
      .then((result) => {
        if(result.msg=='success'){
        open = !open
        emailExists=false
        }
        else{

          emailExists=true
        }
        console.log(result)
      })
      .catch((error) => console.log("error", error));    
  }

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
let otpMatch=true
  function onSignup() {


    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    var raw = JSON.stringify(signupObject);
    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw
    };
    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/user`, requestOptions)
      .then((response) => response.json())
      .then((result) => {
        console.log(result)
        if(result.statusCode!=405){
        //navigate("/login");
        open=false
        
        }
        else{
          open=true
          otpMatch=false
        }
      })
      .catch((error) => console.log("error", error));
  }

  let showPassword = false;
</script>

<div class="">
  <Navbar color="text-black" btnTheme={""} />
  <div
    class="d-flex align-items-center justify-content-center mt-0 pt-0 pt-sm-5 mt-sm-5"
  >
    <div class="container shadow p-sm-5" style="background-color: #F9F8FC;">
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
            style="border-color: {emailExists
              ? 'red'
              : ''};"
          />
          <div>
            {#if emailExists}
            <div class="mb-1 ps-2 text-danger">Email already exists</div>
            {/if}
          </div>
          <div>
            <Modal isOpen={open} {toggle}>
              <ModalHeader {toggle}>Email Verification</ModalHeader>

              <ModalBody>
                <Label
                  >Enter The One Time Password (OTP) sent to you via email</Label
                >
                <input
                  type="text"
                  class="form-control mb-3"
                  placeholder="OTP"
                  bind:value={signupObject.otp}
                />
                {#if !otpMatch}
                <div class="mb-1 ps-2 text-danger">OTP did'nt match</div>
                {/if}
              </ModalBody>

              <ModalFooter>
                <Button color="primary" on:click={onSignup}>Verify</Button>
              </ModalFooter>
            </Modal>
          </div>

          <!-- <Input
            
            type="switch"
            class = "mb-3"
            label="Is Individual?"
            bind:checked={signupObject.is_individual}
            on:change={changePlaceHolder}
          /> -->

          <!-- <input
            type="text"
            class="form-control mb-3"
            {placeholder}A
            bind:value={signupObject.user_number}
          />
          <input
            type="text"
            class="form-control mb-3"
            placeholder="Tax Number"
            bind:value={signupObject.tax_number}
          /> -->

          <input
            type="password"
            class="form-control mb-3"
            placeholder="Password"
            bind:value={signupObject.password}
          />
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
                <button class="btn ps-5 pe-5 col-12" on:click={toggle}>
                  Sign up
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
  .switch-input{
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
