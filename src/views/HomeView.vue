<script setup>
import axios from 'axios'
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue'
import defulatImg from '../assets/WhatsApp Image 2024-10-06 at 4.44.11 PM.jpeg'

// Import Swiper styles
import 'swiper/css'

import 'swiper/css/pagination'
import 'swiper/css/navigation'

// import required modules
import { Autoplay, Pagination, Navigation } from 'swiper/modules'

import { onMounted, ref } from 'vue'
const cards = ref([])
const sliders = ref([])
const serves = ref([])
const language = ref('en')

const date = new Date()
const years = date.getFullYear()
const toggleLanguage = () => {
  language.value = language.value === 'en' ? 'ar' : 'en'
  data()
}
const dark = () => {
  document.body.classList.toggle('dark')
}

const data = async () => {
  try {
    const api = await axios.get('https://admin.wowws.net/api/company', {
      headers: {
        'Accept-Language': language.value,
      },
    })
    cards.value = api.data.data
    sliders.value = api.data.data[0].sliders
    serves.value = api.data.data[0].services
  } catch (error) {
    console.log(error)
  }
}
onMounted(() => {
  data()
})
</script>

<template>
  <nav class="flex justify-around items-center shadow py-2 bg-amber-100 fixed z-30 top-0 w-screen">
    <button class="bg-orange-400 w-20 h-10 rounded text-white text-xl" @click="toggleLanguage">
      {{ language === 'en' ? 'عربي' : 'English' }}
    </button>
    <button @click="dark" class="p-2 text-white bg-orange-400 rounded">
      {{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}
    </button>
  </nav>
  <swiper
    :modules="[Autoplay, Pagination, Navigation]"
    :autoplay="{ delay: 2500, disableOnInteraction: false }"
    :pagination="{ clickable: true }"
    :navigation="true"
    class="mySwiper"
  >
    <swiper-slide v-for="slid in sliders" :key="slid.id">
      <div class="slider-item">
        <img :src="slid.image" alt="Slide Image" />
      </div>
    </swiper-slide>
  </swiper>

  <!-- Display the services -->
  <div class="container mx-auto">
    <h2 class="text-2xl font-bold text-center py-4">خدمات شركة التكنولوجيا</h2>
    <div class="grid grid-col-1 md:grid-2 xl:grid-cols-4 gap-2">
      <article class="group my-3" v-for="service in serves" :key="service.id">
        <img
          :alt="service.title"
          :src="service.image || defulatImg"
          class="h-56 w-full rounded-xl object-cover shadow-xl"
        />

        <div class="p-4">
          <h3 class="text-lg font-medium text-gray-900">{{ service.title }}</h3>
          <div
            class="detilse my-2"
            :style="{ color: service.color, border: `1px solid ${service.color}` }"
          >
            <p class="card-text">
              {{ language === 'en' ? 'About this service' : 'حول هذه الخدمه' }}
            </p>
            <p class="card-text">{{ service.description }}</p>
          </div>
        </div>
      </article>
    </div>
  </div>

  <!-- Display the footer -->

  <footer class="bg-amber-100">
    <div class="mx-auto max-w-screen-xl space-y-2 px-4 py-2 sm:px-6 lg:space-y-2 lg:px-8">
      <div class="sm:flex sm:items-center sm:justify-between"></div>

      <div
        class="grid grid-cols-1 gap-8 border-t border-gray-100 pt-8 sm:grid-cols-2 lg:grid-cols-3 lg: items-end"
      >
        <div v-for="company in cards" :key="company.id">
          <p class="font-medium text-gray-900 text-2xl">{{ company.company_name }}</p>

          <ul class="mt-6 space-y-4 text-sm">
            <li>
              <a href="#" class="text-gray-700 text-lg transition hover:opacity-75">
                {{ company.description }}
              </a>
            </li>
          </ul>
        </div>

        <div>
          <ul class="mt-6 space-y-4 text-sm">
            <li v-for="serv in serves" :key="serv.id">
              <a href="#" class="text-gray-700 text-xl transition hover:opacity-75">
                {{ serv.title }}
              </a>
            </li>
          </ul>
        </div>
        <div class="">
          <p class="text-xs text-gray-500">
            &copy; {{ years }}. Company Name. All rights reserved.
          </p>
        </div>
        <div class="">
          <ul class="mt-8 flex justify-start gap-6 sm:mt-0 sm:justify-end">
            <li>
              <a
                href="https://web.facebook.com/TechInnovations?_rdc=1&_rdr#"
                rel="noreferrer"
                target="_blank"
                class="text-gray-700 transition hover:opacity-75"
              >
                <span class="sr-only">Facebook</span>

                <svg class="size-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                  <path
                    fill-rule="evenodd"
                    d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z"
                    clip-rule="evenodd"
                  />
                </svg>
              </a>
            </li>
            <li>
              <a
                href="https://x.com/TechInnovations"
                rel="noreferrer"
                target="_blank"
                class="text-gray-700 transition hover:opacity-75"
              >
                <span class="sr-only">Twitter</span>

                <svg class="size-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                  <path
                    d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84"
                  />
                </svg>
              </a>
            </li>
            <li>
              <a
                href="https://www.instagram.com/TechInnovations/#"
                rel="noreferrer"
                target="_blank"
                class="text-gray-700 transition hover:opacity-75"
              >
                <span class="sr-only">Instagram</span>

                <svg class="size-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                  <path
                    fill-rule="evenodd"
                    d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z"
                    clip-rule="evenodd"
                  />
                </svg>
              </a>
            </li>
            <li>
              <a
                href="https://www.instagram.com/TechInnovations/#"
                rel="noreferrer"
                target="_blank"
                class="text-gray-700 transition hover:opacity-75"
              >
                <span class="sr-only">LinkedIn</span>

                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  x="0px"
                  y="0px"
                  class="size-6"
                  viewBox="0 0 48 48"
                >
                  <path
                    fill="#0288D1"
                    d="M42,37c0,2.762-2.238,5-5,5H11c-2.761,0-5-2.238-5-5V11c0-2.762,2.239-5,5-5h26c2.762,0,5,2.238,5,5V37z"
                  ></path>
                  <path
                    fill="#FFF"
                    d="M12 19H17V36H12zM14.485 17h-.028C12.965 17 12 15.888 12 14.499 12 13.08 12.995 12 14.514 12c1.521 0 2.458 1.08 2.486 2.499C17 15.887 16.035 17 14.485 17zM36 36h-5v-9.099c0-2.198-1.225-3.698-3.192-3.698-1.501 0-2.313 1.012-2.707 1.99C24.957 25.543 25 26.511 25 27v9h-5V19h5v2.616C25.721 20.5 26.85 19 29.738 19c3.578 0 6.261 2.25 6.261 7.274L36 36 36 36z"
                  ></path>
                </svg>
              </a>
            </li>
            <li>
              <a
                href="https://www.youtube.com/channel/TechInnovations"
                rel="noreferrer"
                target="_blank"
                class="text-gray-700 transition hover:opacity-75"
              >
                <span class="sr-only">Youtube</span>

                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  x="0px"
                  y="0px"
                  viewBox="0 0 48 48"
                  class="size-6"
                >
                  <path
                    fill="#FF3D00"
                    d="M43.2,33.9c-0.4,2.1-2.1,3.7-4.2,4c-3.3,0.5-8.8,1.1-15,1.1c-6.1,0-11.6-0.6-15-1.1c-2.1-0.3-3.8-1.9-4.2-4C4.4,31.6,4,28.2,4,24c0-4.2,0.4-7.6,0.8-9.9c0.4-2.1,2.1-3.7,4.2-4C12.3,9.6,17.8,9,24,9c6.2,0,11.6,0.6,15,1.1c2.1,0.3,3.8,1.9,4.2,4c0.4,2.3,0.9,5.7,0.9,9.9C44,28.2,43.6,31.6,43.2,33.9z"
                  ></path>
                  <path fill="#FFF" d="M20 31L20 17 32 24z"></path>
                </svg>
              </a>
            </li>
            <li>
              <a
                href="https://www.tiktok.com/@TechInnovations"
                rel="noreferrer"
                target="_blank"
                class="text-gray-700 transition hover:opacity-75"
              >
                <span class="sr-only">Tiktok</span>

                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  x="0px"
                  y="0px"
                  class="size-6"
                  viewBox="0 0 50 50"
                >
                  <path
                    d="M41,4H9C6.243,4,4,6.243,4,9v32c0,2.757,2.243,5,5,5h32c2.757,0,5-2.243,5-5V9C46,6.243,43.757,4,41,4z M37.006,22.323 c-0.227,0.021-0.457,0.035-0.69,0.035c-2.623,0-4.928-1.349-6.269-3.388c0,5.349,0,11.435,0,11.537c0,4.709-3.818,8.527-8.527,8.527 s-8.527-3.818-8.527-8.527s3.818-8.527,8.527-8.527c0.178,0,0.352,0.016,0.527,0.027v4.202c-0.175-0.021-0.347-0.053-0.527-0.053 c-2.404,0-4.352,1.948-4.352,4.352s1.948,4.352,4.352,4.352s4.527-1.894,4.527-4.298c0-0.095,0.042-19.594,0.042-19.594h4.016 c0.378,3.591,3.277,6.425,6.901,6.685V22.323z"
                  ></path>
                </svg>
              </a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.dark-mode {
  background-color: rgba(0, 0, 0, 0.751);
  color: white;
  transition:
    background-color 0.5s,
    color 0.5s;
}

.swiper {
  width: 60%;
  height: 50%;
  margin-top: 5rem;
}

.swiper-slide {
  text-align: center;
  font-size: 18px;
  background: #fff;

  /* Center slide text vertically */
  display: flex;
  justify-content: center;
  align-items: center;
}

.swiper-slide img {
  display: block;
  width: 100%;
  height: 400px;
  object-fit: cover;
}
::v-deep(.swiper-pagination-bullet) {
  background-color: rgb(255, 98, 0); /* لون النقاط */
  opacity: 0.6; /* شفافية النقاط */
}

::v-deep(.swiper-pagination-bullet-active) {
  background-color: rgb(255, 157, 0); /* لون النقطة النشطة */
  opacity: 1; /* شفافية النقطة النشطة */
  transform: scale(1.2); /* تكبير النقطة النشطة */
  transition: transform 0.3s ease, opacity 0.3s ease;
}
::v-deep(.swiper-button-next) {
    color: rgb(255, 128, 0); /* لون الأيقونة */
}

::v-deep(.swiper-button-prev) {
  color: rgb(255, 128, 0); /* لون الأيقونة */
}


</style>
