<template>
    <div class="page">

        <!-- Header -->
        <section class="header">
            <h1>Our Gallery</h1>
            <div class="ornament">
                <span></span>
            </div>
        </section>

        <!-- Gallery -->
        <section class="gallery" :style="{ columnCount: columns }">
            <div v-for="(image, index) in images" :key="image.name" class="gallery-item" @click="openLightbox(index)">
                <img :src="image.thumbnail" :alt="image.alt" loading="lazy" decoding="async">
            </div>
        </section>

        <!-- Slider -->
        <div class="gallery-control">
            <span class="control-label">+</span>
            <input
                v-model.number="columns"
                type="range"
                min="1"
                max="4"
                step="1"
                aria-label="Gallery size"
            >
            <span class="control-label">-</span>
        </div>

        <!-- Lightbox -->
        <vue-easy-lightbox :visible="visible" :imgs="lightboxImages" :index="lightboxIndex" @hide="visible = false" />
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
            visible: false,
            lightboxIndex: 0,
            imageCount: 12, // edit here
        }
    },
    mounted() {
        this.setTitle()
    },
    methods: {
        setTitle() {
            document.title = `Satrio & Sabilla's Wedding`
        },
        openLightbox(index) {
            this.lightboxIndex = index
            this.visible = true
        },
    },
    computed: {
        images() {
            return Array.from(
                { length: this.imageCount },
                (_, index) => {
                    const name = `image-${index + 1}`
                    return {
                        name,
                        thumbnail: `${process.env.BASE_URL}images/thumbnails/${name}.webp`,
                        original: `${process.env.BASE_URL}images/${name}.jpeg`,
                        alt: `Prewedding photo ${index + 1}`
                    }
                }
            )
        },
        lightboxImages() {
            return this.images.map(image => {
                return {
                    src: image.original,
                    title: image.alt
                }
            })
        }
    },
}
</script>

<style>
* {
    box-sizing: border-box;
}

html, body {
    margin: 0;
    padding: 0;
}
body {
    margin: 0;
    color: #1c325b;
    font-family: "Montserrat", sans-serif;
    background-color: #D4E2D4;
    background-image:
        radial-gradient(
            rgba(101, 174, 125, 0.305) 0.8px,
            transparent 0.8px
        ),
        radial-gradient(
            rgba(174, 112, 255, 0.327) 0.8px,
            transparent 0.8px
        );
    background-size: 9px 9px, 13px 13px;
    background-position: 0 0, 4px 6px;
}
.page {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 60px 30px 120px;
}

/* =========================
     HEADER
  ========================= */

.header {
    text-align: center;
    margin-bottom: 40px;
}
.header h1 {
    margin: 0;
    font-family: "Cormorant Garamond", Georgia, serif;
    font-size: 46px;
    font-weight: 400;
    letter-spacing: 2px;
    color: #3F5145;
}

/* =========================
   ORNAMENT
========================= */

.ornament {
    width: 100%;
    margin-top: 30px;
    position: relative;
    height: 15px;
    color: #9A8FA8;
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
     MASONRY GALLERY
  ========================= */

.gallery {
    column-gap: 14px;
}
.gallery-item {
    width: 100%;
    margin-bottom: 14px;
    break-inside: avoid;
    -webkit-column-break-inside: avoid;
    cursor: pointer;
    overflow: hidden;
    border-radius: 10px;
}
.gallery-item img {
    display: block;
    width: 100%;
    height: auto;
    border-radius: 10px;
    transition:
        transform 0.3s ease,
        opacity 0.3s ease;
}
.gallery-item:hover img {
    opacity: 0.92;
    transform: scale(1.015);
}

/* =========================
     SLIDER
  ========================= */

.gallery-control {
    position: fixed;
    left: 50%;
    bottom: 24px;
    transform: translateX(-50%);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 14px;
    padding: 10px 18px;
    background: rgba(244, 241, 232, 0.88);
    border: 1px solid rgba(154, 143, 168, 0.442);
    border-radius: 999px;
    box-shadow: 0 6px 24px rgba(63, 81, 69, 0.12);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    z-index: 1000;
}
.gallery-control input[type="range"] {
    appearance: none;
    -webkit-appearance: none;
    width: 160px;
    height: 4px;
    border-radius: 999px;
    background: #cc92ff;
    outline: none;
    cursor: pointer;
}
.gallery-control input[type="range"]::-webkit-slider-thumb {
    appearance: none;
    -webkit-appearance: none;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: #647565;
    border: 3px solid #F4F1E8;
    box-shadow: 0 2px 6px rgba(63, 81, 69, 0.25);
    cursor: pointer;
}
.gallery-control input[type="range"]::-moz-range-thumb {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: #647565;
    border: 3px solid #F4F1E8;
    box-shadow: 0 2px 6px rgba(63, 81, 69, 0.25);
    cursor: pointer;
}
.control-label {
    font-family: "Montserrat", sans-serif;
    font-size: 18px;
    font-weight: 400;
    color: #9A8FA8;
    user-select: none;
}

/* =========================
     MOBILE
  ========================= */

@media (max-width: 600px) {
    .page {
        padding: 40px 15px 100px;
    }
    .header {
        margin-bottom: 30px;
    }
    .header h1 {
        font-size: 38px;
        letter-spacing: 1.5px;
    }
    .gallery {
        column-gap: 8px;
    }
    .gallery-item {
        margin-bottom: 8px;
    }
    .gallery-control {
        bottom: 16px;
        padding: 9px 14px;
        gap: 10px;
    }
    .gallery-control input[type="range"] {
        width: 130px;
    }
    .control-label {
        font-size: 17px;
    }
}

:root {
    --matcha-bg: #F4F1E8;
    --matcha-soft: #E8EBDD;
    --matcha: #647565;
    --matcha-dark: #3F5145;
    --taro: #9A8FA8;
    --taro-soft: #C8C0CF;
    --divider: #A8B2A1;
}
</style>