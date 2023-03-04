<script>
  export let onClickContinue;
</script>

<div class="mt-3">
  <div class="row">
    <div class="col-md-10">
      <h2>Greetings! from Digital Marketplace for Tourism branded as DMT</h2>
      <span>
        We welcome you to list your Hotels and Experience offering based in
        Africa and Asia to the world
      </span>
      <div>May we all prosper</div>
      <br />
      <button on:click={onClickContinue} class="mt-3 btn pe-5 ps-5">
        Lets get Started
      </button>
    </div>
  </div>
</div>

<style>
  input[type="radio"] {
    transform: scale(1.3);
    accent-color: #9427f7;
  }
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
