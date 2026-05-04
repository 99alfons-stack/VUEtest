<template>
  <div id="app">
    <h1>Packlista för vandring/tälta</h1>

    <div class="add-row">
      <input
        v-model="newItem"
        type="text"
        placeholder="Lägg till"
        @keyup.enter="addItem"
      />

      <select v-model="selectedCategory">
        <option value="gear">General gear</option>
        <option value="shelter">Shelter</option>
        <option value="clothing">Clothing</option>
        <option value="food">Food & Water</option>
        <option value="navigation">Navigation</option>
        <option value="electronics">Electronics</option>
        <option value="personal">Personal / Hygiene</option>
      </select>

      <label style="display:inline-flex;align-items:center;gap:6px;">
        <input type="checkbox" v-model="isOptional" /> Optional
      </label>

      <button @click="addItem">Lägg till</button>
    </div>

    <div class="summary">
      Packat: {{ packedCount }} / {{ totalCount }} · kvar: {{ totalCount - packedCount }} · kvar (måste): {{ requiredRemaining }}
    </div>

    <div class="category" v-for="category in categoryOrder" :key="category">
      <h2>{{ categoryLabels[category] }}</h2>

      <div v-if="!items[category] || items[category].length === 0" class="empty">
        inga saker finns här ännu.
      </div>

      <label
        v-for="(item, index) in items[category] || []"
        :key="category + '-' + index"
        class="item-row"
      >
        <input type="checkbox" v-model="item.packed" />
        <span :class="{ done: item.packed }">{{ item.name }}</span>
        <small v-if="!item.required" class="optional">(optional)</small>
      </label>
    </div>

    <button class="clear-btn" @click="removePacked">Ta bort allt som är packat</button>
  </div>
</template>

<script>


export default {
  name: 'App',

  data() {
    return {
      newItem: "",
      selectedCategory: "gear",
      // Visa general gear först
      categoryOrder: ["gear", "shelter", "clothing", "food", "navigation", "electronics", "personal"],
      categoryLabels: {
        shelter: "Shelter",
        clothing: "Clothing",
        food: "Food & Water",
        gear: "General hiking gear",
      },
      isOptional: false,
      items: {
        gear: [
          { name: "Stormkök / Trangia", packed: false, required: false },
          { name: "Pannlampa", packed: false, required: true },
          { name: "Vattenflaska / Hydration", packed: false, required: true },
          { name: "Kniv / Multiverktyg", packed: false, required: false },
          { name: "Rep / Paracord", packed: false, required: false },
        ],
        shelter: [
          { name: "Tält", packed: false, required: true },
          { name: "Sovsäck", packed: false, required: true },
          { name: "Liggunderlag", packed: false, required: true },
          { name: "Tältpinnar / reparationskit", packed: false, required: false },
        ],
        clothing: [
          { name: "Regnjacka", packed: false, required: true },
          { name: "Extra strumpor", packed: false, required: true },
          { name: "Varm tröja", packed: false, required: true },
          { name: "Badkläder / sandaler", packed: false, required: false },
        ],
        food: [
          { name: "Mat / fryspåsar", packed: false, required: true },
          { name: "Vatten / reningstabletter", packed: false, required: true },
          { name: "Snacks / energi-bars", packed: false, required: false },
        ],
        navigation: [
          { name: "Karta & kompass", packed: false, required: true },
          { name: "GPS / telefon med offline-kartor", packed: false, required: false },
        ],
        electronics: [
          { name: "Powerbank", packed: false, required: false },
          { name: "Telefon + laddare", packed: false, required: true },
        ],
        personal: [
          { name: "Första hjälpen-kit", packed: false, required: true },
          { name: "Toalettpapper / hygien", packed: false, required: false },
        ],
      },
    };
  },
  computed: {
    totalCount() {
      let total = 0;
      for (const category of this.categoryOrder) {
        total += (this.items[category] || []).length;
      }
      return total;
    },
    packedCount() {
      let packed = 0;
      for (const category of this.categoryOrder) {
        packed += (this.items[category] || []).filter((item) => item.packed).length;
      }
      return packed;
    },
    requiredRemaining() {
      let remaining = 0;
      for (const category of this.categoryOrder) {
        remaining += (this.items[category] || []).filter((it) => it.required && !it.packed).length;
      }
      return remaining;
    },
  },
  methods: {
    addItem() {
      const text = this.newItem.trim();
      if (!text) return;

      if (!this.items[this.selectedCategory]) {
        this.items[this.selectedCategory] = [];
      }

      this.items[this.selectedCategory].push({ name: text, packed: false, required: !this.isOptional });

      this.newItem = "";
      this.isOptional = false;
    },
    removePacked() {
      for (const category of this.categoryOrder) {
        this.items[category] = (this.items[category] || []).filter((item) => !item.packed);
      }
    },
  },
};

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  max-width: 760px;
  margin: 32px auto;
  text-align: left;
  color: #2c3e50;
}

h1 {
  margin-bottom: 4px;
}

.add-row {
  display: flex;
  gap: 8px;
  margin: 16px 0;
}

.add-row input,
.add-row select,
.add-row button {
  padding: 8px;
}

.summary {
  margin-bottom: 16px;
  font-weight: 600;
}

.category {
  border: 1px solid #dfe6ee;
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 12px;
  background: #f9fbfd;
}

.category h2 {
  margin-top: 0;
}

.item-row {
  display: block;
  margin: 6px 0;
}

.done {
  text-decoration: line-through;
  opacity: 0.65;
}

.empty {
  color: #6b7280;
}

.clear-btn {
  margin-top: 8px;
  padding: 10px 12px;
}
</style>