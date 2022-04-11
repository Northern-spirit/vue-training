<template>
  <div>
    <input
      v-model="ticker"
      @keydown.enter="addCards"
      placeholder="add card"
      type="text"
    />
    <ul
      v-for="card in mCard"
      :key="card.name"
      :class="shoose === card ? 'style1' : 'style2'"
      @click="select(card)"
    >
      <li>{{ card.name }} - {{ card.prise }}USD</li>

      <button @click="remote(card)">delete</button>
    </ul>
    <div v-if="shoose" class="display-flex">
      <div
        v-for="(bar, inx) in normalizeHeight()"
        :key="inx"
        class="bg-purple-800 border-gray-600 w-10"
        :style="{height: `${bar}%`}"
      >
        <div class="text-lg leading-6 font-medium text-gray-900 my-8">
          <div>{{ shoose.name }} - USD</div>
        </div>
      </div>
      <button @click="shoose = null">X</button>
    </div>
    <div v-else></div>
  </div>
</template>

<script>
export default {
  name: "CardPAge",
  data() {
    return {
      ticker: "",
      placeholder: "",
      shoose: null,
      mCard: [],
      graph: [],
    };
  },
  methods: {
    addCards() {
      const currentTicker = {
        name: this.ticker,
        prise: "-",
      };
      this.ticker = "";
      this.mCard.push(currentTicker);

      //берем данный криптовалют и валют по API ключу
      setInterval(async () => {
        const f = await fetch(
          `https://min-api.cryptocompare.com/data/price?fsym=${currentTicker.name}&tsyms=USD&api_key=d22ead832d59cca8773f2c2f8c00860a26470103c028af209df0e0ee4e8c5989`
        );
        const data = await f.json();
        console.log(data.USD);

        // находим в новых введенных валютах одинаковое имя с нашим введенным инпутом и в прайс кладем значение приходящее из API (сумму)
        this.mCard.find((card) => card.name === currentTicker.name).prise =
          data.USD;
        currentTicker.prise = data.USD;
        if (this.shoose?.name === currentTicker.name) {
          this.graph.push(data.USD);
        }
      }, 5000);
    },
    //Удаление выбранной карточки
    remote(remote) {
      this.mCard = this.mCard.filter((card) => card != remote);
    },
    normalizeHeight() {
      const norMax = Math.max(...this.graph);
      const norMin = Math.min(...this.graph);
      return this.graph.map(e => 5 + ((e-norMin)*95)/(norMax-norMin))
    },
    select(card) {
      this.shoose = card;
      this.graph = [];      
    },
  },
};
</script>

<style scoped>

.display-flex {
  display: flex;
  justify-content: space-around;
}
.bg-purple-800 {
  --bg-opacity: 1;
  background-color: #553c9a;
  background-color: rgba(85, 60, 154, var(--bg-opacity));
}

.border-gray-600 {
  --border-opacity: 1;
  border-color: #718096;
  border-color: rgba(113, 128, 150, var(--border-opacity));
}

.w-10 {
  width: 2.5rem;
}

.h-24 {
  height: 6rem;
}
.text-lg {
  font-size: 1.125rem;
}
.leading-6 {
  line-height: 1.5rem;
}
.font-medium {
  font-weight: 500;
}
.text-gray-900 {
  --text-opacity: 1;
  color: #1a202c;
  color: rgba(26, 32, 44, var(--text-opacity));
}
.my-8 {
  margin-top: 2rem;
  margin-bottom: 2rem;
}
</style>