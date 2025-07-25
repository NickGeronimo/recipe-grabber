<template>
  <div class="container">
    <h1>🍽 Recipe Finder</h1>

    <div class="search">
      <input
        v-model="searchQuery"
        @keyup.enter="searchRecipes"
        placeholder="Search for recipes..."
      />
      <button @click="searchRecipes">Search</button>
    </div>

    <div v-if="loading">Loading...</div>
    <div v-else-if="recipes.length === 0 && searched">No recipes found.</div>

    <div class="recipes">
      <div v-for="recipe in recipes" :key="recipe.idMeal" class="recipe-card">
<router-link
  :to="`/recipe/${recipe.idMeal}`"
>
  <img :src="recipe.strMealThumb" alt="Meal Image" />
</router-link>
        <h2>{{ recipe.strMeal }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      recipes: [],
      loading: false,
      searched: false,
    };
  },
  methods: {
    async searchRecipes() {
      if (!this.searchQuery) return;

      this.loading = true;
      this.searched = true;

      try {
        const res = await fetch(
          `https://www.themealdb.com/api/json/v1/1/search.php?s=${this.searchQuery}`
        );
        const data = await res.json();
        this.recipes = data.meals || [];
      } catch (err) {
        console.error('Error fetching recipes:', err);
        this.recipes = [];
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

