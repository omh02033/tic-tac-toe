<script lang="ts">
  let board: Array<number> = [];
  let winBoards: Array<number> = [];
  for (let i = 0; i < 9; i++) {
    board[i] = 0;
  }
  let turn = 1;

  const check = (p: number) => {
    const way = [1, 3, 4, 2];
    for (let dw of way) {
      checkroot: for (let start = -dw * 2; start <= 0; start += dw) {
        let pre = -1;
        for (let i = p + start; i < p + start + 3 * dw; start += dw) {
          if (i < 0 || i >= 9) continue checkroot;
          if (board[i] !== turn) continue checkroot;
          if (pre !== -1) {
            if (Math.abs(Math.floor(i / 3) - Math.floor(pre / 3)) >= 2)
              continue checkroot;
            if (Math.abs((i % 3) - (pre % 3)) >= 2) continue checkroot;
          }
          pre = i;
        }
        return { turn, arr: [0, 1, 2].map(v => p + start + v * dw) }
      }
    }
    return null;
  };
  const setBoard = (p: number) => {
    if (!board[p]) {
      board[p] = turn;
      console.log('sss');
      const checkRes = check(p);
      if (checkRes) {
        alert(`${checkRes.turn === 1 ? 'Player' : 'AI'} Win!`);
        winBoards = checkRes.arr;
        return;
      }
      turn *= -1;
    }
  };
</script>

<div id="container">
  <div id="board">
    {#each board as pst, idx}
      <button on:click={() => setBoard(idx)} class={winBoards.indexOf(idx) !== -1 ? 'win' : ''}>
        {pst === 1 ? "O" : pst === -1 ? "X" : ""}
      </button>
    {/each}
  </div>
</div>

<style>
  #container {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  #board {
    background-color: antiquewhite;
    width: 300px;
    height: 300px;
    display: flex;
    flex-wrap: wrap;
  }
  button {
    box-sizing: border-box;
    width: 100px;
    height: 100px;
    border: 1px solid black;
    background: none;
    padding: 0;
    margin: 0;
    text-align: center;
    font-size: 40px;
    font-weight: 700;
  }
  button.win {
    color: red;
  }
</style>
