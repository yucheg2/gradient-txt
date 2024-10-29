<template>
  <div class="gradient-txt">
    <template v-for="(postTone, index) in contentPostTones" :key="index">
      <span v-if="postTone.position > 0">
        {{
          content.slice(
            contentPostTones[index - 1]
              ? contentPostTones[index - 1]?.length +
                  contentPostTones[index - 1]?.position
              : 0,
            postTone.position
          )
        }}
      </span>
      <span :style="{ background: getGradientStyle(postTone.tone) }">
        {{
          content.slice(postTone.position, postTone.position + postTone.length)
        }}
      </span>
      <span v-if="index === contentPostTones.length -1">
        {{
          content.slice(postTone.position + postTone.length)
        }}
      </span>
    </template>
  </div>
</template>

<script setup>
import { defineProps } from "vue";
defineProps({
  content: {
    type: String,
    required: true,
  },
  contentPostTones: {
    type: Array,
    required: true,
  },
});

const getGradientStyle = (tone) => {
  const color1 = tone < 0 ? "red" : tone === 1 ? "green" : "yellow";
  const color2 = tone === -1 ? "red" : tone <= 0 ? "yellow" : "green";

  const tonePercent = (tone < 0 ? 1 + tone : tone) * 100;
  const gradient = `color-mix(in oklab, ${color1}, ${color2} ${tonePercent}%)`;
  return gradient;
};
</script>

<style>
.gradient-txt {
  text-align: left;
}
</style>
