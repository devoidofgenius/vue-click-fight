<template>
<div id="app">
  <template v-if=gameOver>
    <div class="game-over">
      <h1 class="title">Game Over!</h1>
      <h2 v-if="playerHealth === 0" class="subtitle">You were slayed by the monster</h2>
      <h2 v-if="monsterHealth === 0" class="subtitle">You slayed the monster</h2>
    </div>
  </template>
	<div v-if=!gameOver class="game-wrapper">
		<div v-if=gameInProgress class="stats">
			<div class="player-stats">
				<h1>You <span class="heart">❤️</span> <span>{{ playerHealth }}</span> <span v-if="healingPoints !== ''" class="healing">+{{ healingPoints }}</span></h1>
				<div class="healthbar">
					<div class="current-health" :style="{ width: playerHealth + '%' }"></div>
				</div>
			</div>
			<div class="monster-stats">
				<h1>Monster <span class="heart">❤️</span></h1>
				<div class="healthbar">
					<div class="current-health" :style="{ width: monsterHealth + '%' }"></div>
				</div>
			</div>
		</div>
		<div class="game-controls">
			<template v-if=!gameInProgress>
        <h1 class="title">Click Fight</h1>
				<div class="actions">
					<button @click="gameInProgress = !gameInProgress" class="new-game">New Game</button>
				</div>
			</template>
			<template v-if=gameInProgress>
				<div class="actions">
					<button @click="meleeAttack" class="melee">Melee ⚔️</button>
					<button @click="magicAttack" class="magic">Magic 🔥</button>
					<button @click="healingSpell" class="heal">Heal 🍺</button>
					<button @click="surrender" class="surrender">Surrender ☠️</button>
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
      gameInProgress: false,
      playerTurn: true,
			gameOver: false,
			healingPoints: ''
    }
  },
  beforeUpdate: function() {
    if (this.gameOver) {
      setTimeout(() => {
        console.log("NEW GAME")
        this.playerHealth = 100
        this.monsterHealth = 100
        this.gameInProgress = false
        this.playerTurn = true
				this.gameOver = false
				this.healingPoints = ''
      }, 3000)
		}
	},
  methods: {
		randomNumber: (min, max) => {
			min = Math.ceil(min)
  		max = Math.floor(max)
  		return Math.floor(Math.random() * (max - min)) + min
    },
    checkMonsterHealth() {
      if (this.monsterHealth <= 0) {
        this.monsterHealth = 0
        this.gameOver = true
      }
    },
    checkPlayerHealth() {
      if (this.playerHealth <= 0) {
        this.playerHealth = 0
        this.gameOver = true
      }
    },
    monsterAttack(maxAttack) {
      this.playerHealth -= this.randomNumber(5, maxAttack)
      this.checkPlayerHealth()
      this.playerTurn = true
    },
		meleeAttack() {
      if (this.playerTurn) {
        this.playerTurn = false
        this.monsterHealth -= this.randomNumber(5, 10)
        this.checkMonsterHealth()
        if (!this.gameOver) {
					if (!this.gameOver) {
					setTimeout(() => {
          	this.monsterAttack(15)
					}, 500)
        }
        }
      }
		},
		magicAttack() {
      if (this.playerTurn) {
        this.playerTurn = false
        this.monsterHealth -= this.randomNumber(10, 20)
        this.checkMonsterHealth()
        if (!this.gameOver) {
					setTimeout(() => {
						this.monsterAttack(25)
					}, 500)
        }
      }
		},
		healingSpell() {
      if (this.playerTurn) {
        this.playerTurn = false
        if (this.playerHealth <= 100) {
					this.healingPoints = this.randomNumber(5, 10)
					this.playerHealth += this.healingPoints
          if (this.playerHealth > 100) this.playerHealth = 100
        }
				this.checkPlayerHealth()
        if (!this.gameOver) {
					setTimeout(() => {
						this.healingPoints = ''
          	this.monsterAttack(25)
					}, 1000)
        }
			}
    },
    surrender() {
      console.log('surrender run')
      if (this.playerTurn) {
        this.playerHealth = 0
        this.gameOver = true
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
  outline: 0;
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

    .title {
      text-transform: uppercase;
      font-family: $baloo;
      letter-spacing: 0.15em;
      color: #273A44;
      font-size: 5em;
      text-align: center;
      margin: 0;
    }

    .game-over {

      .title {
        color: #F62713;
      }
      .subtitle {
        text-transform: uppercase;
        font-family: $baloo;
        letter-spacing: 0.15em;
        color: #273A44;
        font-size: 2.5em;
        text-align: center;
        margin: 0;
      }

    }

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
				display: flex;
				justify-content: space-between;
				align-items: center;
				text-transform: uppercase;
				color: #273A44;
				position: relative;
        span {
          font-size: 0.75em;
        }
        span.heart {
          font-size: 0.5em;
					padding: 0 0.5em;
        }
				.healing {
					font-size: 0.65em;
					color: #22A369;
					position: absolute;
					right: -30px;
					top: 0;
					animation-name: fadeInOut;
					animation-timing-function: ease-in-out;
					animation-duration: 2250ms;
				}
			}

			.healthbar {
				width: 100%;
				background-color: #ECEDF2;
				height: 35px;

				.current-health {
					background-color: #22A369;
					height: 100%;
          transition: 250ms all ease-in-out;
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

				&:hover,
        &:active {
					background-color: darken(#416EFC, 10);
				}
			}

			&:hover,
      &:active {
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

@keyframes fadeInOut {
	0% {
		opacity: 0;
	}
	25%, 50% {
		opacity: 1;
	}
	100% {
		opacity: 0;
	}
}
</style>
