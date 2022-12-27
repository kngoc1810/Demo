<template>
  <div class="custom-select" @blur="open = false">
    <div class="selected" :class="{ open: open }" @click="open = !open">
      {{ selected }}
    </div>
    <div class="items" ref="itemContainerRef" :class="{ selectHide: !open }">
      <div
          v-for="(option, i) of options"
          :key="i"
          @click="
            selected = option;
            open = false;
          "
          ref="itemsRefs"
        >
          {{ option }}
        </div>
    </div>
  </div>
</template>

<script setup>
  import { ref, onMounted, watch, computed, nextTick } from "vue";

  const options = ref(["go", "python", "rust", "javascript", "ios", "test1", "test2"]);
  const selected = ref(null);
  const open = ref(false);
  const itemsRefs = ref(null);
  const itemContainerRef = ref(null);

  watch(open, (newVal) => {
    if (newVal) {
      const lastItem = itemsRefs.value[5];
      nextTick(() => {
        lastItem.scrollIntoView({ behavior: "smooth" });
      })
    }
  });
</script>

<style scoped>
  .custom-select {
    position: relative;
    width: 100%;
    text-align: left;
    outline: none;
    height: 47px;
    line-height: 47px;
  }

  .custom-select .selected {
    border-radius: 6px;
    border: 1px solid #666666;
    padding-left: 1em;
    cursor: pointer;
    user-select: none;
    height: 47px;
  }

  .custom-select .selected.open {
    border: 1px solid #ad8225;
    border-radius: 6px 6px 0px 0px;
  }

  .custom-select .selected:after {
    position: absolute;
    content: "";
    top: 22px;
    right: 1em;
    width: 0;
    height: 0;
    border: 5px solid transparent;
    border-color: #fff transparent transparent transparent;
  }

  .custom-select .items {
    color: #fff;
    border-radius: 0px 0px 6px 6px;
    border-right: 1px solid #ad8225;
    border-left: 1px solid #ad8225;
    border-bottom: 1px solid #ad8225;
    position: absolute;
    background-color: #0a0a0a;
    left: 0;
    right: 0;
    z-index: 1;
    height: 100px;
    overflow: auto;
  }

  .custom-select .items div {
    color: #fff;
    padding-left: 1em;
    cursor: pointer;
    user-select: none;
  }

  .custom-select .items div:hover {
    background-color: #ad8225;
  }

  .selectHide {
    display: none;
  }
</style>
