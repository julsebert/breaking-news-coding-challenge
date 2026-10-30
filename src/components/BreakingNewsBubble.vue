<script setup lang="ts">
import { ref, reactive, useId, onUnmounted } from 'vue';
import type { NewsItem } from '../types/NewsItem';
import BreakingNewsMenu from './BreakingNewsMenu.vue';

const props = defineProps<{
    news: readonly NewsItem[];
}>();

const position = reactive({ x: 20, y: 20 });
const menuId = `breaking-news-menu-${useId()}`;

const bubbleRef = ref<HTMLButtonElement | null>(null);

const DRAG_THRESHOLD = 5;

let isDragging = false;
let hasDragged = false;

let startX = 0;
let startY = 0;
let offsetX = 0;
let offsetY = 0;

// Prevents toggling after dragging or when no news is available.
function onBubbleClick(event: MouseEvent): void {
    if (props.news.length === 0 || (event.detail !== 0 && hasDragged)) {
        event.preventDefault();
    }
    hasDragged = false;
}

// Initializes dragging with the left mouse button.
function onMouseDown(event: MouseEvent): void {
    if (event.button !== 0) return;

    isDragging = true;
    hasDragged = false;

    startX = event.clientX;
    startY = event.clientY;

    offsetX = event.clientX - position.x;
    offsetY = event.clientY - position.y;

    window.addEventListener('mousemove', onMouseMove);
    window.addEventListener('mouseup', onMouseUp);
}

// Moves the bubble within the viewport after the drag threshold is reached.
function onMouseMove(event: MouseEvent): void {
    if (!isDragging) return;

    const distance = Math.hypot(
        event.clientX - startX,
        event.clientY - startY
    );

    if (!hasDragged && distance < DRAG_THRESHOLD) return;

    hasDragged = true;

    const bubble = bubbleRef.value;
    if (!bubble) return;

    const maxX = Math.max(0, window.innerWidth - bubble.offsetWidth);
    const maxY = Math.max(0, window.innerHeight - bubble.offsetHeight);

    position.x = Math.max(0, Math.min(event.clientX - offsetX, maxX));
    position.y = Math.max(0, Math.min(event.clientY - offsetY, maxY));
}

// Ends dragging and removes mouse listeners.
function onMouseUp(): void {
    isDragging = false;

    window.removeEventListener('mousemove', onMouseMove);
    window.removeEventListener('mouseup', onMouseUp);
}

// Clean up event listeners if the component is unmounted during dragging.
onUnmounted(() => {
    window.removeEventListener('mousemove', onMouseMove);
    window.removeEventListener('mouseup', onMouseUp);
});
</script>

<template>
    <div
        class="breaking-news"
        :style="{
            left: `${position.x}px`,
            top: `${position.y}px`
        }"
    >
        <button
            ref="bubbleRef"
            type="button"
            class="news-bubble"
            :popovertarget="news.length > 0 ? menuId : undefined"
            :aria-label="`Breaking News, ${news.length} news items`"
            @click="onBubbleClick"
            @mousedown="onMouseDown"
        >
            {{ news.length }}
        </button>

        <BreakingNewsMenu
            v-if="news.length > 0"
            :id="menuId"
            popover="auto"
            :news="news"
        />
    </div>
</template>

<style scoped>
.news-bubble {
    width: 56px;
    height: 56px;
    border: 4px solid #007980;
    border-radius: 50%;
    background-color: white;
    font-size: 24px;
    font-weight: 600;
    color: #0B0C10;
    cursor: pointer;
    user-select: none;
}

.news-bubble:hover {
    background-color: #ecfeff;
}

.news-bubble:focus-visible {
    outline: 3px solid #007980;
    outline-offset: 3px;
}

.breaking-news {
    position: fixed;
    width: 56px;
    height: 56px;
    z-index: 1000;
}

.breaking-news :deep(.news-menu) {
    position: fixed;
    inset: auto;
    position-anchor: auto;
    position-area: right;
    position-try-fallbacks: flip-inline, top, bottom, top left, bottom left;
    margin: 8px;
    width: max-content;
    max-width: min(320px, calc(100vw - 16px));
    box-sizing: border-box;
}
</style>
