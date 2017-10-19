<template>
<div id="app">
	<div class="game-wrapper">
		<div class="stats">
			<div class="player-stats">
				<h1>You</h1>
				<div class="healthbar">
					<div class="current-health" :style="{ width: playerHealth + '%' }"></div>
				</div>
			</div>
			<div class="monster-stats">
				<h1>Monster</h1>
				<div class="healthbar">
					<div class="current-health" :style="{ width: monsterHealth + '%' }"></div>
				</div>
			</div>
		</div>
		<div class="game-controls">
			<template v-if=!gameInProgress>
				<div class="actions">
					<button @click="gameInProgress = !gameInProgress" class="new-game">New Game</button>
				</div>
			</template>
			<template v-if=gameInProgress>
				<div class="actions">
					<button @click="meleeAttack" class="melee">Melee ⚔️</button>
					<button @click="magicAttack" class="magic">Magic 🔥</button>
					<button class="heal">Heal 🍺</button>
					<button class="surrender">Surrender ☠️</button>
				</div>
			</template>
		</div>
	</div>
</div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      playerHealth: 100,
		  monsterHealth: 100,
		  gameInProgress: false
    }
  },
  methods: {
		randomNumber(min, max) {
			min = Math.ceil(min)
  		max = Math.floor(max)
  		return Math.floor(Math.random() * (max - min)) + min
		},
		meleeAttack() {
			this.monsterHealth -= this.randomNumber(0, 5)
			if (this.monsterHealth <= 0) {
				this.monsterHealth = 0
			}
		},
		magicAttack() {
			this.monsterHealth -= this.randomNumber(5, 20)
			if (this.monsterHealth <= 0) {
				this.monsterHealth = 0
			}
		},
		healingSpell() {
			// Need to not go over 100 health
			this.playerHealth += this.randomNumber(0, 10)
			if (this.playerHealth >= 0) {
				this.playerHealth = 100
			}
		}
	}
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Baloo+Chettan');

$baloo: 'Baloo Chettan', cursive;

* {
	box-sizing: border-box;
}

html,
body {
	height: 100%;
	width: 100%;

	#app {
		height: 100%;
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
	}

}

.game-wrapper {
	max-width: 1300px;
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;

	.stats {
		width: 100%;
		display: flex;

		.player-stats,
		.monster-stats {
			display: flex;
			flex-direction: column;
			align-items: center;
			width: 50%;
			margin: 1em;
			font-family: $baloo;
			letter-spacing: 0.15em;

			h1 {
				text-transform: uppercase;
				color: #273A44;
			}

			.healthbar {
				width: 100%;
				background-color: #ECEDF2;
				height: 35px;

				.current-health {
					background-color: #22A369;
					height: 100%;
				}

			}

		}

	}

	.game-controls {
		width: 65%;
		padding: 5em 0;

		button {
			padding: .75em 1em;
			text-transform: uppercase;
			border: 0;
			color: #273A44;
			font-family: $baloo;
			font-size: 1.25em;
			letter-spacing: 0.15em;
			border-radius: 1.5em;
			background-color: #ECEDF2;
			transition: 250ms all ease-in-out;
			cursor: pointer;

			&.new-game {
				color: #ECEDF2;
				background-color: #416EFC;

				&:hover {
					background-color: darken(#416EFC, 10);
				}
			}

			&:hover {
				background-color: #768092;
				color: #ECEDF2;
			}

		}

		.actions {
			display: flex;
			justify-content: space-evenly;
		}

	}

}
</style>
