<template>
  <div class="apartment-listing  bg-gradient-to-br from-amber-50 via-orange-50 to-red-50">
    <!-- Add ImageModal component -->
    <ImageModal 
      :is-open="isModalOpen" 
      :image="selectedImage" 
      :alt="selectedImageAlt"
      @close="closeModal"
    />

    <div class="grid grid-cols-4 gap-1">
      <div v-for="(image, index) in images_header" :key="index" class="cursor-pointer h-[400px]" @click="openModal(image, 'Main Image')">
        <img :src="image" alt="Main Image" class="w-full h-full object-cover">
      </div> 
    </div>

    <!-- Property Details -->
    <section class="bg-gradient-to-r from-orange-600 to-amber-600 rounded-b-3xl p-2 md:p-12 mb-12 text-white shadow-2xl container mb-5">
      <div class="container py-2 flex flex-col items-center mt-5 text-white">
        <div class="text-white font-medium text-xl">Te koop</div>
        <h1 class="text-4xl font-bold mb-4">{{ property.title }}</h1>
        <div class="price-tag text-3xl font-semibold text-amber-200 mb-3">
          €{{ property.price.toLocaleString('nl-BE') }}
    
        </div>
        <div class="location-tag text-gray-700 mb-6 flex gap-2 items-center">
          <p class="text-xl md:text-2xl text-orange-100 leading-relaxed">{{ property.location }}</p>
        </div>
      </div>
    </section>

    <section class="py-8 flex justify-center items-center container">  
      <div class="mx-auto px-4 w-full">
        <div class="timeline-container">
          <div v-for="(why, index) in property.story" :key="index" 
               class="flex flex-col md:flex-row items-center gap-8 mb-8">
            <div class="timeline-content flex flex-col md:flex-row items-center gap-8">
              <div class="timeline-image w-full md:w-1/2 p-5 flex justify-center items-center">
                <img :src="why.image" :alt="why.title" class="object-cover rounded-lg" :class="why.class">
              </div>
              <div class="timeline-text w-full md:w-1/2 bg-white rounded-lg p-5 shadow-lg">
                <i class="fas fa-hand-wave text-amber-400 text-2xl"></i>
                <p class="text-gray-600 p-5 font-medium tracking-wide">{{ why.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <div class="features-grid grid grid-cols-2 md:grid-cols-4 gap-4 mb-8 container mx-auto px-4">
      <div v-for="(feature, index) in property.features" 
        :key="index" 
        class="bg-white rounded-2xl p-5 shadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 border border-orange-100 flex items-center gap-2">
        <div class="flex items-center space-x-3">
          <div class="p-3 bg-gradient-to-br from-orange-100 to-amber-100 rounded-xl text-orange-600">
            <i :class="feature.icon"></i>
          </div>
        </div>
        <span class="text-stone-600">{{ feature.text }}</span>
      </div>
    </div>

    <section class="py-8 container">
      <div class="mx-auto px-4">
        <h2 class="text-2xl font-semibold mb-5 mt-6 text-amber-400 text-center">In de buurt</h2>
        <div class="gallery-grid grid grid-cols-2 md:grid-cols-4 gap-4">
          <div v-for="(surrounding, index) in property.surroundings" :key="index" class="bg-white rounded-lg shadow-lg overflow-hidden">
            <img 
              :src="surrounding.image" 
              :alt="surrounding.title" 
              class="h-[300px] w-full object-cover cursor-pointer" 
              @click="openModal(surrounding.image, surrounding.title)"
            />
            <div class="p-4"> 
              <div>
                <h3 class="font-semibold">{{ surrounding.title }}</h3>
              </div>
              <p class="text-gray-700">{{ surrounding.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>
 

    <section class="why-section py-12 container">  
      <div class="mx-auto px-4">
        <h2 class="text-2xl font-semibold mb-5 text-center text-amber-400">Ideaal voor</h2>
        <div class="grid grid-cols-2 gap-5 md:px-20">
          <div v-for="(why, index) in property.for_whom" :key="index" class="text-center p-2 rounded-md p-3 shadow-lg
          bg-gradient-to-r from-orange-500 to-amber-500 rounded-xl text-white">
            <h3 class="text-lg font-semibold text-white">{{ why.title }}</h3>
            <p class="text-white">{{ why.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <div class="grid grid-cols-4 gap-1 mt-2">
      <div v-for="(image, index) in images_other" :key="index" class="main-image cursor-pointer" @click="openModal(image, 'Main Image')">
        <img :src="image" alt="Main Image" class="w-full h-[150px] md:h-[500px] object-cover">
      </div>
    </div>

    <section class="py-8 flex justify-center items-center container px-4">
      <div class="mx-auto bg-white rounded-lg shadow-lg flex md:flex-row items-center hadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1">
        <div class="flex flex-col items-start md:items-center px-5">
          <h2 class="text-2xl font-semibold text-gray-600">Last but not least</h2>
          <p class="font-medium mb-4 text-gray-600">Onze ondergrondse parking is ook te koop.</p>
        </div>

        <img :src="parking1" alt="Parking" class="size-[150px] md:size-[30vh] object-cover rounded-md cursor-pointer" @click="openModal(parking1, 'Parking')" />
      </div>
    </section>


    <!-- Schedule Viewing -->
    <section class="bg-gradient-to-r from-orange-600 to-amber-600 rounded-t-3xl p-2 md:p-12 mb-12 text-white shadow-2xl container mt-4">
      <div class="mx-auto px-4 text-center p-5 text-white">
        <h2 class="text-3xl font-bold mb-3 text-white">Plan een bezoek in</h2>
        <p class="text-white font-medium">Bezoekdagen zijn gepland op 14 juni.</p>
        <span class="text-white font-medium">Contacteer ons via <a href="mailto:gistelinckpieter@gmail.com" class="text-white">gistelinckpieter@gmail.com</a> of 
        <a href="tel:0487638688" class="text-white">0487638688</a></span>

        <div class="py-8 text-center text-white" >  
          Made with ❤️ by Eveline & Pieter
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ImageModal from './ImageModal.vue'
import lousbergmarkt1 from '@/assets/lousbergmarkt1.jpeg'
import koninginAstridpark from '@/assets/astrid1.jpeg'
import buiten1 from '@/assets/buiten1.jpeg'
import buiten2 from '@/assets/buiten2.jpeg'
import buiten3 from '@/assets/buiten3.jpeg'
import buiten4 from '@/assets/buiten4.jpeg'
import buiten5 from '@/assets/buiten5.jpeg'
import keuken1 from '@/assets/keuken1.jpeg'
import keuken2 from '@/assets/keuken2.jpeg'
import keuken3 from '@/assets/keuken3.jpeg'
import living1 from '@/assets/living1.jpeg'
import living2 from '@/assets/living2.jpeg'
import living3 from '@/assets/living3.jpeg'
import living4 from '@/assets/living4.jpeg'
import living5 from '@/assets/living5.jpeg'
import living6 from '@/assets/living6.jpeg'
import slaapkamer1 from '@/assets/slaapkamer1.jpeg'
import badkamer1 from '@/assets/badkamer1.jpeg'
import badkamer2 from '@/assets/badkamer2.jpeg'
import buurtwinkel from '@/assets/buurtwinkel.jpeg'
import gentbrugsemeersen from '@/assets/gentbrugsemeersen.png'
import parking1 from '@/assets/parking1.webp'
import koppel from '@/assets/koppel.jpeg'

const images_header = ref([
  living1,
  buiten1,
  keuken2,
  slaapkamer1
])

const images_other = ref([
  buiten1,
  buiten2,
  keuken1,
  badkamer2,
  buiten3,
  living3,
  living4,
  living5,
  living6,
  badkamer1,
  keuken3,
  buiten4,
])

const property = ref({
  title: 'Gezellig appartement op toplocatie in Gent',
  price: 295000,
  bedrooms: 1,
  bathrooms: 1,
  bergruimte: 1,
  area: 59,
  location: 'Ferdinand Lousbergkaai 101 bus 003, 9000 Gent',
  description: 'Met veel plezier hebben wij hier als koppel gewoond. Een warm, licht en gezellig appartement waar we mooie herinneringen hebben opgebouwd. Nu er een baby op komst is, verhuizen we naar iets ruimer, en nemen we met pijn in het hart afscheid van deze plek. We hopen dat de volgende bewoners er net zo graag zullen wonen als wij. Kom gerust eens kijken. Misschien wordt dit ook jouw nieuwe thuis.',
  videoUrl: 'https://www.youtube.com/embed/lx3RndgFIG8',
  epc: 289,
  mapUrl: 'https://www.google.com/maps/embed?pb=your-location-embed-code',
  calendlyUrl: 'https://calendly.com/gistelinckpieter/bezoek-appartement',
  available_from: '14 juli 2025',
  parking: {
    title: 'Parking',
    description: 'Parking ook te koop.',
    image: '/images/surroundings/1.jpg',
  },
  story: [
    {
      description: 'Hallo, wij zijn Eveline en Pieter, en wij verkopen ons appartement. Met een baby op komst is het tijd om te verhuizen naar iets ruimer. We hebben hier als koppel veel plezier gehad en mooie herinneringen opgebouwd. We hopen dat de volgende bewoners er net zo graag zullen wonen als wij.',
      image: koppel,
      class: 'w-[350px] h-[500px]'
    },
    {
      description: 'Prachtig gelegen en dicht bij de natuur, je gelooft soms niets dat je in Gent woont. Het parkje achter het appartement is een perfecte plek om te genieten van de zon. Het ligt ook ingesloten en niet langs een strat. Je waant jezelf in een oase van rust, in Gent.',
      image: living2,
      class: 'w-[500px] h-[600px]'
    },
    {
      description: 'Ons werk ligt in Ninove en Gent. Met dat we heel snel toegang hebben tot autostrades was. Maar je bent ook heel dicht bij de stad. In 5 minuten sta je in het centrum. 12 minuten stappen naar Dampoort station en 10 minuten naar het Zuid. Openbaar vervoer in handbereik.',
      class: 'w-[500px] h-[500px]',
      image:   buiten1,
    },
    {
      description: 'Ben jij een sportief persoon? Ga lopen naar de Gentbrugse Meersen. Zwem in het Van Eyck of Rozebroeken. Of ga naar de gym in het centrum. Alles binnen handbereik.',
      image: gentbrugsemeersen,
      class: 'w-[500px] h-[500px]'
    }
  ],
  surroundings: [
    {
      title: 'Lousbergspark',
      description: 'Buiten',
      image: buiten5,
    },
  {
    title: 'Lousbergsmarkt',
    description: 'De Lousbergsmarkt is een van de grootste markten in Gent. Het is een perfecte plek om te eten of te kopen.',
    image: lousbergmarkt1,
  },
  {
    title: 'Koningin Astridpark',
    description: 'De Koningin Astridpark is een van de grootste parken in Gent. Het is een perfecte plek om te wandelen of te fietsen.',
    image: koninginAstridpark,
  },
  {
    title: 'Buurtwinkel',
    description: 'De buurtwinkel is een van de grootste winkels in Gent. Het is een perfecte plek om te eten of te kopen.',
    image: buurtwinkel,
  }],
  for_whom: [{
    title: 'Starters',
    description: 'Begin zorgeloos aan jouw carrière.'
  },
  {
    title: 'Investeerders',
    description: 'Ideaal om te verhuren.'
  },
  {
    title: 'Singles',
    description: 'Geniet van jouw eigen stekje in Gent.'
  },
  {
    title: 'Koppels',
    description: 'Geniet van elkaar en de buurt.'
  }],
  features: [
    {
      icon: 'fas fa-bed',
      text: '1 slaapkamer',
    },
    {
      icon: 'fas fa-bath',
      text: '1 badkamer',
    },
    {
      icon: 'fas fa-ruler-combined',
      text: '1 bergruimte',
    },
    {
      icon: 'fas fa-ruler-combined',
      text: '59 m²',
    },
    {
      icon: 'fas fa-ruler-combined',
      text: '289 kWh/m²',
    },
    {
      icon: 'fas fa-calendar-alt',
      text: 'Beschikbaar vanaf akte'
    },
    {
      icon: 'fas fa-calendar-alt',
      text: 'Bergruimte (wasmachine en droogkast)'
    },
    {
      icon: 'fas fa-parking',
      text: 'Zonnepanelen op dak'
    }
  ]
})

// Add these new refs for modal functionality
const isModalOpen = ref(false)
const selectedImage = ref('')
const selectedImageAlt = ref('')

// Add these new methods for modal functionality
const openModal = (image, alt) => {
  selectedImage.value = image
  selectedImageAlt.value = alt
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
  selectedImage.value = ''
  selectedImageAlt.value = ''
}

// Update the onMounted hook to include keyboard listener
onMounted(() => {
  // Load Calendly script
  const script = document.createElement('script')
  script.src = 'https://assets.calendly.com/assets/external/widget.js'
  script.async = true
  document.head.appendChild(script)
})

</script>