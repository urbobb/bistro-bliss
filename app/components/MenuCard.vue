<script setup lang="ts">
import { computed, ref, onMounted, onUnmounted } from "vue";

const props = defineProps<{
  sectiontitle: string;
  dishes: Dish[];
}>();
const isMobile = ref(false);
const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768;
};

onMounted(() => {
  checkMobile();
  window.addEventListener("resize", checkMobile);
});

onUnmounted(() => {
  window.removeEventListener("resize", checkMobile);
});

/* Split the dishes based on screen size.
Show 4 dishes per slide on mobile, 8 otherwise  
*/
const dishSlides = computed(() => {
  const slides = [];
  const dishesPerSlide = isMobile.value ? 4 : 8;
  for (let i = 0; i < props.dishes.length; i += dishesPerSlide) {
    slides.push(props.dishes.slice(i, i + dishesPerSlide));
  }
  return slides;
});
</script>

<template>
  <div class="menu-section">
    <div class="menu-section-title">
      <div class="infinity"></div>
      <h2>{{ props.sectiontitle }}</h2>
      <div class="infinity"></div>
    </div>

    <UCarousel
      v-slot="{ item: slideDishes }"
      :items="dishSlides"
      arrows
      :prev="{
        icon: 'i-heroicons-outline-chevron-left',
        color: 'neutral',
        variant: 'ghost',
      }"
      :next="{
        icon: 'i-heroicons-outline-chevron-right',
        color: 'neutral',
        variant: 'ghost',
      }"
      :ui="{
        item: 'basis-full',
        container: 'rounded-lg',
        prev: '-start-10 sm:-start-12',
        next: '-end-10 sm:-end-12',
      }">
      <div class="slide-content">
        <div class="column">
          <div
            v-for="(dish, index) in slideDishes.slice(0, 4)"
            :key="`col1-${index}`"
            class="dish">
            <div class="dish-details">
              <h3>{{ dish.title }}</h3>
              <p>{{ dish.price }}€</p>
            </div>
            <div class="dish-description">
              <p>{{ dish.description }}</p>
            </div>
          </div>
        </div>
        <div v-if="!isMobile" class="column">
          <div
            v-for="(dish, index) in slideDishes.slice(4, 8)"
            :key="`col2-${index}`"
            class="dish">
            <div class="dish-details">
              <h3>{{ dish.title }}</h3>
              <p>{{ dish.price }}€</p>
            </div>
            <div class="dish-description">
              <p>{{ dish.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </UCarousel>
  </div>
</template>

<style scoped>
.menu-section {
  padding: 20px 40px;
  color: white;
  position: relative;
}

.menu-section-title {
  display: flex;
  flex-direction: row;
  gap: 20px;
  justify-content: space-between;
  align-items: center;
  justify-content: center;
  align-items: center;
  margin-bottom: 75px;
}

.menu-section-title h2 {
  color: #b29771;
  font-size: 20pt;
  letter-spacing: 15%;
  white-space: nowrap;
  font-family: "Cormorant Garamond", sans-serif;
}

.slide-content {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  gap: 75px;
  min-height: 500px;
}

.column {
  display: flex;
  flex-direction: column;
  width: 48%;
  gap: 20px;
}

.dish {
  display: flex;
  flex-direction: column;
  align-items: start;
  text-align: center;
  gap: 5px;
  height: 125px;
  font-family: "Jost", sans-serif;
}

.dish-details {
  position: relative;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: baseline;
  width: 100%;
  color: #b29771;
  border-bottom: 1px solid #333;
  padding-bottom: 10px;
}

.dish-details h3 {
  margin: 0;
  text-align: left;
  letter-spacing: 10%;
}

.dish-details p {
  margin: 0;
  white-space: nowrap;
  text-align: right;
  font-weight: 600;
}

.dish-description {
  width: 75%;
  text-align: left;
  color: #ccc;
}

@media only screen and (max-width: 768px) {
  .slide-content {
    flex-direction: column;
    align-items: center;
    min-height: auto;
  }

  .column {
    width: 100%;
  }

  .menu-section-title h2 {
    font-size: 16pt;
  }

  .dish {
    height: 125px;
    gap: 10px;
  }

  .dish-details {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    gap: 5px;
    border-bottom: 1px solid #333;
    padding-bottom: 10px;
  }

  .dish-details h3,
  .dish-details p {
    text-align: start;
    width: 100%;
    font-size: 12pt;
  }

  .dish-details p {
    width: fit-content;
  }

  .dish-description {
    text-align: start;
    width: 90%;
    font-size: 11pt;
    text-align: justify;
    overflow-wrap: break-word;
  }
}
</style>
