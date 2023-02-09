<script lang="ts">
  import { Route, Router } from "svelte-routing";
  import Contact from "../../../pages/Contact/Contact.svelte";
  import Dashboard from "../../../pages/Dashboard/Dashboard.svelte";
  import FaQs from "../../../pages/FAQs/FAQs.svelte";
  import Home from "../../../pages/Home/Home.svelte";
  import ListingCheckout from "../../../pages/Listings/ListingCheckout.svelte";
  import Listings from "../../../pages/Listings/Listings.svelte";
  import Login from "../../../pages/Login/Login.svelte";
  import Register from "../../../pages/Register/Register.svelte";
  export let loginToken: string = "";
</script>

<div>
  <Router>
    <Route path="/" component={Home} />
    <Route path="/login" component={Login} />
    <Route path="/register" component={Register} />
    <Route path="/listing" component={Listings} />
    <Route path="/listing/:id" component={ListingCheckout} />
    <Route path="/contact" component={Contact} />
    <Route path="/dashboard" component={Dashboard} />
    <Route path="/dashboard/:subpage" component={Dashboard} />
    <Route path="/faqs" component={FaQs} />
  </Router>
</div>
