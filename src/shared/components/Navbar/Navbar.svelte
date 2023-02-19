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
    <!-- <NavbarToggler
      on:click={() => {
        isOpen = !isOpen;
      }}
    > -->
    <!-- <DropdownToggle
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
      </DropdownToggle> -->
    <!-- </NavbarToggler> -->

    <!-- <DropdownToggle
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
    </DropdownToggle> -->

    <!-- <Collapse
      style="transition: 0s;"
      {isOpen}
      navbar
      expand="lg"
      on:update={handleUpdate}
    > -->
    <Nav class="ms-auto" navbar>
      <!-- <NavItem>
          <NavLink>
            <Button class="bg-transparent {color} border-0 fw-bold">
              <Link style="text-decoration: none;color: inherit;" to="/faqs">
                FAQs
              </Link>
            </Button>
          </NavLink>
        </NavItem>
        {#if !loggedIn}
          <NavItem>
            <NavLink>
              <Button class="bg-transparent {color} border-0 fw-bold">
                <Link
                  style="text-decoration: none;color: inherit;"
                  to="/register">Sign up</Link
                >
              </Button>
            </NavLink>
          </NavItem>
          <NavItem>
            <NavLink>
              <Button class="bg-transparent {color} border-0 fw-bold">
                <Link style="text-decoration: none;color: inherit;" to="/login">
                  Login
                </Link>
              </Button>
            </NavLink>
          </NavItem>
        {:else}
          <NavItem>
            <Dropdown>
              <NavLink>
                <DropdownToggle nav>
                  <div class="d-flex align-items-center">
                    <img
                      src="/assets/static/images/global/no_avatar.png"
                      alt="avator"
                      height="25pt"
                      class="me-2"
                    />
                    <b class={color}>{first_name} {last_name}</b>
                  </div>
                </DropdownToggle>
              </NavLink>
              <DropdownMenu style="z-index: 2001;">
                <DropdownItem>Profile</DropdownItem>
                <DropdownItem>My Trips</DropdownItem>
                <DropdownItem>
                  <Link
                    to="/dashboard/listings"
                    style="text-decoration:none;color:inherit"
                    >Manage Listings</Link
                  >
                </DropdownItem>
                <DropdownItem divider />
                <DropdownItem on:click={onLogout} class="text-danger"
                  >Logout</DropdownItem
                >
              </DropdownMenu>
            </Dropdown>
          </NavItem>
        {/if} -->
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
            <DropdownMenu class="shadow mt-2" style="border-radius:10pt">
              {#if !loggedIn}
                <Link
                  style="text-decoration: none;color: inherit;"
                  to="/register"
                >
                  <DropdownItem>Sign up</DropdownItem>
                </Link>
                <Link style="text-decoration: none;color: inherit;" to="/login">
                  <DropdownItem>Login</DropdownItem>
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
    <!-- </Collapse> -->
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
