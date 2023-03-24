<script>
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import LoginForm from "../../shared/Forms/LoginForm.svelte";
</script>

<div>
  <Navbar color="text-black" btnTheme={""} />
</div>
<div
  class="d-flex align-items-center justify-content-center"
  style="min-height: 89vh;"
>
  <LoginForm />
</div>
