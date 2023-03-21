<script>
  export let onClickContinue;
</script>

<div class="mt-3">
  <div class="row">
    <div class="col-md-10">
      <h2>Greetings from Digital Marketplace for Tourism Branded as DMT</h2>
      <br />
      <span>
        We welcome all the individuals and businesses to our tourism marketplace
        platform. Here, you can list Accommodation property and Experience
        offering based in Africa and Asia. Focused on tourism niche and regional
        specific services, the sellers will be able to take great advantage
        among buyers around the world. We wish everyone who is connected with
        DMT for the prosperity of this community and help boost business
        operations around the region.
      </span>
      <div>May we all prosper</div>
      <br />
      <button on:click={onClickContinue} class="mt-3 btn pe-5 ps-5">
        Let's get Started
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
