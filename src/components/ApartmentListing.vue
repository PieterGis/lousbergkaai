<template>
  <div class="bg-gradient-to-br from-amber-50 via-orange-50 to-red-50">
    <!-- Add ImageModal component -->
    <ImageModal 
      :is-open="isModalOpen" 
      :image="selectedImage" 
      :alt="selectedImageAlt"
      :current-index="currentImageIndex"
      :total-images="allImages.length"
      @close="closeModal"
      @previous="previousImage"
      @next="nextImage"
    />

    <div class="grid grid-cols-4 gap-2 container pt-5 px-4">
      <div v-for="(image, index) in images_header" :key="index" class="cursor-pointer h-[200px] md:h-[400px]" @click="openModal(image, 'Main Image', 'header')">
        <img :src="image" alt="Main Image" class="w-full h-full object-cover rounded-2xl">
      </div> 
    </div>

    <!-- Property Details -->
     <div class="container flex justify-center py-3 px-4">
      <section class="bg-gradient-to-r from-orange-400 to-amber-400 rounded-3xl p-2 md:p-12 mb-12 text-white shadow-2xl mb-5 w-full">
      <div class="py-2 flex flex-col items-center mt-5 text-white">
        <div class="text-white font-medium md:text-xl">Te koop</div>
        <h1 class="text-lg md:text-4xl font-bold md:mb-4 text-center">{{ property.title }}</h1>
        <div class="price-tag md:text-3xl font-semibold text-yellow-200 mb-2 md:mb-3">
          €{{ property.price.toLocaleString('nl-BE') }}
        </div>
        <div class="text-gray-700 mb-2 md:mb-6 flex gap-2 items-center">
          <p class="md:text-2xl text-orange-100 leading-relaxed text-center mb-5">{{ property.location }}</p>
        </div>
      </div>
    </section>
     </div>

     <div class="container flex justify-center py-3 px-4 text-center">
      <div class="bg-white rounded-3xl p-5 shadow-lg w-[120vh] flex items-center gap-8 p-5">
        <p class="text-gray-700">{{ property.koppel.description }}</p>
        <div class="w-[100px] h-[150px] rounded-2xl rotate-12 overflow-hidden shrink-0 flex items-center justify-center transition-transform duration-300 hover:rotate-[360deg]">
          <img :src="property.koppel.image" :alt="property.koppel.title" 
            class="w-full h-full object-cover">
        </div>
      </div>
     </div>
 

    <section class="py-8 flex justify-center items-center container">  
      <div class="mx-auto px-4 w-full">
        <div class="timeline-container">
          <div v-for="(why, index) in property.story" :key="index" 
               class="flex flex-col md:flex-row items-center gap-2 mb-8">
            <div class="timeline-content flex flex-col md:flex-row items-center gap-8">
              <div class="w-full md:w-1/2 p-5 flex justify-center items-center">
                <img 
                  :src="why.image" 
                  :alt="why.title" 
                  class="object-cover shadow-lg rounded-lg cursor-pointer hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 " 
                  :class="why.class"
                  @click="openModal(why.image, why.title)"
                >
              </div>
              <div class="timeline-text w-full md:w-1/2 rounded-lg p-5 shadow-lg bg-white">
                <i class="fas fa-hand-wave text-amber-400 text-2xl"></i>
                <p class="flex items-center space-x-3 p-3 rounded-xl font-medium text-stone-800">{{ why.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <h2 class="text-2xl font-semibold mb-5 text-center text-amber-400">Eigenschappen</h2>
    <div class="features-grid grid grid-cols-1 md:grid-cols-4 gap-2 md:gap-4 mb-8 container mx-auto px-4">
      
      <div v-for="(feature, index) in property.features" 
        :key="index" 
        class="bg-white rounded-2xl p-5 shadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 border border-orange-100 flex items-center gap-2">
        <div class="flex items-center space-x-3">
          <div class="p-3 bg-gradient-to-br from-orange-100 to-amber-100 rounded-xl text-orange-600">
            <i :class="feature.icon"></i>
          </div>
        </div>
        <span class="text-stone-600 text-sm">{{ feature.text }}</span>
      </div>
    </div>

    <section class="py-8 container">
      <div class="mx-auto px-4">
        <h2 class="text-2xl font-semibold mb-5 mt-6 text-amber-400 text-center">In de buurt</h2>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
          <div v-for="(surrounding, index) in property.surroundings" :key="index" 
          class="bg-white rounded-lg shadow-lg overflow-hidden shadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 ">
            <img 
              :src="surrounding.image" 
              :alt="surrounding.title" 
              class="h-[300px] w-full object-cover cursor-pointer" 
              @click="openModal(surrounding.image, surrounding.title)"
            />
            <div class="p-4"> 
              <div>
                <h3 class="font-semibold mb-1">{{ surrounding.title }}</h3>
              </div>
              <p class="text-gray-700" v-html="surrounding.description"></p>
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
          bg-gradient-to-r from-orange-500 to-amber-500 rounded-xl text-white shadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 ">
            <h3 class="text-lg font-semibold text-white">{{ why.title }}</h3>
            <p class="text-white" v-html="why.description"></p>
          </div>
        </div>
      </div>
    </section>

    <h2 class="text-2xl font-semibold mb-5 mt-6 text-amber-400 text-center">Foto's</h2>
    <div class="grid grid-cols-4 gap-2 mt-2 container mx-auto px-4">
      <div v-for="(image, index) in images_other" :key="index" class="cursor-pointer shadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 rounded-2xl overflow-hidden" @click="openModal(image, 'Main Image', 'other')">
        <img :src="image" alt="Main Image" class="w-full h-[150px] md:h-[400px] object-cover ">
      </div>
    </div>

    <section class="py-8 flex justify-center items-center container px-4">  
      <div class="flex flex-col md:flex-row items-center gap-8 mb-8">
        <div class="timeline-content flex flex-col md:flex-row items-center gap-8">
          <div class="w-full md:w-1/2 p-5 flex justify-center items-center ">
            <img :src="property.parking.image" :alt="property.parking.title" 
            class="object-cover rounded-lg size-[300px] cursor-pointer shadow-lg hover:shadow-xl transition-all 
        duration-300 transform hover:-translate-y-1 " @click="openModal(property.parking.image, property.parking.title)">
          </div>
          <div class="w-full md:w-1/2 rounded-lg p-5 shadow-lg bg-white">
              <h3 class="text-lg font-semibold text-amber-400">{{ property.parking.title }}</h3>
            <p class="flex items-center space-x-3 rounded-xl font-medium text-stone-800">{{ property.parking.description }}</p>
          </div>
        </div>
      </div>
    </section>


    <!-- Schedule Viewing -->
    <section class="bg-gradient-to-r from-amber-400 to-orange-400 rounded-t-3xl p-2 md:p-12 text-white shadow-2xl container mt-4 p-5">
      <div class="mx-auto px-4 text-center p-5 text-white">
        <h2 class="text-3xl font-bold mb-3 text-white">Plan een bezoek in</h2>
        <p class="text-white font-medium">De bezoekersdag gaat door op zaterdag 14 juni.</p>
        <span class="text-white font-medium">Contacteer ons via <a href="mailto:gistelinckpieter@gmail.com" class="text-white">gistelinckpieter@gmail.com</a> of 
        <a href="tel:+32 498 40 91 17" class="text-white">+32 498 40 91 17</a></span>

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
import plan1 from '@/assets/plan1.jpeg'
import kaai from '@/assets/kaai.jpg'
import keuken4 from '@/assets/keuken4.jpg'

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
  slaapkamer1,
  keuken4,
  kaai,
  plan1,
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
    title: 'Last but not least',
    description: 'Onze ondergrondse parkeerplaats is ook te koop.',
    image: parking1,
  },
  koppel: {
    title: 'Koppel',
    description: 'Hallo, wij zijn Eveline en Pieter, en wij verkopen ons appartement. Met een baby op komst is het tijd om te verhuizen naar iets ruimer. We hebben hier veel plezier gehad en mooie herinneringen opgebouwd. We hopen dat de volgende bewoners hier net zo graag zullen wonen als wij!',
    image: koppel,
    class: 'w-[500px] h-[500px]'
  },
  story: [
    {
      description: 'Prachtig gelegen en dicht bij de natuur, je gelooft haast niet dat je in Gent woont. Het parkje achter het appartement is een perfecte plek om te genieten van de zon. Zowel het appartement als het park liggen niet rechtstreeks langs de baan. Zo waan je je in een oase van rust, dicht bij het centrum van Gent.',
      image: living2,
      class: 'w-[500px] h-[600px]'
    },
    {
      description: 'Het appartement ligt niet ver van de Keizervest, waar je meteen toegang hebt tot de autostrades E40 en E17. Ook het openbaar vervoer is binnen handbereik: Gent Dampoort en Gent Zuid liggen binnen een kwartier stappen. De mooiste troef: op 10 minuten fietsen sta je pal in het centrum van het mooie Gent.',
      class: 'w-[500px] h-[500px]',
      image:   buiten1,
    },
    {
      description: 'Ben je sportief of hou je van wandelen? Ga lopen langs het water of verken de Gentbrugse Meersen. De Visserij komt uit in het Keizerpark. Van daar kan je via een autovrij pad langs de Schelde tot aan de Gentbrugse Meersen lopen. Liever zwemmen? Ook het Van Eyck of de Rozebroeken liggen in de buurt. Of ga naar de gym in het centrum. ',
      image: gentbrugsemeersen,
      class: 'w-[500px] h-[500px]'
    }
  ],
  surroundings: [
    {
      title: 'Lousbergspark',
      description: 'Via het terras kom je rechtstreeks in het Lousbergpark (1,14 hectare). Dit gezellige parkje ligt verscholen tussen de Ferdinand Lousbergkaai, Karperstraat, Tarbotstraat en Forelstraat. Je vindt er een speeltuin, een grote picknicktafel, een petanquebaan en een buurtcentrum met gemeenschapstuin. <a href="https://visit.gent.be/nl/zien-doen/ferdinand-lousbergpark" target="_blank">Meer info</a>',
      image: buiten5,
    },
  {
    title: 'Lousbergsmarkt',
    description: 'Wandel langs het water naar de Lousbergsmarkt. Deze overdekte versmarkt huist een groentewinkel, kaaswinkel, bakkerij en brunchplek. Wie een hart heeft voor lokale en biologische producten, vindt hier zeker iets naar zijn of haar zin. <a href="https://www.lousbergmarkt.be/" target="_blank">Meer info</a>',
    image: lousbergmarkt1,
  },
  {
    title: 'Koningin Astridpark',
    description: 'Naast de Lousbergsmarkt ligt het Koningin Astridpark. Het is een park met een vijver, open grasveld en speelterrein. Ideaal om te ontspannen. <a href="https://stad.gent/nl/buitenlocaties/koningin-astridpark" target="_blank".>Meer info</a>',
    image: koninginAstridpark,
  },
  {
    title: 'Buurtwinkel',
    description: 'In de Forelstraat, op slechts twee minuten stappen, bevindt zich de Buurtwinkel, waar je alle essentiële benodigdheden kan vinden.',
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
    description: 'Geniet van elkaar en de buurt. ❤️ '
  }],
  features: [
    {
      icon: 'fas fa-bed',
      text: '1 slaapkamer',
    },
    {
      icon: 'fas fa-bath',
      text: '1 badkamer (gerenoveerd in 2020)',
    },
    {
      icon: 'fas fa-broom',
      text: 'Berging met plaats voor wasmachine en droogkast'
    },
    {
      icon: 'fas fa-broom',
      text: '1 extra bergruimte in kelder',
    },
 
    {
      icon: 'fas fa-ruler-combined',
      text: '59 m² bewoonbare ruimte + 28 m² terras',
    },
    {
      icon: 'fas fa-bolt',
      text: '322 kWh/m² (EPC D)',
    },
    {
      icon: 'fas fa-solar-panel',
      text: 'Zonnepanelen op dak'
    },
    {
      icon: 'fas fa-calendar-alt',
      text: 'Beschikbaar vanaf akte'
    }
  ]
})

// Add these new refs for modal functionality
const isModalOpen = ref(false)
const selectedImage = ref('')
const selectedImageAlt = ref('')
const currentImageIndex = ref(0)
const allImages = ref([])

// Update the openModal method
const openModal = (image, alt, section = 'all') => {
  // Set images based on section
  if (section === 'header') {
    allImages.value = [...images_header.value]
  } else if (section === 'other') {
    allImages.value = [...images_other.value]
  } else {
    allImages.value = [image]
  }
  
  currentImageIndex.value = allImages.value.indexOf(image)
  selectedImage.value = image
  selectedImageAlt.value = alt
  isModalOpen.value = true
}

// Add navigation methods
const previousImage = () => {
  if (currentImageIndex.value > 0) {
    currentImageIndex.value--
  } else {
    // If at the start, go to the end
    currentImageIndex.value = allImages.value.length - 1
  }
  selectedImage.value = allImages.value[currentImageIndex.value]
}

const nextImage = () => {
  if (currentImageIndex.value < allImages.value.length - 1) {
    currentImageIndex.value++
  } else {
    // If at the end, go back to the start
    currentImageIndex.value = 0
  }
  selectedImage.value = allImages.value[currentImageIndex.value]
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