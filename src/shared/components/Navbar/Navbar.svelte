<script lang="ts">
  import { Link } from "svelte-routing";
  import {
    Button,
    Collapse,
    Container,
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
    Nav,
    Navbar,
    NavbarBrand,
    NavbarToggler,
    NavItem,
    NavLink,
  } from "sveltestrap";

  export let color: string = "text-white";
  export let btnTheme: string = "btn-outline-light";

  let isOpen: boolean = true;

  function handleUpdate(event: any) {
    isOpen = event.detail.isOpen;
  }
  $: loggedIn = sessionStorage.getItem("loggedIn") === "1";
  $: first_name = sessionStorage.getItem("first_name");
  $: last_name = sessionStorage.getItem("last_name");

  const onLogout = () => {
    sessionStorage.removeItem("loggedIn");
    sessionStorage.removeItem("first_name");
    sessionStorage.removeItem("last_name");
    sessionStorage.removeItem("di");
    window.location.href = "/";
  };
</script>

<Container>
  <Navbar light expand="lg">
    <NavbarBrand>
      <Link to="/">
        <img src="/assets/logo.png" alt="Logo" />
      </Link>
    </NavbarBrand>

    <Nav class="ms-auto" navbar>
      <NavLink>
        <NavItem>
          <Dropdown>
            <DropdownToggle
              class="btn bg-transparent border border-secondary"
              style="border-radius:30pt;display: flex;justify-content: space-between;align-items: center;padding: 4pt;"
            >
              <span class="me-2 ms-2 text-dark">Menu</span>
              <img
                src="/assets/static/images/global/no_avatar.png"
                alt=""
                style="border-radius: 15pt;"
                class="border border-secondary"
                height="30pt"
                width="30pt"
              />
            </DropdownToggle>
            <DropdownMenu
              class="shadow mt-2"
              style="border-radius:10pt;position: absolute;z-index: 2500;"
            >
              {#if !loggedIn}
                <Link
                  style="text-decoration: none;color: inherit;"
                  to="/register"
                >
                  <DropdownItem>Sign up</DropdownItem>
                </Link>
                <Link style="text-decoration: none;color: inherit;" to="/login">
                  <DropdownItem>Log In</DropdownItem>
                </Link>
                <div class="border-top mt-1 mb-1" />
                <Link style="text-decoration: none;color: inherit;" to="/login">
                  <DropdownItem>List on DMT</DropdownItem>
                </Link>
              {:else}
                <Link
                  to="/dashboard/listings"
                  style="text-decoration:none;color:inherit"
                >
                  <DropdownItem>List on DMT</DropdownItem>
                </Link>
              {/if}
              <Link style="text-decoration: none;color: inherit;" to="/faqs">
                <DropdownItem>FAQs</DropdownItem>
              </Link>
              {#if loggedIn}
                <DropdownItem on:click={onLogout}>
                  <span class="text-danger">Log out</span>
                </DropdownItem>
              {/if}
            </DropdownMenu>
          </Dropdown>
        </NavItem>
      </NavLink>
    </Nav>
  </Navbar>
</Container>

{#if btnTheme === ""}
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
  </style>
{/if}
