<script lang="ts">
  let stds = JSON.parse(localStorage.getItem("stds") || "[]");
  let rooms = JSON.parse(localStorage.getItem("rooms") || "[]");

  let Arrangements: any[] = JSON.parse(localStorage.getItem("arr") || "[]");
  let classes: any[] = JSON.parse(localStorage.getItem("class") || "[{}]")?.map(
    (e: any) => e.no
  );
let hided:{[key:string]:any}={}
  function shuffleArray(array: any[]) {
    for (var i = array.length - 1; i > 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      var temp = array[i];
      array[i] = array[j];
      array[j] = temp;
    }
  }
  let ClassWithRanked: any = {};

  $: classes.forEach((e) => {
    let _stds = stds.filter((_e: any) => _e.class == e);
    shuffleArray(_stds);
    ClassWithRanked[e] = _stds;
  });
  // $:console.log(ClassWithRanked);
</script>

<div class="flex">
  <div class="printArea p-6 flex flex-col shadow-lg border justify-center">
    {#each rooms as _rm}
      <div class="p-4 m-auto border relative">
        <div
          class="absolute -left-12 top-1/2 -rotate-90 text-xl text-center font-bold bg-green-600 text-white px-2"
        >
          {_rm.name}
        </div>
        <div>
          <div class="flex">
            <div
              style={`margin-left:2px;width:${64 * _rm.div - 4}px;`}
              class=" border-b border-indigo-900 px-4 text-md font-bold bg-slate-700 text-white rounded-sm"
            >
              Door Side
            </div>
            <div
              style={`margin-left:40px;width:${64 * (_rm.col - _rm.div)}px;`}
              class=" border-b border-indigo-900 px-4 text-md bg-slate-700 text-white font-bold rounded-sm"
            >
              Window Side
            </div>
          </div>
          <div>
            {#each Array(_rm.row) as _, _rowi}
              <div class="flex">
                {#each Array(_rm.col) as _, _coli}
                  {@const arr = Arrangements?.find(
                    (e) => e.col == _coli && e.row == _rowi && e.room == _rm.id
                  )}
                  <div
                    style={`width: 60px;margin: 2px; ${
                      _rm.div == _coli + 1 ? "margin-right:40px;" : ""
                    }`}
                    class={` bg-indigo-500   text-lg font-bold text-white rounded-sm   h-8 flex `}
                  >
                    {#if arr && (!hided[arr.cls])}
                      <div class="m-auto">
                        {ClassWithRanked[arr.cls]?.[
                          Arrangements.filter(
                            (e) => e.cls == arr.cls
                          ).findIndex(
                            (e) =>
                              e.col == _coli &&
                              e.row == _rowi &&
                              e.room == _rm.id
                          )
                        ].adno}
                      </div>
                    {/if}
                  </div>
                {/each}
              </div>
            {/each}
          </div>
        </div>
      </div>
    {/each}
  </div>
  <div class=" mx-5 shadow h-max w-full np">
    <div class="px-3 font-bold text-slate-600 border-b">Settings</div>

    <div class="mt-2 p-3">
      <div class="font-bold text-gray-500">Hide Class</div>
      <div class="p-3">
        {#each classes as e,i}
          <div class="py-1 border-b">
            <label class="relative inline-flex items-center cursor-pointer">
              <input type="checkbox" value="" class="sr-only peer" bind:checked={hided[e]} />
              <div
                class="w-11 h-6 bg-gray-200 rounded-full peer peer-focus:ring-4 peer-focus:ring-slate-300 dark:peer-focus:ring-slate-800 dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-0.5 after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-slate-600"
              />
              <span
                class="ml-3 text-sm font-medium text-gray-900 dark:text-gray-300"
                >Class {e}</span
              >
            </label>
          </div>
        {/each}
      </div>
    </div>
  </div>
</div>

<style lang="scss">
  .printArea {
    width: 210mm;
    height: 297mm;
    place-content: baseline;
  }

  @media print
        {
            @page {
                margin-top: 0;
                margin-bottom: 0;
            }
           .np {
               display: none;
            }
            
        }
</style>
