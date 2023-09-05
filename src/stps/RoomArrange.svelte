<script lang="ts">
  import ClassPick from "../lib/ClassPick.svelte";

  let stds = JSON.parse(localStorage.getItem("stds") || "[]");
  let rooms = JSON.parse(localStorage.getItem("rooms") || "[]");

  let Arrangements: any[] = JSON.parse(localStorage.getItem("arr") || "[]");
  let classes = [1, 2, 3, 4];
  let AvailableInClasses: any[] = [];

  $: {
    AvailableInClasses = classes.map((cls) => {
      let numofStd = stds.filter((e: any) => e.class == cls).length;
      let placed = Arrangements.filter((e: any) => e.cls == cls).length;
      //console.log(numofStd,placed,"e");

      return {
        avail: numofStd - placed,
        cls: cls,
      };
    });
  }
  // $:console.log(AvailableInClasses, "eeeeeeeeeeee");
</script>

<div>
  <div class="flex gap-3 px-5 bg-white shadow-lg py-2 sticky z-20 left-0 top-0">
    <button
      on:click={() => {
        localStorage.setItem("arr", JSON.stringify(Arrangements));
      }}
      class="bg-slate-600 text-white px-2 py-0.5 rounded"
    >
      Save
    </button>
  </div>
<div class="p-5">

    {#each rooms as _rm}
      <div >
        <div class="text-lg font-bold px-2 my-3">
          {_rm.name}
        </div>
        <div>
          <table>
            {#each Array(_rm.row) as _, _rowi}
              <tr>
                {#each Array(_rm.col) as _, _coli}
                  {@const arr = Arrangements?.find(
                    (e) => e.col == _coli && e.row == _rowi && e.room == _rm.id
                  )}
                  <td
                    class={`border p-1 min-w-[80px] ${
                      _rm.div == _coli ? "mr-5" : ""
                    }`}
                  >
                    {#if arr}
                      <div
                        class="relative w-full h-10 bg-indigo-100 flex rounded-lg"
                      >
                        <div class="m-auto text-lg font-bold text-indigo-700">
                          {arr.cls}
                        </div>
                        <button
                          on:click={() => {
                            Arrangements = Arrangements?.filter(
                              (e) =>
                                !(
                                  e.col == _coli &&
                                  e.row == _rowi &&
                                  e.room == _rm.id
                                )
                            );
                          }}
                          class="absolute right-0 h-5 w-5 flex rounded-full bg-red-700 text-white"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            class="h-4 w-4 m-auto"
                            viewBox="0 0 24 24"
                            ><path
                              fill="currentColor"
                              d="M6.4 19L5 17.6l5.6-5.6L5 6.4L6.4 5l5.6 5.6L17.6 5L19 6.4L13.4 12l5.6 5.6l-1.4 1.4l-5.6-5.6L6.4 19Z"
                            /></svg
                          >
                        </button>
                      </div>
                    {:else}
                      <div>
                        <ClassPick
                          opts={AvailableInClasses.filter((e) => e.avail > 0)}
                          on:change={({ detail }) => {
                            Arrangements.push({
                              col: _coli,
                              row: _rowi,
                              cls: detail,
                              room: _rm.id,
                            });
                            Arrangements = Arrangements;
                          }}
                        />
                      </div>
                    {/if}
                  </td>
                {/each}
              </tr>
            {/each}
          </table>
        </div>
      </div>
    {/each}
</div>
</div>
