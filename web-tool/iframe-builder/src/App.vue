<script setup>
import { reactive, computed, onMounted } from "vue";
import ColorPicker from "./components/ColorPicker.vue";

const form = reactive({
  baseUrl: "https://ehrencreative.de/github-profile-showcase/iframer/",
  username: "aalolexx",
  width: "100%",
  height: "500",
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
    <h1 class="mb-2">GitHub Profile Card - Iframe Generator</h1>
    <p class="mb-4">Showcase your GitHub Profile by simply pasting the iframe code to your page.
      The Iframe will automatically fetch the GitHub API and display your profile informations.
    </p>

    <form class="row g-3 mb-4">
      <div class="col-md-6">
        <label class="form-label"><strong>Your GitHub Username</strong></label>
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

    <div class="accordion accordion-flush mb-4" id="flushAccordion">
      <div class="accordion-item">
        <h2 class="accordion-header" id="flushHeading1">
          <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse"
            data-bs-target="#flushPanel1" aria-expanded="false" aria-controls="flushPanel1">
            <span>Configure Look and Feel</span>
            <span>
              <div class="preview-color" :style="{'background-color': form.color1}"></div>
              <div class="preview-color" :style="{'background-color': form.color2}"></div>
              <div class="preview-color" :style="{'background-color': form.colorText}"></div>
              <div class="preview-color" :style="{'background-color': form.colorSecondary}"></div>
              <div class="preview-color" :style="{'background-color': form.colorBackground}"></div>
              <div class="preview-color" :style="{'background-color': form.colorBodyBackground}"></div>
            </span>
          </button>
        </h2>
        <div id="flushPanel1" class="accordion-collapse collapse" aria-labelledby="flushHeading1"
          data-bs-parent="#flushAccordion">
          <div class="accordion-body">
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
          </div>
        </div>
      </div>
    </div>

    <p><strong>Iframe code to copy</strong></p>
    <pre class="rounded"><code>{{ embedCode }}</code></pre>
    <!--<div class="col-12 d-flex gap-2">
      <button class="btn btn-primary" @click="copyCode">Copy embed code</button>
    </div>-->

    <h2 class="mb-4">Iframe preview</h2>

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

.accordion-button {
  border-radius: var(--bs-border-radius) !important;
  padding: .75rem .75rem;
  display: flex;
  justify-content: space-between;
}

.accordion-button span {
  display: flex;
  margin-right: 2em;
}

.accordion-button:hover {
  color: #ff6b94;
}

.accordion-button::after {
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23fff'%3e%3cpath fill-rule='evenodd' d='M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z'/%3e%3c/svg%3e");
}

.accordion .accordion-button:not(.collapsed)::after {
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23575df0'%3e%3cpath fill-rule='evenodd' d='M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z'/%3e%3c/svg%3e");
}

.accordion-button.collapsed {
  background-color: #19191e;
}

.accordion-button:not(.collapsed) {
  color: #575df0;
  background-color: #19191e;
  box-shadow: none !important;
  border-bottom-left-radius: 0 !important;
  border-bottom-right-radius: 0 !important;
}

.preview-color {
  width: 2em;
  height: 1em;
  transition: 0.3s;
}

.accordion-button:not(.collapsed) .preview-color  {
  width: 0;
}

.accordion-body {
  border: 4px solid #19191e;
}

.container {
  max-width: 1180px;
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