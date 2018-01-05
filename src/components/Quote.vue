<template>
  <div class="row">
    <div class="center">
      <div class="col-sm-12">
        <div class="quotebox">
          <p class="quotebox__mark">
            “
          </p>
          <p class="quotebox__text js-copytextarea">
            <b>{{ coin }}</b> is a hoax, because {{ reason }}
          </p>
          <hr class="quotebox__line">
          <div>
            <p class="quotebox__author float-left">
              {{ author.first_name }} {{ author.last_name }}, {{ profession }}
            </p>
            <p class="float-right">
              <a v-bind:href="'https://twitter.com/intent/tweet?url=https://cryptohoax.party/&text=' + fullQuote" target="_blank">
                <i class="fab fa-2x fa-twitter"></i>
              </a>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { EventBus } from '../event-bus.js';

export default {
  data() {
    return {
      coins: {},
      reasons: [],
      professions: [],
      coin: 'Ripple',
      reason: 'my cat ate all my Dogecoins',
      author: {
        first_name: 'Abraham',
        last_name: 'Lincoln'
      },
      profession: 'Banker'
    }
  },
  computed: {
    fullQuote() {
      return '"' + this.coin + ' is a hoax, because ' + this.reason + '" - ' + this.capitalizedFirstName + ' ' + this.capitalizedLastName + ', ' + this.profession;
    },
    capitalizedFirstName() {
      var string = this.author.first_name;
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    capitalizedLastName() {
      var string = this.author.last_name;
      return string.charAt(0).toUpperCase() + string.slice(1);
    }
  },
  methods: {
    randomizeQuote(){
      this.randomizeCoin();
      this.randomizeReason();
      this.randomizeAuthor();
      this.randomizeProfession();
    },
    randomizeCoin(){
      var keys = Object.keys(this.coins);
      var coin = this.coins[keys[ keys.length * Math.random() << 0]];
      this.coin = coin.CoinName;
    },
    randomizeReason(){
      this.reason = this.reasons[ this.reasons.length * Math.random() << 0];
    },
    randomizeAuthor () {
      axios.get(`https://randomuser.me/api`)
      .then(response => {
        const name = response.data.results[0].name
        this.author.first_name = name.first;
        this.author.last_name = name.last;
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    randomizeProfession() {
      this.profession = this.professions[ this.professions.length * Math.random() << 0];
    }
  },
  created() {
    const self = this;

    EventBus.$on('randomizer-called', function (id) {
      self.randomizeQuote();
    });

    axios.get(`https://s3.eu-central-1.amazonaws.com/cryptohoax.party/coins.json`)
    .then(response => {
      this.coins = response.data.Data
    })
    .catch(e => {
      this.errors.push(e)
    })

    axios.get(`https://s3.eu-central-1.amazonaws.com/cryptohoax.party/data.json`)
    .then(response => {
      this.reasons = response.data.reasons
      this.professions = response.data.professions
    })
    .catch(e => {
      this.errors.push(e)
    })
  }
}
</script>

<style lang="scss" scoped>

.center {
  margin: 0 auto;
}

.quotebox {
  overflow: hidden;
  padding: 30px;
  border-radius: 17px;
  background-color: #1E1F27;
  color: white;
  box-shadow: 0 2px 20px rgba(0,0,0,0.17);

  @media (min-width: 760px){
    width: 600px;
  }

  &__mark {
    margin-top: -10px;
    font-weight: bold;
    font-size:100px;
    color:white;
    font-family: 'Times New Roman', Georgia, Serif;
  }

  &__text {
    font-size: 2rem;
    margin-top: -65px;
  }

  &__line {
    border-top: 1px solid white;
    color: transparent;
  }

  &__author {
    font-size: 1.3em;
    text-transform: capitalize;
  }
}
</style>
