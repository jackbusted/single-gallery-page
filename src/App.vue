<template>
    <div id="app">
        <main class="gallery-page">

            <!-- Header -->
            <header class="gallery-header">
                <h1>Our Gallery</h1>
                <div class="ornament">
                    <span></span>
                </div>
            </header>

            <!-- Gallery -->
            <section class="gallery" :style="{ gridTemplateColumns: `repeat(${columns}, 1fr)` }">
                <div v-for="(image, index) in images" :key="index" class="gallery-item" @click="openLightbox(index)">
                    <img :src="image" :alt="`Prewedding photo ${index + 1}`" loading="lazy" />
                </div>
            </section>

            <!-- Slider -->
            <div class="slider-wrapper">
                <input v-model.number="columns" type="range" min="1" max="4" step="1" class="gallery-slider" />
                <div class="slider-labels">
                    <span>Large</span>
                    <span>Small</span>
                </div>
            </div>

            <!-- Lightbox -->
            <vue-easy-lightbox :visible="lightboxVisible" :imgs="images" :index="lightboxIndex" @hide="closeLightbox" />
        </main>
    </div>
</template>

<script>
import VueEasyLightbox from 'vue-easy-lightbox'

export default {
    name: 'App',
    components: {
        VueEasyLightbox
    },
    data() {
        return {
            columns: 2,
            lightboxVisible: false,
            lightboxIndex: 0,
            images: [
                `${process.env.BASE_URL}images/image-1.jpeg`,
                `${process.env.BASE_URL}images/image-2.jpeg`,
                `${process.env.BASE_URL}images/image-3.jpeg`,
                `${process.env.BASE_URL}images/image-4.jpeg`,
                `${process.env.BASE_URL}images/image-5.jpeg`,
                `${process.env.BASE_URL}images/image-6.jpeg`,
                `${process.env.BASE_URL}images/image-7.jpeg`,
                `${process.env.BASE_URL}images/image-8.jpeg`,
                `${process.env.BASE_URL}images/image-9.jpeg`,
                `${process.env.BASE_URL}images/image-10.jpeg`,
                `${process.env.BASE_URL}images/image-11.jpeg`,
                `${process.env.BASE_URL}images/image-12.jpeg`
            ]
        }
    },
    methods: {
        openLightbox(index) {
            this.lightboxIndex = index
            this.lightboxVisible = true
        },
        closeLightbox() {
            this.lightboxVisible = false
        }
    }
}
</script>

<style>
* {
    box-sizing: border-box;
}

html,
body {
    margin: 0;
    padding: 0;
    background: #ffffff;
}

body {
    font-family:
        -apple-system,
        BlinkMacSystemFont,
        "Segoe UI",
        sans-serif;
}

#app {
    min-height: 100vh;
}

/* =========================
   PAGE
========================= */

.gallery-page {
    width: 100%;
    max-width: 1100px;
    margin: 0 auto;
    padding: 35px 20px 60px;
}

/* =========================
   HEADER
========================= */

.gallery-header {
    text-align: center;
    margin-bottom: 35px;
}

.gallery-header h1 {
    margin: 0;
    font-family: Georgia, serif;
    font-size: 48px;
    font-weight: 400;
    letter-spacing: 1px;
    color: #222;
}

/* Ornament */

.ornament {
    width: 100%;
    margin-top: 30px;
    position: relative;
    height: 15px;
}

.ornament::before {
    content: "";
    position: absolute;
    left: 0;
    right: 0;
    top: 7px;
    height: 1px;
    background: #222;
}

.ornament span {
    position: absolute;
    left: 50%;
    top: 1px;
    width: 14px;
    height: 14px;
    background: white;
    border: 1px solid #222;
    transform: translateX(-50%) rotate(45deg);
}

/* =========================
   GALLERY
========================= */

.gallery {
    display: grid;
    gap: 14px;
    width: 100%;
    transition: grid-template-columns 0.3s ease;
}

.gallery-item {
    width: 100%;
    overflow: hidden;
    cursor: pointer;
    background: #f3f3f3;
    aspect-ratio: 1 / 1;
}

.gallery-item img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;

    transition:
        transform 0.35s ease,
        opacity 0.35s ease;
}

.gallery-item:hover img {
    transform: scale(1.03);
    opacity: 0.92;
}

/* =========================
   SLIDER
========================= */

.slider-wrapper {
    margin-top: 35px;
    width: 100%;
}

.gallery-slider {
    display: block;
    width: 100%;
    cursor: pointer;
}

.slider-labels {
    display: flex;
    justify-content: space-between;
    margin-top: 8px;

    font-size: 12px;
    color: #777;
}

/* =========================
   MOBILE
========================= */

@media (max-width: 600px) {
    .gallery-page {
        padding: 25px 15px 50px;
    }

    .gallery-header {
        margin-bottom: 25px;
    }

    .gallery-header h1 {
        font-size: 38px;
    }

    .gallery {
        gap: 8px;
    }
}
</style>