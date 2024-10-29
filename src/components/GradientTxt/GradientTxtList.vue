<template>
  <InfiniteScroll :is-loading="loading" @scrolled-to-end="loadMore">
    <div class="gradient-list">
      <template v-for="comment in textArr" :key="comment.date">
        <GradientTxt
          :content="comment.content"
          :content-post-tones="comment.contentPostTones"
        />
        <hr class="gradient-list__divider" />
      </template>
    </div>
  </InfiniteScroll>
</template>

<script setup>
import { defineProps, ref } from "vue";
import GradientTxt from "./GradientTxt.vue";
import InfiniteScroll from "../common/InfiniteScroll.vue";

const props = defineProps({
  list: {
    type: Array,
    required: true,
  },
});

let paginationStep = 10;

const textArr = ref(props.list.slice(0, paginationStep));
const loading = ref(false);

const loadMore = async () => {
  loading.value = true;

  try {
    const result = await new Promise((resolve) => {
      setTimeout(() => {
        paginationStep += 10;
        resolve(props.list.slice(0, paginationStep));
      }, 5000);
    });

    textArr.value = result;
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false;
  }

  textArr;
};
</script>

<style>
.gradient-list__divider {
  margin-bottom: 40px;
}
</style>
