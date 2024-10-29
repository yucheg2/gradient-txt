<template>
  <div class="infinite-scroll" ref="scrollContainer">
    <slot />
    <div ref="trigger"></div>
    <div :style="{ opacity: isLoading ? 1 : 0 }" class="loading">
      Загрузка...
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, defineProps, defineEmits } from "vue";

const props = defineProps({
  isLoading: {
    type: Boolean,
    default: false,
  },
});

const emits = defineEmits(["scrolledToEnd"]);

const trigger = ref(null);
const scrollContainer = ref(null);

const observer = new IntersectionObserver(
  (entries) => {
    let entry = entries[0];
    if (entry.isIntersecting) {
      emits("scrolledToEnd");
    }
  },
  { root: scrollContainer.value }
);

const handleScroll = () => {
  if (props.isLoading) return;
  observer.observe(trigger.value);
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

onBeforeUnmount(() => {
  observer.disconnect();
  window.removeEventListener("scroll", handleScroll);
});
</script>

<style scoped>
.infinite-scroll {
  height: 100%;
  width: 100%;
  overflow: auto;
}
.infinite-scroll-container .loading {
  text-align: center;
  padding: 20px;
}
</style>
