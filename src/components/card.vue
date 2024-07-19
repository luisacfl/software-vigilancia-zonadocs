<template>
  <div>
    <h1>Manage Variables with Selector Controls</h1>
    
    <div class="filters">
      <!-- Category Selector -->
      <div class="filter">
        <label for="category">Select Category:</label>
        <select v-model="selectedCategory">
          <option value="All">All</option>
          <option v-for="category in categories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>
      </div>

      <!-- Year Selector -->
      <div class="filter">
        <label for="year">Select Year:</label>
        <select v-model="selectedYear">
          <option value="All">All</option>
          <option v-for="year in years" :key="year" :value="year">
            {{ year }}
          </option>
        </select>
      </div>
    </div>

    <!-- Display Cards -->
    <div class="card-group" v-for="(cards, year) in filteredAndGroupedCards" :key="year">
      <h2>{{ year }}</h2>
      <div class="cards">
        <div class="card" v-for="card in cards" :key="card.id">
          <h3>{{ card.title }}</h3>
          <p>{{ card.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

// Sample card data
const cards = ref([
  { id: 1, title: 'Card 1', description: 'Description 1', category: 'Category A', year: 2022 },
  { id: 2, title: 'Card 2', description: 'Description 2', category: 'Category B', year: 2023 },
  { id: 3, title: 'Card 3', description: 'Description 3', category: 'Category A', year: 2021 },
  { id: 4, title: 'Card 4', description: 'Description 4', category: 'Category C', year: 2023 },
  // Add more cards as needed
]);

// Reactive references for selected category and year
const selectedCategory = ref('All');
const selectedYear = ref('All');

// Extract unique categories and years from the card data
const categories = computed(() => {
  const allCategories = cards.value.map(card => card.category);
  return [...new Set(allCategories)];
});

const years = computed(() => {
  const allYears = cards.value.map(card => card.year);
  return [...new Set(allYears)];
});

// Filter and group cards based on selected category and year
const filteredAndGroupedCards = computed(() => {
  let filteredCards = cards.value;

  if (selectedCategory.value !== 'All') {
    filteredCards = filteredCards.filter(card => card.category === selectedCategory.value);
  }

  if (selectedYear.value !== 'All') {
    filteredCards = filteredCards.filter(card => card.year === parseInt(selectedYear.value, 10));
  }

  const groupedByYear = filteredCards.reduce((acc, card) => {
    (acc[card.year] = acc[card.year] || []).push(card);
    return acc;
  }, {});

  return groupedByYear;
});
</script>

<style scoped>
.filters {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.filter {
  display: flex;
  flex-direction: column;
}

.card-group {
  margin-bottom: 20px;
}

.cards {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.card {
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
  background-color: #f9f9f9;
  width: 200px;
}
</style>
