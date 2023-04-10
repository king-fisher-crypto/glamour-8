<template>
	<div>		
		<div class="setup-container d-flex flex-direction-col justify-between align-center" v-if="!isStarted">
			<Loading />
			<div class="setup animate-fadeIn-s">
				<Logo/>
				<div class="hero-banner d-flex flex-direction-col">
					<h1>Tvoja glamuroznost je</h1>
					<img src="../assets/images/neskoncna_logo.png" alt="" rel="preload">
					<h2>Naj danes še bolj zasije.</h2>
				</div>
				<div class="description animate-fadeIn">
					<p>
						Danes je tvoj dan! Na ta dan slavimo<br class="sp">
						<strong>neskončno veličino in glamuroznost,</strong><br>
						ki jo vsaka ženska skriva v sebi. <strong>Glamour</strong><br class="sp">
						te želi kot <strong>najbolj ženstvena znamka</strong> <br>
						opomniti, da si lahko samozavestna, drzna<br class="sp">
						in da se v tebi skriva <strong>neskončna</strong> moč. Ne<br>
						samo danes, temveč vsak dan. 
					</p>
					<p>
						<strong>Ker si za nas številka <span>1</span>, te danes<br class="sp">
						obdarujemo kar dvojno.</strong> Po končani<br>
						aktivaciji te namreč čakata <strong>unikaten žig in<br class="sp">
						kuponček za Alverde balzam za<br>
						ustnice v dveh odtenkih.</strong>
					</p>
				</div>
				<div class="action-warpper animate-fadeIn">
					<button type="button" class="btn primary" @click="startQuiz()">začni</button>
				</div>				
			</div>
		</div>
		<div v-if="isStarted">
			<div class="quiz animate-fadeIn" v-if="currentQuestionIndex < questions.length && chosenAnswers[currentQuestionIndex] == undefined " :key="currentQuestionIndex">
				<Logo/>
				<div class="hero-banner text-center">
					<img src="../assets/images/quiz-banner.png" alt="Quiz" rel="preload">
				</div>
				
				<h3 class="subtitle" v-if="currentQuestionIndex == 0">
					Med tremi zastavljenimi odgovori na<br class="sp">
					vprašanje izberi pravilnega in osveži<br>
					svoje znanje o najbolj ženstveni <br class="sp">
					blagovni znamki <strong>GLAMOUR.</strong>
				</h3>
				<div class="answers">
					<p class="overview" v-html="questions[currentQuestionIndex].description"></p>
					<h3 class="answers--title" v-html="questions[currentQuestionIndex].question"></h3>
					<div class="answers--container">
						<div class="answer answer--option" v-for="(answer, index) in questions[currentQuestionIndex].answers" @click="selectAnswer(index)">{{ `${alphabets[index]} ) ${answer}`}}</div>						
					</div>
				</div>
			</div>
			<div class="result animate-right" v-else-if="chosenAnswers[currentQuestionIndex] != undefined" @click="nextQuestion()">	
				<Logo/>			
				<div class="hero-banner text-center">
					<img src="../assets/images/quiz-banner.png" alt="Quiz" rel="preload" v-if="chosenAnswers[currentQuestionIndex] == true">
          <img src="../assets/images/incorrect.png" alt="Quiz" rel="preload" v-else>
				</div>				
				<h2 class="title" v-html="currentQuestionResult.title"></h2>
				<div class="product">
					<p class="product--description" v-html="currentQuestionResult.description"></p>
					<div class="product--info" v-if="questions[currentQuestionIndex].product_info != undefined">
						<img :src="require(`../assets/images/${questions[currentQuestionIndex]['product_info'].image_path}`)" alt="thumbnail" rel="preload">
						<div class="detail">
							<h3 class="brand">Glamour</h3>
							<h4 class="name">{{ questions[currentQuestionIndex]['product_info'].name }}</h4>
							<div class="tar">
								<h5 class="title">KATRAN</h5>
								<p><span class="number">{{ questions[currentQuestionIndex]['product_info'].tar_amount }}</span> mg</p>
							</div>
							<div class="price">{{ `${questions[currentQuestionIndex]['product_info'].price} €` }}</div>
						</div>
					</div>
				</div>
			</div>
			<div class="quiz-completed animate-fadeIn" v-else-if="currentQuestionIndex >= questions.length">
				<Logo/>
				<div class="top">
					<div class="hero-banner d-flex flex-direction-col">
						<h1>Glamour</h1>
						<img src="../assets/images/completed-banner.png" alt="" rel="preload">
						<h2>ŠT. <code>1</code></h2>
					</div>
					<p class="first text-center">
						Glamurozno! Osvojila si znanje o<strong>znamki,<br class="sp"> 
						ki št. <span>1</span> ostaja za vedno</strong> Zdaj
						je tvoje<br> <strong>elegantno darilo</strong>, s katerim boš lahko<br class="sp"> 
						zasijala v vsej svoji veličini.
					</p>
					<div class="gift-wrapper text-center">
						<img src="../assets/images/gift.png" alt="Gift" rel="preload">
					</div>					
					<p class="second text-center">
						<strong>Kupon za Alverde balzam za ustnice<br class="sp">
						v dveh odtenkih</strong> boš prejela na e-naslov,<br>
						ki ga imaš navedenega na profilu portala.<br class="sp">
						Darilo v <strong>izbranem odtenku</strong> lahko<br>
						prevzameš v vseh <strong>fizičnih prodajalnah<br class="sp"> 
						DM po Sloveniji.</strong>

            			<strong>Kupon</strong> za lak za nohte Catrice boš
						prejela<br class="sp"> na e-naslov, ki ga imaš
						navedenega na<br> profilu portala.
						Odtenek <strong>lahko izbereš<br class="sp"> po svojem
						okusu</strong>, lak pa prevzameš v<br> vseh
						<strong>fizičnih prodajalnah DM po<br class="sp"> Sloveniji.</strong>
					</p>
					<h6>Tvoj Glamour</h6>
					<div class="action-wrapper">
						<a href="https://aktivacije.nas-partner.si/" class="btn primary small">nazaj na portal</a>
					</div>
				</div>	
			</div>
		</div>
	</div>
</template>

<script>
import Loading from './Loading.vue';
import Logo from './Logo.vue'
import Vue from 'vue';

import api from '~/plugins/api'

export default {
	name: 'Quiz',
	components: {
		Loading,
		Logo
	},
	data() {
		return {
			isStarted: false,
			questions: [],
			alphabets: [],
			currentQuestionResult: {},
			chosenAnswers: [],
			chosenAnswerExactness: 0,
			currentQuestionIndex: 0
		};
	},
	mounted() {
		this.init()
	},
	methods: {
		init() {
			window.Vue = Vue;
			
			this.questions = [
				{
					id: 1,
					description: 'Vsaka posameznica glamuroznost izžareva<br class="sp"> na svoj način, zato Glamour nudi asortiman,<br> v katerem lahko vsaka polnoletna kadilka<br class="sp"> poišče nekaj zase.',
					question: 'Katere Glamour cigarete so najbolj<br class="sp">priljubljena izbira polnoletnih kadilk?',
					correct_answer: 'Glamour Amber',
					answers: [
						'Glamour Amber', 'Glamour Azure', 'Glamour Lilac'
					],
					result: {
						correct: {
							title: 'Krasno!<br>Prav imaš.',
							description: 'Najbolj priljubljena izbira slovenskih polnoletnih<br class="sp"> kadilk je <strong>Glamour Amber.</strong>',
						},
						incorrect: {
							title: 'Žal ne …',
							description: 'Najbolj priljubljena izbira slovenskih polnoletnih <br class="sp"> kadilk je <strong>Glamour Amber.</strong>',
						}
					},
					product_info: {
						image_path: 'product_1.png',
						name: 'Amber',
						tar_amount: 1,
						price: "4.40"
					}
				},
				{
					id: 2,
					description: 'Glamour vse ženske na <span>1</span>. mesto<br class="sp">postavlja vsakodnevno. Danes pa<br>slavimo prav tebe in tvojo energijo.',
					question: 'Glamour že leta ostaja številka <code>1</code><br class="sp">v segmentu:',
					correct_answer: 'super slim cigaret',
					answers: [
						'slim cigaret', 'super slim cigaret', 'king size cigaret'
					],
					result: {
						correct: {
							title: 'Odlično<br>ti gre.',
							description: 'Glamour že leta ostaja št. <span>1</span> v segmentu <br class="sp"><strong>super slim cigaret.</strong>'
						},
						incorrect: {
							title: 'Opa, za las<br>si zgrešila.',
							description: 'Glamour namreč že leta ostaja št. <span>1</span> v segmentu <br class="sp"><strong>super slim cigaret.</strong>'
						}
					}
				},
				{
					id: 3,
					description: 'Glamour si prizadeva polnoletnim<br class="sp">kadilkam ponuditi najboljšo kvaliteto<br> po vrhunski ceni.',
					question: 'Zato lahko Glamour cigarete<br class="sp">polnoletnim kadilkam ponudiš že za:',
					correct_answer: '4.40 €',
					answers: [
						'4.40 €', '4.50 €', '4.30 €'
					],
					result: {
						correct: {
							title: 'Glamurozno,<br>tvoj odgovor<br>je pravilen.',
							description: 'Polnoletne kadilke lahko Glamour cigarete<br class="sp">dobijo po vrhunski ceni <strong>4.40 €.</strong>'
						},
						incorrect: {
							title: 'Ups,<br>odgovor je<br>napačen.',
							description: 'Polnoletne kadilke lahko Glamour cigarete<br class="sp">dobijo po vrhunski ceni <strong>4.40 €.</strong>'
						}
					}
				},
				{
					id: 4,
					description: 'Pri Glamourju vemo, da ima vsaka<br class="sp"> ženska edinstvene želje in čisto<br> vsaka izmed želja je enako<br class="sp">pomembna. Zato je Glamour<br>ustvarjen za moderne polnoletne<br class="sp">kadilke, ki zase izbirajo le najboljše.',
					question: 'Katere cigarete iz Glamour<br class="sp">asortimana najpogosteje<br>ponudiš polnoletnim kadilkam,<br class="sp">ki želijo najmočnejši okus<br> kajenja?',
					correct_answer: 'Glamour Lilac',
					answers: [
						'Glamour Amber', 'Glamour Lilac', 'Glamour Azure'
					],
					result: {
						correct: {
							title: 'Odlična<br>izbira.',
							description: '<strong>Glamour Lilac</strong> so s 5 mg katrana najmočnejše<br class="sp"> cigarete iz Glamour asortimana in tako z<br> lahkoto prepričajo polnoletne kadilke z željo po<br class="sp"> močnem okusu kajenja.'
						},
						incorrect: {
							title: 'Na žalost<br>odgovor<br>ni pravilen.',
							description: 'Najmočnejše cigarete iz Glamour asortimana so<br class="sp"> s 5 mg katrana <strong>Glamour Lilac.</strong>'
						}
					},
					product_info: {
						image_path: 'product_2.png',
						name: 'Lilac',
						tar_amount: 5,
						price: "4.40"
					}
				}
			];
			
			for (let i = 0; i < 26; i++) {
				this.alphabets.push(String.fromCharCode(i + 97).toUpperCase())				
			}
		},
		startQuiz() {
      this.isStarted = true
			this.scrollToTop()
		},
		selectAnswer(index) {      	
			let checkState = (this.questions[this.currentQuestionIndex].correct_answer == this.questions[this.currentQuestionIndex].answers[index])
			let str = (checkState) ? 'correct' : 'incorrect'

			this.currentQuestionResult = this.questions[this.currentQuestionIndex].result[str]
			window.Vue.set(this.chosenAnswers, this.currentQuestionIndex, checkState)
			
			this.scrollToTop()
    },
	nextQuestion() {			
		this.currentQuestionResult = {};
		this.currentQuestionIndex++
		
      	if (this.currentQuestionIndex == 4) this.logPart()

			this.scrollToTop()
		},
		scrollToTop() {
			window.scrollTo({ top: 0 })
		},
    logPart() {      
      api
        .logParticipation({
          activationId: 96
        })
        .then((response) => {})
        .catch((errorMessage, error) => {})
        .then(() => {})
    }
	}	
}
</script>