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
        <section class="gallery">
            <div
                v-for="(group, groupIndex) in imageGroups"
                :key="groupIndex"
                class="gallery-group"
                :class="`gallery-group--${group.length}`"
            >
                <div
                    v-for="(image, index) in group"
                    :key="image.name"
                    class="gallery-item"
                    :class="`gallery-item--${index + 1}`"
                    @click="openLightbox(image.originalIndex)"
                >
                    <img
                        :src="image.thumbnail"
                        :alt="image.alt"
                        loading="lazy"
                        decoding="async"
                    >
                </div>
            </div>
        </section>

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
            visible: false,
            lightboxIndex: 0,
            imageCount: 47, // edit here
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
                        originalIndex: index,
                        alt: `Prewedding photo ${index + 1}`
                    }
                }
            )
        },
        imageGroups() {
            const groups = []

            for (let i = 0; i < this.images.length; i += 5) {
                groups.push(this.images.slice(i, i + 5))
            }

            return groups
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
   GALLERY
========================= */

.gallery {
    width: 100%;
    max-width: 760px;
    margin: 0 auto;
}
.gallery-group {
    position: relative;
    width: 100%;
    aspect-ratio: 1 / 1.15;
    margin-bottom: 55px;
}

/* =========================
   GALLERY ITEM
========================= */

.gallery-item {
    position: absolute;
    overflow: hidden;
    cursor: pointer;
    border-radius: 10px;
    box-shadow: 0 4px 14px rgba(63, 81, 69, 0.10);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.gallery-item img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 7px;
    transition: transform 0.3s ease, opacity 0.3s ease;
}
.gallery-item:hover {
    box-shadow: 0 8px 22px rgba(63, 81, 69, 0.16);
}
.gallery-item:hover img {
    transform: scale(1.025);
    opacity: 0.94;
}

/* =========================
   FIVE IMAGE GROUP
   ========================= */

.gallery-group--5 .gallery-item--1 {
    top: 0;
    left: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--5 .gallery-item--2 {
    top: 0;
    right: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--5 .gallery-item--3 {
  top: 26%;
  left: 50%;
  width: 44%;
  height: 48%;
  transform: translateX(-50%);
  z-index: 3;

  /* frame */
  padding: 4px;
  box-sizing: border-box;
  background: #F4F1E8;
  border: 1px solid #D8D4C9;
  box-shadow: 0 6px 18px rgba(63, 81, 69, 0.16);
}
.gallery-group--5 .gallery-item--3:hover {
  transform: translateX(-50%) scale(1.015);
}
.gallery-group--5 .gallery-item--4 {
    bottom: 0;
    left: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--5 .gallery-item--5 {
    bottom: 0;
    right: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--5 .gallery-item--3:hover {
    transform: translateX(-50%) scale(1.015);
}

/* =========================
   FOUR IMAGE GROUP
========================= */

.gallery-group--4 .gallery-item--1 {
    top: 0;
    left: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--4 .gallery-item--2 {
    top: 0;
    right: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--4 .gallery-item--3 {
    bottom: 0;
    left: 0;
    width: 47%;
    height: 47%;
}
.gallery-group--4 .gallery-item--4 {
    bottom: 0;
    right: 0;
    width: 47%;
    height: 47%;
}

/* =========================
   THREE IMAGE GROUP
========================= */

.gallery-group--3 .gallery-item--1 {
    top: 0;
    left: 29%;
    width: 42%;
    height: 52%;
}
.gallery-group--3 .gallery-item--2 {
    bottom: 0;
    left: 0;
    width: 42%;
    height: 48%;
}
.gallery-group--3 .gallery-item--3 {
    bottom: 0;
    right: 0;
    width: 42%;
    height: 48%;
}

/* =========================
   TWO IMAGE GROUP
========================= */

.gallery-group--2 .gallery-item--1 {
    top: 10%;
    left: 5%;
    width: 42%;
    height: 70%;
}
.gallery-group--2 .gallery-item--2 {
    top: 10%;
    right: 5%;
    width: 42%;
    height: 70%;
}

/* =========================
   ONE IMAGE GROUP
========================= */

.gallery-group--1 .gallery-item--1 {
    top: 10%;
    left: 50%;
    width: 55%;
    height: 70%;
    transform: translateX(-50%);
}
.gallery-group--1 .gallery-item--1:hover {
    transform: translateX(-50%) scale(1.015);
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
    .gallery-group {
        margin-bottom: 35px;
    }
    .gallery-item {
        border-radius: 8px;
    }
    .gallery-item img {
        border-radius: 8px;
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