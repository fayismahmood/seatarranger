<script lang="ts">
  import { flip } from "svelte/animate";

  let _ = document.createElement("input");
  _.type = "file";

  let classes: any[] = JSON.parse(localStorage.getItem("class") || "[]");
  let _stds: any[] = JSON.parse(localStorage.getItem("stds") || "[]");
  $: localStorage.setItem("class", JSON.stringify(classes));

  _.onchange = () => {
    let fl = _.files?.[0];
    if (fl) {
      const reader = new FileReader();
      let data = reader.readAsText(fl);
      reader.onload = () => {
        console.log(reader.result);
        if (typeof reader.result == "string") {
          let _ = reader.result?.split("\n").map((e) => {
            e = e.replaceAll("\r", "");
            console.log(e, "asdf");

            let __ = e.split(",");
            return { adno: __[0], name: __[1], class: __[2] };
          });

          _stds = _;

          localStorage.setItem("stds", JSON.stringify(_));
        }
      };
    }
  };
</script>

<div class="flex">
  <div class="p-8 shadow mx-6">
    <div class="text-lg text-gray-500 mb-2">
      Upload Students' adno and class as a csv file.
    </div>
    <button
      on:click={() => {
        _.click();
      }}
      class="bg-slate-600 text-white px-2 py-0.5 rounded">Upload CSV</button
    >
    <button
      on:click={() => {
        localStorage.removeItem("stds");
        _stds = [];
      }}
      class="bg-slate-600 text-white px-2 py-0.5 rounded">Clear</button
    >
    <div>
      <div class="mt-6 overflow-auto max-h-[65vh] w-max">
        <table>
          <thead>
            <tr class="text-left">
              <th class="px-3">Adno</th>
              <th class="px-3">Name</th>
              <th class="px-3">Class</th>
            </tr>
          </thead>
          {#each _stds as e, i}
            <tr class="border-b">
              <td class="px-3">
                {e.adno}
              </td>
              <td class="px-3">
                {e.name}
              </td>
              <td class="px-3">
                {e.class}
              </td>
              <td class="px-3">
                <button
                  on:click={() => {
                    _stds = _stds.filter((_) => _.adno != e.adno);
                    localStorage.setItem("stds", JSON.stringify(_stds));
                  }}
                  class="p-0.5 my-auto h-5 w-5 text-red-700 bg-red-100 rounded-lg hover:text-red-600"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="100%"
                    height="100%"
                    viewBox="0 0 24 24"
                    ><path
                      fill="currentColor"
                      d="M7 21q-.825 0-1.413-.588T5 19V6H4V4h5V3h6v1h5v2h-1v13q0 .825-.588 1.413T17 21H7ZM17 6H7v13h10V6ZM9 17h2V8H9v9Zm4 0h2V8h-2v9ZM7 6v13V6Z"
                    /></svg
                  >
                </button>
              </td>
            </tr>
          {/each}
        </table>
      </div>
    </div>
  </div>
  <div class="ml-16 w-full max-w-[400px] shadow">
    <div class="px-4 py-2 flex gap-3 border-b w-full bg-white">
      <div class="text-lg font-bold text-slate-600">Classes</div>

      <button
        on:click={() => {
          classes.push({ no: null, col: "#ffff" });
          classes = classes;
        }}
        class="ml-auto px-2 py-0.5 hover:bg-slate-500 bg-slate-600 text-white rounded"
        >Add</button
      >
    </div>
    <div class=" w-full flex flex-wrap max-h-[75vh] overflow-auto">
      {#each classes as e, i (i)}
        <div animate:flip={{}} class=" shadow px-5 py-3 flex gap-5 w-full">
          <div class="inp">
            <label>Class</label>
            <input bind:value={classes[i].no} type="text" />
          </div>
          <div class="inp">
            <label>color</label>
            <input
              bind:value={classes[i].col}
              style="padding: none;"
              type="color"
            />
          </div>
          <button
            on:click={() => {
              classes = classes.filter((_, _i) => _i !== i);
            }}
            class="p-1 text-red-500 rounded-lg m-auto hover:text-red-900 bg-red-200 h-5 w-5"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="100%"
              height="100%"
              viewBox="0 0 24 24"
              ><path
                fill="currentColor"
                d="M7 21q-.825 0-1.413-.588T5 19V6H4V4h5V3h6v1h5v2h-1v13q0 .825-.588 1.413T17 21H7ZM17 6H7v13h10V6ZM9 17h2V8H9v9Zm4 0h2V8h-2v9ZM7 6v13V6Z"
              /></svg
            >
          </button>
        </div>
      {/each}
    </div>
  </div>
</div>

<style lang="scss">
  .inp {
    label {
      font-size: small;
      display: block;
    }
    input[type="text"] {
      border: 2px solid rgb(178, 175, 175);
      padding: 2px 4px;
      outline: none;
      border-radius: 5px;
      width: 100px;
      &:focus {
        border: 2px solid rgb(31, 48, 80);
      }
    }
  }
</style>
