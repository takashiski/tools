<script>
  let row=10;
  let col=8;
  $:result=JSON.stringify((()=>{
    let s = [];
    for(let r=0;r<row;r+=1){
      let ss = [];
      for(let c=0;c<col;c+=1){
        ss.push(`${r},${c}`);
      }
      s.push(ss);
    }
    return s;
  })()).slice(0,-1).slice(1);

  let textarea;

  function copy(){
    navigator.clipboard.writeText(result);
  }
</script>



<h1>KLE Matrix</h1>
<p class="lead">KLEのひな形作成</p>

<p>row: <input type="number" name="" id="row" step="1" bind:value={row}></p>

<p>col: <input type="number" name="" id="col" step="1" bind:value={col}></p>

<p><textarea name="" id="result" cols="30" rows="10" value={result}></textarea></p>
<p><button on:click={copy}>COPY</button></p>