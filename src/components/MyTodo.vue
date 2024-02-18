<script setup lang="ts">
defineProps({
  text: String,
  checked: Boolean,
});
const emit = defineEmits(["onClickDelete", "onClickCheck"]);
</script>

<template>
  <div class="todo">
    <div @click="emit('onClickCheck')" class="check">
      <img v-if="checked" src="/Vector.svg" alt="" />
    </div>

    <span class="text">{{ text }}</span>
    <img
      @click="emit('onClickDelete')"
      class="delete"
      src="/close.svg"
      alt=""
    />
  </div>
</template>

<style scoped>
.delete {
  cursor: pointer;
  margin-right: 10px;
  width: 24px;
  height: 24px;
  opacity: 0.6;
  transition: 0.5s;
}
.delete:hover {
  opacity: 1;
}
.text {
  text-align: center;
  flex: 1;
  font-weight: normal;
  font-size: 12px;
}
.todo {
  display: flex;
  align-items: center;
  background-color: var(--soft-color);
  border-radius: 5px;
  width: 100%;
  min-height: 55px;
  padding-left: 20px;
}
.check {
  cursor: pointer;
  width: 24px;
  height: 24px;
  background-color: var(--soft-color);
  border: 1px solid var(--secondary-color);
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.6;
  transition: 0.5s;
}
.check:hover {
  opacity: 1;
}
</style>
