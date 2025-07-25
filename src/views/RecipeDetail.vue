<template>
  <div class="container" v-if="recipe">
    <h1>{{ recipe.strMeal }}</h1>
    <img :src="recipe.strMealThumb" alt="Meal Image" class="detail-img" />
    <p><strong>Category:</strong> {{ recipe.strCategory }}</p>
    <p><strong>Area:</strong> {{ recipe.strArea }}</p>
    <p><strong>Instructions:</strong></p>
    <p>{{ recipe.strInstructions }}</p>

    <div v-if="recipe.strYoutube">
      <p><strong>Video:</strong></p>
      <a :href="recipe.strYoutube" target="_blank">{{ recipe.strYoutube }}</a>
    </div>
  </div>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      recipe: null,
    };
  },
  async mounted() {
    const res = await fetch(
      `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.id}`
    );
    const data = await res.json();
    this.recipe = data.meals?.[0] || null;
  },
};
</script>

<style>
.detail-img {
  max-width: 400px;
  border-radius: 12px;
  margin-bottom: 1rem;
}
</style>
