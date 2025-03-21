<script setup lang="ts">
import { ref } from 'vue';
import useCursorMove from 'hooks/useCursorMove';

const cursorDefaultRadius = 5;

const containerRef = ref<HTMLDivElement | null>(null);
const cursorRef = ref<HTMLDivElement | null>(null);
const {
  showCursor,
  cursorSize,
  cursorBigRadius,
  onMouseEnter,
  onMouseMove,
  onMouseLeave,
} = useCursorMove(containerRef, cursorRef, cursorDefaultRadius);
</script>

<template>
  <div
    class="firewormButton"
    @mouseenter="onMouseEnter"
    @mousemove="onMouseMove"
    @mouseleave="onMouseLeave"
    ref="containerRef"
  >
    <div class="content">
      <div class="dot dot1" />
      <div class="dot dot2" />
      <div class="dot dot3" />
      <div class="dot dot4" />
      <div class="dot dot5" />
      <div class="dot dot6" />
      <div class="dot dot7" />
      <div class="buttonWrapper">
        <span>Button</span>
      </div>
    </div>
    <div
      class="cursor"
      :style="{
        visibility: showCursor ? 'visible' : 'hidden',
        width: `${
          cursorSize === 'big' ? cursorBigRadius * 2 : cursorDefaultRadius * 2
        }px`,
        height: `${
          cursorSize === 'big' ? cursorBigRadius * 2 : cursorDefaultRadius * 2
        }px`,
      }"
      ref="cursorRef"
    />
  </div>
</template>

<style scoped lang="scss">
.firewormButton {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;

  .content {
    $primary-color: #f4cf47;
    $border-color: #000;
    $buttonHeight: 54px;

    box-sizing: border-box;
    width: 180px;
    height: $buttonHeight;
    cursor: pointer;
    position: relative;

    .dot {
      position: absolute;
      transition: transform calc(var(--speed) / 14) ease;
      width: var(--size);
      height: var(--size);
      transform: translate(var(--starting-x), var(--starting-y));
      z-index: 2;

      &::after {
        content: '';
        animation: hoverFirefly var(--speed) infinite,
          hoverFireFlicker calc(var(--speed) / 3) infinite
            calc(var(--speed) / 3);
        animation-play-state: paused;
        display: block;
        border-radius: 100%;
        background: yellow;
        width: 100%;
        height: 100%;
        box-shadow: 0px 0px 6px 0px #ffeb3b, 0px 0px 4px 0px #fdfca9 inset,
          0px 0px 2px 1px #ffffff42;
      }
    }
    .dot1 {
      --speed: 14s;
      --size: 6px;
      --starting-x: 30px;
      --starting-y: 20px;
      top: 3px;
      left: -14px;
      opacity: 0.7;
    }
    .dot2 {
      --speed: 16s;
      --size: 3px;
      --starting-x: 40px;
      --starting-y: 10px;
      top: 0px;
      left: -4px;
      opacity: 0.7;
    }
    .dot3 {
      --speed: 20s;
      --size: 4px;
      --starting-x: -10px;
      --starting-y: 20px;
      top: -8px;
      right: 14px;
    }
    .dot4 {
      --speed: 18s;
      --size: 2px;
      --starting-x: -30px;
      --starting-y: -5px;
      bottom: 4px;
      right: -14px;
      opacity: 0.9;
    }
    .dot5 {
      --speed: 22s;
      --size: 5px;
      --starting-x: -40px;
      --starting-y: -20px;
      bottom: -6px;
      right: -3px;
    }
    .dot6 {
      --speed: 15s;
      --size: 4px;
      --starting-x: 12px;
      --starting-y: -18px;
      bottom: -12px;
      left: 30px;
      opacity: 0.7;
    }
    .dot7 {
      --speed: 19s;
      --size: 3px;
      --starting-x: 6px;
      --starting-y: -20px;
      bottom: -16px;
      left: 44px;
    }

    .buttonWrapper {
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
      border-radius: 30px;
      width: 100%;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
      background-color: $primary-color;
      border: 3px solid $border-color;
      z-index: 11;

      span {
        font-size: 24px;
        letter-spacing: 2px;
        color: $border-color;
        transition: all 0.3s ease;
      }

      &::before,
      &::after {
        content: '';
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        border-radius: calc($buttonHeight / 2);
        opacity: 0;
        transition: opacity 0.3s;
      }

      &::before {
        box-shadow: 0px 0px 24px 0px #ffeb3b;
      }
      &::after {
        box-shadow: 0px 0px 23px 0px #fdfca9 inset, 0px 0px 8px 0px #ffffff42;
      }
    }

    &:hover {
      .dot {
        transform: translate(0, 0);
        &::after {
          animation-play-state: running;
        }
      }
      .buttonWrapper {
        span {
          letter-spacing: 4px;
        }
        &::before,
        &::after {
          opacity: 1;
        }
      }
    }
  }

  .cursor {
    position: absolute;
    border-radius: 50%;
    background-color: #ffeb3b;
    mix-blend-mode: difference;
    pointer-events: none;
    transition: width 0.4s, height 0.4s;
    transform-origin: 50% 50% 0;
  }

  @keyframes hoverFirefly {
    0% {
      transform: translate(0, 0);
    }
    12% {
      transform: translate(4px, 2px);
    }
    24% {
      transform: translate(-3px, 4px);
    }
    37% {
      transform: translate(3px, -3px);
    }
    55% {
      transform: translate(-2px, 0);
    }
    74% {
      transform: translate(0, 3px);
    }
    88% {
      transform: translate(-4px, -2px);
    }
    100% {
      transform: translate(0, 0);
    }
  }

  @keyframes hoverFireFlicker {
    0% {
      opacity: 1;
    }
    25% {
      opacity: 0.3;
    }
    50% {
      opacity: 0.8;
    }
    75% {
      opacity: 0.4;
    }
    100% {
      opacity: 1;
    }
  }
}
</style>
