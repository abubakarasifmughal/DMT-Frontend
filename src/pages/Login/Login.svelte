<script>
  import { Link, navigate } from "svelte-routing";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import config from "../../../environment.json";
  import { Modal } from "sveltestrap";
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

    fetch(
      `${config.SERVER_IP}:${config.SERVER_PORT}/user/login`,
      requestOptions
    )
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
      .catch((error) => console.log("error", error));
  }
  let isOpen = false;
  const toggle = () => (isOpen = !isOpen);
</script>

<Modal body header="Invalid credentials" {isOpen} {toggle}>
  Sorry, you've entered incorrect email or password, You can try again.
</Modal>
<Navbar color="text-black" btnTheme={""} />
<div
  class="d-flex align-items-center justify-content-center"
  style="height: 90vh;"
>
  <div class="container shadow" style="background-color: #F9F8FC;">
    <div class="row p-5">
      <div
        class="col-md-6 d-flex justify-content-center flex-column p-5"
        style="height: 50vh;"
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
        <input
          type="password"
          class="form-control mb-3"
          placeholder="Password"
          bind:value={signupObject.password}
        />
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
              <Link to="/register" style="text-decoration:none;color:inherit;">
                <button class="btn col-12 ps-5 pe-5"> Sign up </button>
              </Link>
            </div>
          </div>
        </div>
      </div>
      <div
        class="col-md-6 text-center d-flex align-items-center justify-content-center flex-column p-5 border"
      >
        <div
          class="image"
          style="background-image: url(/assets/static/images/Register.png);"
        />
        <div class="p-5">
          <h3><b>Adventure Travel Simplifed.</b></h3>
          <span
            >Browse and book tours and activities so incredible, you’ll want to
            tell your friends.</span
          >
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
</style>
