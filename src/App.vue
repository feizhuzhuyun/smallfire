<template>
	<div id="app">
    <!-- 第一组火柴 -->
		<ul class="fireBox">
			<li
				class="fireElement"
				v-for="(item, index) in listOne"
				:key="index"
				@click="fireElementClick('listOne', index)"
				:class="{ active: listOne[index].active }"
			>
				<span class="fireHeader"></span>
				<span class="fireBody"></span>
			</li>
		</ul>
    <!-- 第二组火柴 -->
		<ul class="fireBox">
			<li
				class="fireElement"
				v-for="(item, index) in listTwo"
				:key="index"
				@click="fireElementClick('listTwo', index)"
				:class="{ active: listTwo[index].active }"
			>
				<span class="fireHeader"></span>
				<span class="fireBody"></span>
			</li>
		</ul>
    <!-- 第三组火柴 -->
		<ul class="fireBox">
			<li
				class="fireElement"
				v-for="(item, index) in listThree"
				:key="index"
				@click="fireElementClick('listThree', index)"
				:class="{ active: listThree[index].active }"
			>
				<span class="fireHeader"></span>
				<span class="fireBody"></span>
			</li>
		</ul>
    <!-- 按钮组 -->
		<div class="btn">
			<button @click="submit">submit</button>
			<button @click="reset">reset</button>
			<button @click="faild">faild</button>
		</div>
		<p :class="{ player1: round == 0, player2: round == 1 }">{{ player[round] }}</p>
	</div>
</template>

<script>
/* eslint-disable */
export default {
	name: "App",
	components: {},
	data() {
		return {
			round: 0,//当前回合
			player: ["玩家1", "玩家2"],//玩家组
			listOne: [//火柴组
				{ active: false },
				{ active: false },
				{ active: false }
			],
			listTwo: [
				{ active: false },
				{ active: false },
				{ active: false },
				{ active: false },
				{ active: false },
			],
			listThree: [
        { active: false },
				{ active: false },
        { active: false },
				{ active: false },
        { active: false },
				{ active: false },
				{ active: false },
			]
		};
	},
	computed: {
		//计算出当前激活的火材组
		activeListNumber() {
			let sum = 0;
			let allList = [this.listOne, this.listTwo, this.listThree];
			for (const listItem of allList) {
				for (const key of listItem) {
					if (key.active == true) {//如果按钮组中存在激活状态火柴,则加一计数
						sum++;
						break;
					}
				}
			}
			return sum;
		}
	},
	methods: {
		//火柴点击
		fireElementClick(list, index) {
			this[list][index].active = !!!this[list][index].active;//active取反
		},
		//提交
		submit() {
			//选择的火材组超过一个则不能提交
			if (this.activeListNumber > 1) {
				alert("你选了多个火材组!");
				this.reset(); // 重置火柴激活状态
				return false;
			}
			//可以提交,数组重新赋值,只选出未激活状态的火柴
			//返回一个符合条件的新数组
			this.listOne = this.listOne.filter((item, index) => {
				return item.active == false;
			});
			this.listTwo = this.listTwo.filter((item, index) => {
				return item.active == false;
			});
			this.listThree = this.listThree.filter((item, index) => {
				return item.active == false;
			});

			//如果数组都为空,当前玩家失败
			if (!this.listOne.length &&!this.listTwo.length &&!this.listThree.length) {
        this.faild();
        return false
			}
			//进入下一回合
			this.round = this.round == 0 ? 1 : 0;
		},
		// 重置火柴激活状态
		reset() {
			let allList = [this.listOne, this.listTwo, this.listThree];
			for (const listItem of allList) {
				for (const item of listItem) {
					item.active = false;
				}
			}
		},
		//认输
		faild() {
			alert(this.player[this.round] + "输了");
			//重新开始游戏
			(this.listOne = [
				{active: false },
				{active: false },
				{active: false }
			]),
				(this.listTwo = [
					{active: false },
					{active: false },
					{active: false },
					{active: false },
					{active: false }
				]),
				(this.listThree = [
					{active: false },
					{active: false },
					{active: false },
					{active: false },
					{active: false },
					{active: false },
					{active: false }
				]);
		}
	}
};
</script>

<style lang="less">
	#app {
		display: flex;
		flex-direction: column;
		.fireBox {
			display: flex;
		}
		.fireElement {
			display: flex;
			align-items: center;
			flex-direction: column;
			.fireHeader {
				width: 30px;
				height: 30px;
				border-radius: 15px;
				background-color: red;
			}
			.fireBody {
				width: 10px;
				height: 100px;
				background-color: rgb(100, 87, 63);
			}
		}
		.fireElement.active {
			.fireHeader {
				background-color: rgb(253, 111, 111);
			}
			.fireBody {
				background-color: rgb(110, 110, 110);
			}
		}
		.btn {
			display: flex;
			margin-left: 25px;
			button {
				margin-left: 20px;
				width: 50px;
				height: 30px;
				border-radius: 10px;
			}
		}
		.player1 {
			text-align: center;
			font-size: 32px;
			color: pink;
		}
		.player2 {
			text-align: center;
			font-size: 32px;
			color: blue;
		}
	}
</style>
