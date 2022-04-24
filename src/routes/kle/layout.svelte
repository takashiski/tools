<script>
  let rawtext = JSON.stringify([
    [
      "0,0",
      "0,1",
      "0,2",
      "0,3",
      "0,4",
      "0,5",
      "0,6",
      { x: 2.25 },
      "5,0",
      "5,1",
      "5,2",
      "5,3",
      "5,4",
      "5,5",
      "5,6",
      "5,7",
      "6,7",
    ],
    [
      { w: 1.5 },
      "1,0",
      "1,1",
      "1,2",
      "1,3",
      "1,4",
      "1,5",
      "1,6",
      { x: 2.25 },
      "6,0",
      "6,1",
      "6,2",
      "6,3",
      "6,4",
      "6,5",
      "6,6",
      { x: 0.25, w: 1.25, h: 2, w2: 1.5, h2: 1, x2: -0.25 },
      "7,7",
    ],
    [
      { w: 1.75 },
      "2,0",
      "2,1",
      "2,2",
      "2,3",
      "2,4",
      "2,5",
      "2,6",
      { x: 2.25 },
      "7,0",
      "7,1",
      "7,2",
      "7,3",
      "7,4",
      "7,5",
      "7,6",
    ],
    [
      { w: 2.25 },
      "3,0",
      "3,1",
      "3,2",
      "3,3",
      "3,4",
      "3,5",
      { x: 2.25 },
      "8,0",
      "8,1",
      "8,2",
      "8,3",
      "8,4",
      "8,5",
      "8,6",
      { w: 1.75 },
      "8,7",
    ],
    [
      { w: 1.25 },
      "4,0",
      "4,1",
      { w: 1.25 },
      "4,2",
      "4,3",
      "4,4",
      "4,5",
      "4,6",
      { x: 1.75 },
      "9,0",
      "9,1",
      "9,2",
      { w: 1.25 },
      "9,3",
      { w: 1.25 },
      "9,4",
      { w: 1.25 },
      "9,5",
      "9,6",
      { w: 1.25 },
      "9,7",
    ],
  ])
  // $: raw = JSON.stringify(`[${rawtext}]`).slice(0, -1).slice(1);
  $: result = ((raw) => {
    if(raw==""){
      return "上のテキストエリアになんか入力して"
    }
    const origin = JSON.parse(rawtext);
    const index = { x: 0, y: 0, w: 0, h: 0, w2: 0, h2: 0, x2: 0, y2: 0 };
    const layout = [index];
    layout.pop();
    for (const line of origin) {
      index.x = 0;
      for (const obj of line) {
        if (typeof obj == "string") {
          const cell = {
            matrix: JSON.parse(`[${obj}]`),
            x: index.x,
            y: index.y,
            w: index.w,
            h: index.h,
            w2: index.w2,
            h2: index.h2,
            x2: index.x2,
            y2: index.y2,
          };
          index.w != 0 ? (index.x += index.w - 1) : (index.x += 1);
          index.w != 0 ? (index.w = 0) : delete cell.w;
          index.h != 0 ? (index.h = 0) : delete cell.h;
          index.h2 != 0 ? (index.h2 = 0) : delete cell.h2;
          index.w2 != 0 ? (index.w2 = 0) : delete cell.w2;
          index.x2 != 0 ? (index.x2 = 0) : delete cell.x2;
          index.y2 != 0 ? (index.y2 = 0) : delete cell.y2;
          layout.push(cell);
        } else if (typeof obj == "object") {
          if (obj.x) {
            index.x += obj.x;
          }
          if (obj.y) {
            index.y += obj.y;
          }
          if (obj.w) {
            index.w = obj.w;
            // index.x = obj.w-1;
          }
          if (obj.h) {
            index.h = obj.h;
            console.log(obj);
          }
        }
      }
      index.y += 1;
    }
    // console.log(layout);
    return JSON.stringify(layout);
  })(rawtext);
  function copy(){
    navigator.clipboard.writeText(result);
  }
</script>

<h1>KLE raw to LAYOUT</h1>

<p class="lead">KLE.jsonからinfo.jsonのlayout作る</p>

<p><textarea name="" id="" cols="60" rows="10" bind:value={rawtext} /></p>
<p><textarea name="" id="result" cols="60" rows="10" bind:value={result} /></p>

<p>
  <button on:click={copy}>COPY</button>
</p>