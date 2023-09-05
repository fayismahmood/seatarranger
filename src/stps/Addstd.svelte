<script>


  let _ = document.createElement("input");
  _.type = "file";

  _.onchange = () => {
    let fl = _.files?.[0];
    if (fl) {
      const reader = new FileReader();
      let data = reader.readAsText(fl);
      reader.onload = () => {
        console.log(reader.result);
        if (typeof reader.result == "string") {
          let _ = reader.result?.split("\n").map((e) => {
            let __ = e.split(",");
            return { adno: __[0], name: __[1], class: __[2] };
          });

          localStorage.setItem("stds", JSON.stringify(_));
        }
      };
    }
  };

  let classes = JSON.parse(localStorage.getItem("class") || "[]");
</script>

<div class="flex">
  <div class="p-8">
    <div class="text-lg text-gray-500 mb-2">
      Upload Students' adno and class as a csv file.
    </div>
    <button
      on:click={() => {
        _.click();
      }}
      class="bg-slate-600 text-white px-2 py-0.5 rounded">Upload CSV</button
    >
  </div>
  <div class="ml-16 w-full">
    <div class="px-4 py-2 flex gap-3 shadow-lg w-full bg-white">
      <button
        on:click={() => {
          classes.push({ no: null, col: "#ffff" });
          classes = classes;
        }}
        class="px-2 py-0.5 bg-slate-600 text-white rounded">Add</button
      >
      <button on:click={()=>{
        localStorage.setItem("class",JSON.stringify(classes))
      }} class="px-2 py-0.5 bg-slate-600 text-white rounded">Save</button>
    </div>
    <div class="w-full flex flex-wrap">
      {#each classes as e, i}
        <div class="m-2 shadow-lg p-3 rounded">
          <div class="inp">
            <label>Class</label>
            <input bind:value={classes[i].no} type="number" />
          </div>
          <div class="inp">
            <label>color</label>
            <input
              bind:value={classes[i].col}
              style="padding: none;"
              type="color"
            />
          </div>
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
    input {
      border: 2px solid rgb(178, 175, 175);
      padding: 5px 10px;
      outline: none;
      border-radius: 5px;
      &:focus {
        border: 2px solid rgb(31, 48, 80);
      }
    }
  }
</style>
