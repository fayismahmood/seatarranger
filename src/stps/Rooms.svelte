<script lang="ts">
  let _room = localStorage.getItem("rooms");
  //let stds = JSON.parse(_std);
  let rooms: any[] = JSON.parse(_room || "[]");
  import { nanoid } from "nanoid";
  import { flip } from "svelte/animate";
  $: localStorage.setItem("rooms", JSON.stringify(rooms));
</script>

<div>
  <!-- <div class="flex gap-3 px-5 bg-white shadow-lg py-2">
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
    
  }} class="bg-slate-600 hover:bg-slate-500 text-white px-2 py-0.5 rounded">
    Save
  </button>  
  </div> -->
  <div class="m-10">
    <div class="mb-10 flex w-full sticky top-0 left-0 pt-5 bg-white bg-opacity-60 backdrop-blur-md">
      <div>
        <div class=" text-2xl text-slate-500 font-bold">Rooms</div>
        <div class="mb-10 text-slate-400">Add, Remove, Edit Rooms</div>
      </div>
      <div class="ml-auto my-auot h-max">
        <button
        on:click={()=>{
          rooms=[...rooms,{}]
        }}
        class="bg-slate-600 text-white text-lg px-3 py-1 rounded-lg shadow-lg shadow-slate-400 font-bold hover:bg-slate-500 ">
           New Room
        </button>
      </div>
    </div>
    {#each rooms as e, i (i)}
      <div animate:flip={{}} class="px-5 py-2 flex justify-center gap-10 border-b">
        <div class="inp">
          <label for="">Room Name</label>
          <input bind:value={rooms[i].name} type="text" />
        </div>
        <div class="inp">
          <label for="">row</label>
          <input class="min" bind:value={rooms[i].row} type="number" />
        </div>
        <div class="inp">
          <label for="">colums</label>
          <input class="min" bind:value={rooms[i].col} type="number" />
        </div>
        <div class="inp">
          <label for="">divider after</label>
          <input class="min" bind:value={rooms[i].div} type="number" />
        </div>
        <button
        on:click={()=>{
          rooms=rooms.filter((_,_i)=>(_i!=i))
        }}
          class="p-1 my-auto h-8 w-8 text-red-700 bg-red-100 rounded-lg hover:text-red-600"
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
      &.min {
        width: 75px;
      }
      &:focus {
        border: 2px solid rgb(31, 48, 80);
      }
    }
  }
</style>
