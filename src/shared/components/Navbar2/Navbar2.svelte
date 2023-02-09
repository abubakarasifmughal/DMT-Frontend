<script lang="ts">
  import { Link } from "svelte-routing";
  import {
    Button,
    Collapse,
    Container,
    Nav,
    Navbar,
    NavbarBrand,
    NavbarToggler,
    NavItem,
    NavLink,
  } from "sveltestrap";

  export let color: string = "text-white";
  export let btnTheme: string = "btn-outline-light";

  let isOpen: boolean = false;

  function handleUpdate(event: any) {
    isOpen = event.detail.isOpen;
  }

  // -------------------
  export let loginToken: string = "";
  const onLogin = () => {
    loginToken = "123abc";
  };
  const onSignup = () => {
    loginToken = "123abc";
  };

  const onLogout = () => {
    loginToken = "";
  };
</script>

<Container fluid>
  <Navbar light expand="md">
    <NavbarBrand>
      <Link to="/">
        <img src="/assets/logo.png" alt="Logo" />
      </Link>
    </NavbarBrand>
    <NavbarToggler on:click={() => (isOpen = !isOpen)} />
    <Collapse {isOpen} navbar expand="lg" on:update={handleUpdate}>
      <Nav class="ms-auto" navbar>
        <NavItem>
          <NavLink>
            <Button on:click={onSignup} class="bg-transparent {color} border-0"
              >Help</Button
            >
          </NavLink>
        </NavItem>
      </Nav>
    </Collapse>
  </Navbar>
</Container>

<style>
  .btn {
    border: 1px solid #9427f7;
    background-color: #9427f7;
    color: white;
  }
  .btn:nth-child(n):hover {
    background-color: #6801c8;
    color: white;
  }

  .form-control {
    border-color: #9427f7;
  }
</style>
