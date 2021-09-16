<template>
	<div class="container">
		<div class="wrapper">
			<div class="circle circle_blue" :style="{opacity: currentCircle === 1 ? 1 : 0.5}" @click="play"></div>
			<div class="circle circle_red" :style="{opacity: currentCircle === 2 ? 1 : 0.5}" @click="play"></div>
			<div class="circle circle_yellow" :style="{opacity: currentCircle === 3 ? 1 : 0.5}" @click="play"></div>
			<div class="circle circle_green" :style="{opacity: currentCircle === 4 ? 1 : 0.5}" @click="play"></div>
		</div>
		<div v-if="round === 0" class="btn" @click="start">Начать</div>
		<div v-else class="btn" @click="restart">Рестарт</div>
		<div class="round" @click="playSound(4)">Раунд {{round}}</div>
		<div class="level">
			<h2 class="level__title">Выберете сложность</h2>
			<form action="" class="form">
				<div class="radio">
					<input type="radio" id="easy" name="level" @change="setLevel(1500)" checked>
					<label for="easy">Легкий</label>
				</div>
				<div class="radio">
					<input type="radio" id="normal" name="level" @change="setLevel(1000)">
					<label for="normal">Нормальный</label>
				</div>
				<div class="radio">
					<input type="radio" id="hard" name="level" @change="setLevel(400)">
					<label for="hard">Сложный</label>
				</div>
		</form>
		</div>
	</div>
	
</template>

<script>
import './App.scss'
export default {
  data() {
    return {
		round: 0,
		gameArray: [],
		playerArray: [],
		currentCircle: 0,
		interval: 1500
    }
  },
  methods: {
	start() {
		this.round += 1
		let rnd = Math.floor(Math.random() * 4 + 1);
		this.gameArray.push(rnd)
		for (let i = 0; i < this.gameArray.length; i++) {
			setTimeout(() => {
				this.currentCircle = this.gameArray[i]
				this.playSound(this.gameArray[i])
				setTimeout(() => {
					this.currentCircle = 0
				}, this.interval / 2)
			}, (i + 1) * this.interval);
		}
	},
	play(e) {
		if (e.target.classList.contains('circle_blue')) {
			this.playerArray.push(1)
			this.playSound(1)
		} else if (e.target.classList.contains('circle_red')) {
			this.playerArray.push(2)
			this.playSound(2)
		} else if (e.target.classList.contains('circle_yellow')) {
			this.playerArray.push(3)
			this.playSound(3)
		} else if (e.target.classList.contains('circle_green')) {
			this.playerArray.push(4)
			this.playSound(4)
		}
		for (let i = 0; i < this.playerArray.length; i++) {
			if (this.playerArray[i] === this.gameArray[i]) {
				if (this.gameArray.length === this.playerArray.length) {
					if (this.playerArray[this.playerArray.length - 1] === this.gameArray[this.gameArray.length - 1]) {
						this.playerArray = []
						this.start()
					} else {
						alert(`Вы проиграли дойдя до раунда ${this.round}`)
						this.restart()
						break
					}
				}
			} else {
				alert(`Вы проиграли дойдя до раунда ${this.round}`)
				this.restart()
				break
			}
		}
	},
	setLevel(seconds) {
		this.interval = seconds
		this.restart()
	},
	restart() {
		this.round = 0;
		this.gameArray = [];
		this.playerArray = []
	},
	playSound(src) {
		const audio = new Audio(require(`./assets/${src}.mp3`))
		audio.play()
	}
},

}
</script>

<style>

</style>