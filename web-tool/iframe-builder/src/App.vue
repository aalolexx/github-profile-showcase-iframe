<script setup>
import { reactive, computed } from "vue";
import ColorPicker from "./components/ColorPicker.vue";

const form = reactive({
  baseUrl: "https://ehrencreative.de/github-profile-showcase/iframer/",
  username: "aalolexx",
  width: "100%",
  height: "400",
  border: false,

  // THEME VARS 
  fontFamily: "poppins, arial",
  cardBorderRadius: "2em",
  colorText: "#ffffff",
  color1: "#575df0",
  color2: "#ff6b94",
  colorSecondary: "#a8b0c0",
  colorBackground: "#19191e",
  colorBodyBackground: "#0d0d0d",
  baseFontSize: "15px",
});

function safeUrl(base) {
  try { return new URL(base); }
  catch { return new URL(base, location.origin); }
}

const themeParamMap = {
  "font-family": () => form.fontFamily,
  "card-border-radius": () => form.cardBorderRadius,
  "color-text": () => form.colorText,
  "color-1": () => form.color1,
  "color-2": () => form.color2,
  "color-secondary": () => form.colorSecondary,
  "color-background": () => form.colorBackground,
  "color-body-background": () => form.colorBodyBackground,
  "base-font-size": () => form.baseFontSize,
};

const embedSrc = computed(() => {
  const u = safeUrl(form.baseUrl);
  if (form.username?.trim()) u.searchParams.set("user", form.username.trim());
  Object.entries(themeParamMap).forEach(([key, getVal]) => {
    const v = String(getVal() ?? "").trim();
    if (v) u.searchParams.set(key, v);
  });
  return u.toString();
});

const embedCode = computed(() =>
  `<iframe src="${embedSrc.value}" width="${form.width}" height="${form.height}" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>`
);

async function copyCode() {
  try {
    await navigator.clipboard.writeText(embedCode.value);
    alert("Embed code copied!");
  } catch {
    prompt("Copy the embed code:", embedCode.value);
  }
}
</script>

<template>
  <div class="container py-4">
    <h1 class="h3 mb-4">Iframe Embed Builder</h1>

    <form class="row g-3">
      <div class="col-md-6">
        <label class="form-label">Username</label>
        <input v-model.lazy="form.username" class="form-control" placeholder="octocat" />
      </div>

      <div class="col-md-3">
        <label class="form-label">Width</label>
        <input v-model.lazy="form.width" class="form-control" placeholder="100%" />
      </div>

      <div class="col-md-3">
        <label class="form-label">Height</label>
        <input v-model.lazy="form.height" class="form-control" placeholder="700" />
      </div>
    </form>

    <h2 class="h6 mb-3">Theme</h2>

    <div class="row g-3">
      <div class="col-12 col-lg-6">
        <label class="form-label">Font family</label>
        <input v-model.lazy="form.fontFamily" class="form-control" placeholder="poppins, arial" />
      </div>

      <div class="col-6 col-lg-3">
        <label class="form-label">Base font size</label>
        <input v-model.lazy="form.baseFontSize" class="form-control" placeholder="15px" />
      </div>

      <div class="col-6 col-lg-3">
        <label class="form-label">Card border radius</label>
        <input v-model.lazy="form.cardBorderRadius" class="form-control" placeholder="2em" />
      </div>

      <div class="col-12 col-md-6 col-lg-2">
        <label class="form-label">Text color</label>
        <ColorPicker v-model.lazy="form.colorText" />
      </div>

      <div class="col-12 col-md-6 col-lg-2">
        <label class="form-label">Primary color 1</label>
        <ColorPicker v-model.lazy="form.color1" />
      </div>

      <div class="col-12 col-md-6 col-lg-2">
        <label class="form-label">Primary color 2</label>
        <ColorPicker v-model.lazy="form.color2" />
      </div>

      <div class="col-12 col-md-6 col-lg-2">
        <label class="form-label">Secondary color</label>
        <ColorPicker v-model.lazy="form.colorSecondary" />
      </div>

      <div class="col-12 col-md-6 col-lg-2">
        <label class="form-label">Card background</label>
        <ColorPicker v-model.lazy="form.colorBackground" />
      </div>

      <div class="col-12 col-md-6 col-lg-2">
        <label class="form-label">Body background</label>
        <ColorPicker v-model.lazy="form.colorBodyBackground" />
      </div>
    </div>

    <hr class="my-4" />

    <h2 class="h5">iframe code</h2>
    <pre class="rounded"><code>{{ embedCode }}</code></pre>
    <!--<div class="col-12 d-flex gap-2">
      <button class="btn btn-primary" @click="copyCode">Copy embed code</button>
    </div>-->

    <iframe :src="embedSrc" :width="form.width" :height="form.height" :style="iframeStyle" class="w-100"
      referrerpolicy="no-referrer-when-downgrade"></iframe>
  </div>
</template>

<style>
:root {
  /* brand palette */
  --bs-primary: #575df0;
  --bs-secondary: #a8b0c0;

  /* page colors */
  --bs-body-bg: #0d0d0d;
  --bs-body-color: #fff;

  --secondary-bg: #19191e;

  /* links (optional – by default they use primary) */
  --bs-link-color: var(--bs-primary);
  --bs-link-hover-color: #ff6b94;
}

.btn-primary {
  --bs-btn-bg: var(--bs-primary);
  --bs-btn-border-color: var(--bs-primary);
  --bs-btn-hover-bg: #ff6b94;
  --bs-btn-hover-border-color: #ff6b94;
}

.container {
  max-width: 1080px;
}

.form-control {
  border: none;
  background-color: var(--secondary-bg)
}

pre {
  background-color: var(--secondary-bg);
  color: #fff;
  font-family: monospace;
  padding: 1em 0.5em;
}
</style>