<template>
	<h2>MineSweeper</h2>
	<!-- Game Settigns -->
	<label for="widthInput">Width:</label>
	<input id="widthInput" v-model="widthInput" type="number">
	<label for="heightInput">Height:</label>
	<input id="heightInput" v-model="heightInput" type="number">
	<label for="minesInput">Mines amount:</label>
	<input id="minesInput" v-model="minesAmountInput" type="number">
	<button @click="startNewGame">New game</button>

	<!-- Game -->
	<table style="margin-top: 10px">
		<tr v-for="(row,y) in height" :key="y">
			<td :class="{ 'shown': mineField[x][y].shown, 'hidden': !mineField[x][y].shown }" v-for="(col,x) in width" :key="x" @click="tileLeftClick(mineField[x][y], x, y)" @contextmenu.prevent="tileRightClick(mineField[x][y])">
				{{mineField[x][y].text}}
			</td> 
		</tr>
	</table>
</template>

<script>
const positionsAround = [
	{ x: -1, y: -1},
	{ x: -1, y: 0},
	{ x: -1, y: 1},
	{ x: 0, y: -1},
	{ x: 0, y: 1},
	{ x: 1, y: -1},
	{ x: 1, y: 0},
	{ x: 1, y: 1}
]

export default {
	name: 'MineSweeper',
	components: {
	},
	data() {
		return {
			widthInput: 10,
			heightInput: 10,
			minesAmountInput: 10,
			width: 10,
			height: 10,
			minesAmount: 10,
			mineField: [],
			gameDone: false,
		}
	},
	
	methods: {
		startNewGame() {
			if(this.widthInput > 0 && this.widthInput <= 50){
				this.width = this.widthInput
			}
			else {
				alert("Width must be more than 0 and less than 51")
				return
			}
			if(this.heightInput > 0 && this.heightInput <= 50){
				this.height = this.heightInput
			}
			else {
				alert("Height must be more than 0 and less than 51")
				return
			}
			if(this.minesAmountInput > 0 && this.heightInput < (this.width*this.height)){
				this.minesAmount = this.minesAmountInput
			}
			else {
				alert("Mines amount must be more than 0 and less than width*height")
				return
			}
			this.newGame()
		},

		newGame() {
			for(var x=0; x<this.width; x++) {
				this.mineField[x] = [];
				for(var y=0; y<this.height; y++) {
						this.mineField[x][y] = this.createMineTile();
				}
			}

			this.generateMines()
		},

		createMineTile() {
			return {
				text: "",
				shown: false,
				mine: false,
				pinned: false,
				aroundValue: 0,
			}
		},

		generateMines() {
			//random generation of mines positions
			let maxTiles = this.width * this.height
			let minedTiles = []

			for(let i = 0; i < this.minesAmount; i++){
				let added = false
				while(!added){
					let randomTile = Math.floor(Math.random() * maxTiles)
					if(!minedTiles.includes(randomTile)){
						minedTiles.push(randomTile)
						//set as mined
						let x = Math.floor(randomTile % this.width)
						let y  = Math.floor(randomTile / this.width)
						console.log(randomTile, x, y)
						this.mineField[x][y].mine = true
						positionsAround.forEach(position => {
							let xpos = x + position.x
							let ypos = y + position.y
							if(((xpos >= 0) && (xpos < this.width)) && ((ypos >= 0) && (ypos < this.height))){
								this.mineField[xpos][ypos].aroundValue++;
							}
						})
						added = true
					}
				}
			}
		},

		tileLeftClick(mine, x, y) {
			if(this.gameDone) return
			if(mine.shown || mine.pinned) return;

			if(mine.mine){
				mine.text = "💣"
				mine.shown = true
				for(var i=0; i<this.width; i++) {
					for(var j=0; j<this.height; j++) {
						if(this.mineField[i][j].pinned || this.mineField[i][j].shown) continue
						if(this.mineField[i][j].mine){
							this.mineField[i][j].shown = true
							this.mineField[i][j].text = "💣"
						} else {
							this.mineField[i][j].shown = true
							if(this.mineField[i][j].aroundValue > 0){
								this.mineField[i][j].text = this.mineField[i][j].aroundValue
							}
						}
					}
				}
				this.gameDone = true
				alert("GAME OVER!")
			} else {
				this.showTile(mine, x, y)
			}
			console.log("left", mine, x, y)
			
		},

		showTile(mine, x, y){
			console.log("showTile", mine, x, y)
			if(mine.shown || mine.pinned || mine.mine) return;

			mine.shown = true
			if(mine.aroundValue > 0){
				mine.text = mine.aroundValue
				return
			}

			positionsAround.forEach(position => {
				let xpos = x + position.x
				let ypos = y + position.y
				if((xpos >= 0) && (xpos < this.width) && (ypos >= 0) && (ypos < this.height)){
					if(!this.mineField[xpos][ypos].shown){
						this.showTile(this.mineField[xpos][ypos], xpos, ypos)
					}
				}
			})
		},

		tileRightClick(mine) {
			if(this.gameDone) return
			if(mine.shown) return

			if(mine.pinned){
				mine.text = ""
				mine.pinned = false
			} else {
				mine.text = "📌"
				mine.pinned = true
			}
		},
	},

	created() {
		this.newGame()
	},
}
</script>

<style>
		td {
			border: 1px solid black;
			width: 25px;
			height: 25px;
		}

		table {
			border: 1px solid blue;
			margin: auto;
		}

		.hidden {
			background: gray;
		}

		.shown {
			background: white;
		}
</style>
