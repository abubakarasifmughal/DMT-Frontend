<script>
  import { createEventDispatcher } from "svelte";
  import config from "../../../environment.json";
  let dispatch = createEventDispatcher();

  export let title = "property";
  export let amenitiesList = [];
  export let selectedAmeneties = [];
  export let admin;

  $: isPresent = (id) => selectedAmeneties.findIndex((a) => a === id) !== -1;
</script>

<div class="col-md-12">
  <hr />
  <br />
  {#each amenitiesList as filter}
    <div class="mb-5">
      <div class="my-2">
        {#if admin}
          <button
            class="btn btn-close mb-2"
            on:click={async () => {
              console.log(filter.id);
              let headersList = {
                Accept: "*/*",
                "User-Agent": "Thunder Client (https://www.thunderclient.com)",
                "Content-Type": "application/json",
              };

              let bodyContent = JSON.stringify({
                id: filter.id,
              });

              let response = await fetch(
                `${config.SERVER_IP}${config.SERVER_PORT}/facilities`,
                {
                  method: "DELETE",
                  body: bodyContent,
                  headers: headersList,
                }
              );
              let data = await response.text();
              console.log(data);

              dispatch("update", {
                detail: "update",
              });
            }}
          />
        {/if}
        <span style="font-size: 20px;">{filter.label}</span>
      </div>
      {#each filter.facilityItems as option}
        <div>
          <div class="mt-2 btn-group col-12">
            <button
              disabled
              class="btn btn-light border col-12 text-start"
              style={(isPresent(option.id)
                ? "background-color: #9427F7;color: white;padding-left:10%;"
                : "") + "transition:0.5s"}
            >
              {option.name}
            </button>
            {#if admin}
              <button
                class="btn btn-outline-danger"
                on:click={async () => {
                  console.log(option.id);
                  let headersList = {
                    Accept: "*/*",
                    "User-Agent":
                      "Thunder Client (https://www.thunderclient.com)",
                    "Content-Type": "application/json",
                  };

                  let bodyContent = JSON.stringify({
                    id: option.id,
                  });

                  let response = await fetch(
                    `${config.SERVER_IP}${config.SERVER_PORT}/facilities-item`,
                    {
                      method: "DELETE",
                      body: bodyContent,
                      headers: headersList,
                    }
                  );
                  let data = await response.text();
                  console.log(data);

                  dispatch("update", {
                    detail: "update",
                  });
                }}
              >
                Remove
              </button>
            {/if}
          </div>
        </div>
      {/each}
      <hr />
    </div>
  {/each}

  <br />
  <br />
</div>

<style>
</style>
