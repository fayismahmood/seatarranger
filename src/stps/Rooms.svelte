<script lang="ts">
  let _room = localStorage.getItem("rooms");
  //let stds = JSON.parse(_std);
  let rooms: any[] = JSON.parse(_room||"[]");
  import { nanoid } from 'nanoid'

</script>

<div>
  <div class="flex gap-3 px-5 bg-white shadow-lg py-2">
    <button
    on:click={() => {
      rooms.push({ row: 0, col: 0, name: "",id:nanoid() });
      rooms=rooms
    }}
    class="bg-slate-600 hover:bg-slate-500 text-white px-2 py-0.5 rounded"
  >
    Add
  </button>

  <button on:click={()=>{
    localStorage.setItem("rooms",JSON.stringify(rooms))
  }} class="bg-slate-600 hover:bg-slate-500 text-white px-2 py-0.5 rounded">
    Save
  </button>
  </div>
  <div class="flex flex-wrap">

    {#each rooms as e, i}
    <div class="p-5">
         
      <div class="inp">
        <label for="">Room Name</label>
        <input bind:value={rooms[i].name} type="text" />
      </div>
      <div class="inp">
        <label for="">row</label>
        <input bind:value={rooms[i].row} type="number" />
      </div>
      <div class="inp">
        <label for="">colums</label>
        <input bind:value={rooms[i].col} type="number" />
      </div>
      <div class="inp">
        <label for="">divider after</label>
        <input bind:value={rooms[i].div} type="number" />
      </div>
    </div>
  {/each}
  </div>
</div>



<style lang="scss">
    .inp{
        label{
            font-size: small;
            display: block;
        }
        input{
            border: 2px solid rgb(178, 175, 175);
            padding: 5px 10px;
            outline: none;
            border-radius: 5px;
&:focus{
    border: 2px solid rgb(31, 48, 80);

}
        }
    }
</style>