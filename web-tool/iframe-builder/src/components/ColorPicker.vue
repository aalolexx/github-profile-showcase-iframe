<script setup>
import { computed } from "vue";

const props = defineProps({
  modelValue: { type: String, default: "" },
  id: { type: String, default: "" },
  placeholder: { type: String, default: "#rrggbb" },
});
const emit = defineEmits(["update:modelValue"]);

const normalized = computed({
  get() {
    const v = (props.modelValue || "").trim();
    return /^[0-9a-f]{3,8}$/i.test(v) ? `#${v}` : v;
  },
  set(v) {
    v = (v || "").trim();
    if (/^[0-9a-f]{3,8}$/i.test(v)) v = `#${v}`;
    emit("update:modelValue", v);
  },
});
</script>

<template>
  <div class="input-group">
    <input
      type="color"
      class="form-control form-control-color"
      :id="id"
      :value="normalized"
      @input="normalized = $event.target.value"
      title="Choose color"
      style="width: 3rem; padding: .25rem;"
    />
    <input
      type="text"
      class="form-control"
      :value="normalized"
      :placeholder="placeholder"
      @input="normalized = $event.target.value"
      spellcheck="false"
    />
  </div>
</template>

<style>
.form-control-color {
  display: inline-block;
  flex-grow: 0 !important;
}
</style>
