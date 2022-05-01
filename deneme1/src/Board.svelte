<svelte:head>

<script type="module" src="https://unpkg.com/chessboard-element/bundled/chessboard-element.bundled.js"></script>
</svelte:head>
<script>
	import { onMount } from 'svelte';
  import { Chess } from 'chess.js';


let xxx ="bir"



	onMount( () => {

	fetch(`./aa1.pgn`).then((response)=>{
	return response.text()
	}).then(function(text) {
xxx= text
//console.log(xxx)
    });

const board = document.querySelector('chess-board');
const statusElement = document.querySelector('#status');
const fenElement = document.querySelector('#fen');
const pgnElement = document.querySelector('#pgn');

	const game = new Chess();
		board.addEventListener('drag-start', (e) => {
		  const {source, piece, position, orientation} = e.detail;

		  // do not pick up pieces if the game is over
		  if (game.game_over()) {
		    e.preventDefault();
		    return;
		  }


		  // only pick up pieces for the side to move
		  if ((game.turn() === 'w' && piece.search(/^b/) !== -1) ||
		      (game.turn() === 'b' && piece.search(/^w/) !== -1)) {
		    e.preventDefault();
		    return;
		  }
		});

		board.addEventListener('drop', (e) => {
		  const {source, target, setAction} = e.detail;

		  // see if the move is legal
		  const move = game.move({
		    from: source,
		    to: target,
		    promotion: 'q' // NOTE: always promote to a queen for example simplicity
		  });

		  // illegal move
		  if (move === null) {
		    setAction('snapback');
		  }

		  updateStatus();
		});

		// update the board position after the piece snap
		// for castling, en passant, pawn promotion
		board.addEventListener('snap-end', (e) => {
		  board.setPosition(game.fen());
		});

		function updateStatus () {
		  let status = '';

		  let moveColor = 'White';
		  if (game.turn() === 'b') {
		    moveColor = 'Black';
		  }

		  if (game.in_checkmate()) {
		    // checkmate?
		    status = `Game over, ${moveColor} is in checkmate.`;
		  } else if (game.in_draw()) {
		    // draw?
		    status = 'Game over, drawn position';
		  } else {
		    // game still on
		    status = `${moveColor} to move`;

		    // check?
		    if (game.in_check()) {
		      status += `, ${moveColor} is in check`;
		    }
		  }

		  statusElement.innerHTML = status;
		  fenElement.innerHTML = game.fen();
		  pgnElement.innerHTML = game.pgn();
		}

		updateStatus();
	});
const hamleler=(a)=> a
</script>

<main>

	<h1>Hello {name}!</h1>
	<chess-board
	    style="width: 400px"
	    position="start"
	    draggable-pieces>
	</chess-board>

	<label>Status:</label>
	<div id="status"></div>
	<label>FEN:</label>
	<div id="fen"></div>
	<label>PGN:</label>
	<div id="pgn"></div>
	<p id="demo">Atom</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
