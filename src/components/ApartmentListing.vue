<template>
  <div class="apartment-listing">
    <b-carousel-list
      v-model="currentImageIndex"
      class="h-[60vh]"
      :arrow="true"
      :arrow-hover="true"
      :items-to-show="4"
      :items-to-list="1"
      :repeat="true"
      :has-drag="false"
      :has-grayscale="false"
      :has-opacity="false"
      :data="images">
      <template #item="list">
        <b-image :src="list.list" class="cursor-pointer"></b-image>
      </template>
    </b-carousel-list>

    <!-- Property Details -->
    <section class="property-details">
      <div class="container mx-auto px-4 py-8 flex flex-col items-center mt-5">
        <div class="text-gray-700 font-medium text-xl">Te koop</div>
        <h1 class="text-4xl font-bold mb-4">{{ property.title }}</h1>
        <div class="price-tag text-3xl font-semibold text-primary mb-6">
          €{{ property.price.toLocaleString('nl-BE') }}
    
        </div>
        <div class="location-tag text-gray-700 mb-6 flex gap-2 items-center">
          <i class="fas fa-map-marker-alt"></i>
          <span>{{ property.location }}</span>
        </div>
       

        <div class="description mb-8 text-center mx-10 my-15text-gray-300">
          <p class="text-gray-500">{{ property.description }}</p>
        </div>
         
        <div class="features-grid grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
          <div class="feature-item">
            <i class="fas fa-bed"></i>
            <span>{{ property.bedrooms }} slaapkamer</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-bath"></i>
            <span>{{ property.bathrooms }} badkamer</span>
          </div>
          <div class="feature-item"> 
            <i class="fas fa-ruler-combined"></i>
            <span>Kelder met berging</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-ruler-combined"></i>
            <span>{{ property.area }} m²</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-ruler-combined"></i>
            <span>{{ property.epc }} kWh/m²</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-calendar-alt"></i>
            <span>Beschikbaar vanaf akte</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-calendar-alt"></i>
            <span>Bergruimte voor wasmachine en droogkast</span>
          </div>
          <div class="feature-item">  
            <i class="fas fa-parking"></i>
            <span>Zonnepanelen op dak</span>
          </div>
        </div>
      </div>
    </section>

    <section class="py-8 flex justify-center items-center container">  
      <div class="mx-auto px-4 text-center">
        <h2 class="text-2xl font-semibold mb-6">Waarom dit appartement?</h2>
        <div class="grid grid-cols-2 gap-5">
          <div v-for="(why, index) in property.why" :key="index" class="p-2 flex flex-col items-start justify-items-start">
            <h3 class="text-lg font-semibold">{{ why.title }}</h3>
            <p class="text-gray-700 flex flex-col text-left">{{ why.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Video Section -->
    <!-- <section class="video-section bg-gray-50 py-8 p-10">
      <div class="container mx-auto px-4">
        <div class="video-container aspect-w-16 aspect-h-9 rounded-xl">
          <iframe
            :src="property.videoUrl"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
            class="w-full h-full rounded-xl"
          ></iframe>
        </div>
      </div>
    </section> -->

    <section class="flex flex-col items-center py-8 bg-gray-100">
      <div class="text-center mb-4">
        <h2 class="text-2xl font-semibold mb-2">Locatie</h2>
        <p class="text-gray-700 text-lg">{{ property.location }}</p>
      </div>
      <div class="w-full px-[20%]">
        <GoogleMaps/>
      </div>
    </section>

    <section class="gallery-section py-8 container">
      <div class="mx-auto px-4 text-center">
        <h2 class="text-2xl font-semibold mb-4 mt-6">In de buurt</h2>
        <div class="gallery-grid grid grid-cols-2 md:grid-cols-4 gap-4">
          <div v-for="(surrounding, index) in property.surroundings" :key="index" class="gallery-item">
            <img :src="surrounding.image" :alt="surrounding.title" class="rounded-md h-[300px] w-full object-cover" />
            <h3 class="text-lg font-semibold pt-4">{{ surrounding.title }}</h3>
            <p class="text-gray-700">{{ surrounding.description }}</p>
          </div>
        </div>
      </div>
    </section>
 

    <section class="why-section py-10 container">  
      <div class="mx-auto px-4">
        <h2 class="text-2xl font-semibold mb-5 text-center">Ideaal voor</h2>
        <div class="why-grid grid grid-cols-2 gap-5 md:px-20">
          <div v-for="(why, index) in property.for_whom" :key="index" class="text-center p-2">
            <h3 class="text-lg font-semibold">{{ why.title }}</h3>
            <p class="text-gray-700">{{ why.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <section class="py-8 flex justify-center items-center">
      Last but not least. Parking ook te koop.

    </section>



    <!-- Schedule Viewing -->
    <section class="bg-gray-100 text-gray-500 pt-8">
      <div class="mx-auto px-4 text-center p-5">
        <h2 class="text-3xl font-semibold mb-3">Plan een bezoek in</h2>
        <p class="text-gray-700">Bezoekdagen zijn gepland op 14 en 15 juni.</p>
        Contacteer ons via <a href="mailto:gistelinckpieter@gmail.com" class="text-primary">gistelinckpieter@gmail.com</a> of 
        <a href="tel:0487638688" class="text-primary">0487638688</a>
      </div>
    </section>

    <section class="bg-gray-100 text-gray-500 py-8 text-center" >  
      Made with ❤️ by Pieter
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import GoogleMaps from './GoogleMaps.vue'
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

// Convert data() to refs
const mainImage = ref(living5)
const images = ref([
  living1,
  buiten1,
  keuken2,
  slaapkamer1,
  living2,
  buiten2,
  badkamer2,
  buiten3,
  buiten4,
  buiten5,
  keuken1,

  keuken3,
  badkamer1,
  living3,
  living4,
  living5,
  living6,
])

const currentImageIndex = ref(0)

const property = ref({
  title: 'Gezellig appartement op toplocatie in Gent',
  price: 295000,
  bedrooms: 1,
  bathrooms: 1,
  bergruimte: 1,
  area: 59,
  location: 'Ferdinand Lousbergkaai 101 bus 003, 9000 Gent',
  description: 'Met veel plezier hebben wij hier als koppel gewoond. Een warm, licht en gezellig appartement waar we mooie herinneringen hebben opgebouwd. Nu er een baby op komst is, verhuizen we naar iets ruimer, en nemen we met pijn in het hart afscheid van deze plek. We hopen dat de volgende bewoners er net zo graag zullen wonen als wij. Kom gerust eens kijken — misschien wordt dit ook jouw nieuwe thuis.',
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
  why: [
    {
      title: 'Rustig gelegen',
      description: 'Niet gelegen langs een straat, baan je jezelf in een oase van rust.'
    },
    {
    title: 'Dicht bij de natuur',
    description: 'Het parkje achter het appartement is een perfecte plek om te genieten van de zon.',
  },
  { 
    title: 'Dicht bij de stad',
    description: 'In 10 minuten ben je al in het centrum.'  
  },
  {
    title: 'Snelle toegang tot autostrades en openbare vervoer',
    description: 'Je bent snel de E17 of E40 op. Daarnaast liggen Dampoort station en Zuidstation binnen 10 minuten.'
  },
  {
    title: 'Sportgelegenheden',
    description: 'Ga lopen naar de Gentbrugse Meersen. Zwem in het Van Eyck of Rozebroeken.'
  }],
  surroundings: [{
    title: 'Lousbergsmarkt',
    description: 'De Lousbergsmarkt is een van de grootste markten in Gent. Het is een perfecte plek om te eten of te kopen.',
    image: lousbergmarkt1,
  },
  {
    title: 'Gentbrugse Meersen',
    description: 'De Gentbrugse Meersen is een van de grootste parken in Gent. Het is een perfecte plek om te wandelen of te fietsen.',
    image: gentbrugsemeersen,
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
  }]
})

// Add these functions before onMounted

const nextImage = () => {
  currentImageIndex.value = (currentImageIndex.value + 1) % images.value.length
  mainImage.value = images.value[currentImageIndex.value]
}

const previousImage = () => {
  currentImageIndex.value = (currentImageIndex.value - 1 + images.value.length) % images.value.length
  mainImage.value = images.value[currentImageIndex.value]
}

// Add keyboard navigation
const handleKeydown = (e) => {
  if (!showModal.value) return
  
  if (e.key === 'ArrowRight') {
    nextImage()
  } else if (e.key === 'ArrowLeft') {
    previousImage()
  } else if (e.key === 'Escape') {
    showModal.value = false
  }
}

// Update the onMounted hook to include keyboard listener
onMounted(() => {
  // Load Calendly script
  const script = document.createElement('script')
  script.src = 'https://assets.calendly.com/assets/external/widget.js'
  script.async = true
  document.head.appendChild(script)
  
  // Add keyboard event listener
  window.addEventListener('keydown', handleKeydown)
})

// Add cleanup in onUnmounted
onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})

// Add modal state
const showModal = ref(false)
</script>

<style scoped>
.apartment-listing {
  font-family: 'Inter', sans-serif;
}

.main-image {
  flex: 2;
  height: 100%;
  overflow: hidden;
}

.main-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.thumbnail {
  cursor: pointer;
  overflow: hidden;
  border-radius: 0.5rem;
  aspect-ratio: 1;
  width: 150px;
  height: 150px;
}

.thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.thumbnail:hover img {
  transform: scale(1.05);
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 0.5rem;
}

.feature-item i {
  color: #4a90e2;
}

.video-container {
  position: relative;
  padding-bottom: 56.25%;
  height: 0;
  overflow: hidden;
}

.video-container iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.bg-primary {
  background-color: #4a90e2;
}

.text-primary {
  color: #4a90e2;
}

@media (max-width: 768px) {
  .hero-section {
    height: auto;
  }
  
  .main-image {
    height: 50vh;
  }
  
  .thumbnail-grid {
    grid-template-columns: repeat(2, 1fr);
    height: auto;
  }
}

/* Add modal styles */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}
</style> 