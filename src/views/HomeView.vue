<script setup lang="ts">
import { ref } from "vue";

const dotContainer = ref<HTMLElement | null>(null);
const isAnimate = ref(false);
const dotCount = ref(20);

const emitParticles = async () => {
  isAnimate.value = true;

  requestAnimationFrame(async () => {
    if (!dotContainer.value) return;
    const children = [...dotContainer.value.children] as HTMLElement[];

    const animations = children.map((child, index) => {
      const angle = Math.random() * 360;
      const dist = 50 + Math.random() * 100;
      const size = 0.5 + Math.random() * 2;
      const hue = Math.random() * 360;

      child.style.backgroundColor = `hsl(${hue}, 30%, 50%`;

      if (Math.random() < 0.5) {
        child.style.mixBlendMode = "add";
      }
      if (Math.random() < 0.5) {
        child.style.filter = `blur(${Math.random() * 20}px)`;
      }

      return child.animate(
        [
          {
            opacity: 0,
            transform: `rotate(${angle}deg) translateX(0px) scale(1)`,
          },
          {
            transform: `rotate(${angle}deg) translateX(${
              dist * 2
            }px) scale(${size})`,
            opacity: 0.5,
            offset: 0.3,
          },
          {
            opacity: 1,
            transform: `rotate(${
              angle + Math.random() * 360
            }deg) translateX(${dist}px) scale(${size})`,
            offset: 0.8,
          },
          {
            opacity: 0.2,
          },
        ],
        {
          duration: 1000 * Math.random() + 1000,
          iterations: Infinity,
          // fill: "forwards",
          // composite: "accumulate",
        }
      );
    });

    await Promise.all(animations.map((anim) => anim.finished));
    isAnimate.value = false;
  });
};
</script>

<template>
  <div class="stage" @click="emitParticles">
    <div>😀</div>
    <div ref="dotContainer" v-if="isAnimate" class="dotContainer">
      <div v-for="dot in dotCount" :key="dot" class="dot"></div>
    </div>
  </div>

  <input type="range" v-model.number="dotCount" min="1" max="100" step="1" />
  {{ dotCount }}
</template>

<style scoped>
.stage {
  display: flex;
  width: 400px;
  height: 400px;
  position: relative;
  cursor: pointer;
  overflow: hidden;
  border: 1px solid #ffffff88;
  background-color: #05142b;
  align-items: center;
  justify-content: center;
}
.dotContainer {
  position: absolute;
}
.dot {
  position: absolute;
  width: 20px;
  height: 20px;
  left: -10px;
  top: -10px;

  border-radius: 25%;
}
</style>
