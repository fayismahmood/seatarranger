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

          localStorage.setItem("stds",JSON.stringify(_))
        }
      };
    }
  };
</script>

<div class="p-8">
  <button
    on:click={() => {
      _.click();
    }}
    class="bg-slate-600 text-white px-2 py-0.5 rounded">Upload CSV</button
  >
</div>
