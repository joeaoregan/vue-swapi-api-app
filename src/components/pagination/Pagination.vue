<script setup>
import { computed, ref, watch, onMounted, onUnmounted } from "vue";

const props = defineProps({
  currentPage: Number,
  totalPages: Number,
  pageSize: Number,
  pageSizes: {
    type: Array,
    default: () => [5, 10, 20, 50],
  },
});

const emit = defineEmits(["update:currentPage", "update:pageSize"]);

const localPageSize = ref(props.pageSize);

watch(
  () => props.pageSize,
  (newVal) => {
    localPageSize.value = newVal;
  },
);

const visiblePages = computed(() => {
  const pages = [];
  const total = props.totalPages;
  const current = props.currentPage;

  pages.push(1);

  if (current > 3) pages.push("…");

  for (let i = current - 1; i <= current + 1; i++) {
    if (i > 1 && i < total) pages.push(i);
  }

  if (current < total - 2) pages.push("…");

  if (total > 1) pages.push(total);

  return pages;
});

function goToPage(page) {
  if (page !== "…" && page >= 1 && page <= props.totalPages) {
    emit("update:currentPage", page);
  }
}

function nextPage() {
  if (props.currentPage < props.totalPages) {
    emit("update:currentPage", props.currentPage + 1);
  }
}

function prevPage() {
  if (props.currentPage > 1) {
    emit("update:currentPage", props.currentPage - 1);
  }
}

function updatePageSize() {
  emit("update:pageSize", localPageSize.value);
}

// Keyboard navigation
function handleKeydown(event) {
  if (event.key === "ArrowLeft") {
    prevPage();
  } else if (event.key === "ArrowRight") {
    nextPage();
  }
}

onMounted(() => {
  window.addEventListener("keydown", handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener("keydown", handleKeydown);
});
</script>

<template>
  <div class="pagination">
    <button @click="prevPage" :disabled="currentPage === 1">Previous</button>

    <button
      v-for="page in visiblePages"
      :key="page + '-btn'"
      @click="goToPage(page)"
      :disabled="page === '…'"
      :class="{ active: currentPage === page }"
    >
      {{ page }}
    </button>

    <span>Page {{ currentPage }} of {{ totalPages }}</span>

    <button @click="nextPage" :disabled="currentPage === totalPages">
      Next
    </button>

    <select v-model.number="localPageSize" @change="updatePageSize">
      <option v-for="size in pageSizes" :key="size" :value="size">
        {{ size }}
      </option>
    </select>
  </div>
</template>

<style scoped src="./style.css"></style>
