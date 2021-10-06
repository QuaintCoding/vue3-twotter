<template>
<div class="user-profile">
    <div class="user-profile__user-panel">
        <h1 class="user-profile__username">@{{ user.username }} - {{ fullName }}</h1>
        <div class="user-profile__admin-badge" v-if="user.isAdmin">
            Admin
        </div>
        <div class="user-profile__admin-badge" v-else>
            Not Admin
        </div>
        <div class="user-profile__follower-count">
            <strong>Followers: </strong> {{ followers }}
        </div>
        <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
            <label for="newTwoot"><strong>Mew Twoot</strong></label>
            <textarea id="newTwoot" rows="4" v-model="newTwootContent"/>

            <div class="user-profile__create-twoot-type">
                <label for="newTwootType"><strong>Type: </strong></label>
                <select id="newTwootType" v-model="selectedTwootType">
                    <!-- :가 없으면 option.value는 단순 string이지만 :를 붙이면 변수에 대한 값을 참조할 수 있음 -->
                    <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                        {{ option.name }}
                    </option>
                </select>
            </div>

            <button>
                Twoot!
            </button>
        </form>
    </div>
    <div class="user-profile__twoots-wrapper">
        <TwootItem 
            v-for="twoot in user.twoots" 
            :key="twoot.id" 
            :username="user.username" 
            :twoot="twoot" 
            @favorite="toggleFavorite"
        />
    </div>
</div>
</template>

<script>
import TwootItem from "./Twootitem.vue"

export default {
  name: 'UserProfile',
  components: { TwootItem },
  data() { // like a function
    return { // 여기에서 쓸 변수들을 선언한다.
        newTwootContent: '',
        selectedTwootType: 'instant',
        twootTypes: [
            { value: 'draft', name: 'Draft' },
            { value: 'instant', name: 'Instant Twoot'}
        ],
        followers: 0,
        user: {
            id:1,
            username: '_Ha',
            firstName: 'Mit',
            lastName: 'Rom',
            email: 'abc@gmail.com',
            isAdmin: true,
            twoots: [
                { id: 1, content: "Twotter is Amazing!" },
                { id: 2, content: "Don't forget" }
            ]
        }
    }
  },
  watch: { // 특정 data가 변화하는지 확인한다.
    followers(newFollowerCount, oldFollowerCount) { // 현재와 전이 다른지 비교하는 방식
      if(oldFollowerCount < newFollowerCount){
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },
  computed: { 
    fullName() { // 위의 data에 선언한 데이터에 접근하려면 this를 사용해야 한다.
      return `${this.user. firstName} ${this.user.lastName}`; // 백틱
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
        console.log(`Favorite Tweet #${id}`);
    },
    createNewTwoot() {
        if (this.newTwootContent && this.selectedTwootType !== 'draft') {
            this.user.twoots.unshift({
                id: this.user.twoots.length + 1,
                content: this.newTwootContent
            })
            this.newTwootContent = '';
        }
    }
  },
  mounted() { // lifecycle hookup(create, destroy), going to run whenever the componenet is loaded for the first time
    this.followUser();
  }
}
</script>

<style>
.user-profile {
    display: grid;
    grid-template-columns: 3fr 1fr;
    width: 100%;
    padding: 50px 5%;
}

.user-profile__user-panel{
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
}

h1{
    margin: 0;
}

.user-profile__admin-badge {
    background-color: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}

.user-profile__create-twoot {
    padding-top: 20px;
    display: flex;
    flex-direction: column;
}
</style>
