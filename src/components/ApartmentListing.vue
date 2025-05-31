<template>
  <div class="apartment-listing">
    <!-- Hero Section with Image Gallery -->
    <section class="hero-section">
      <div class="flex flex-col md:flex-row h-full rounded-b-3xl overflow-hidden">
        <div class="flex basis-2/3">
          <img 
            :src="mainImage" 
            alt="Main apartment view" 
            class="cursor-pointer object-cover h-full w-full" 
            @click="showModal = true"
          />
        </div>
        <div class="flex basis-1/3 flex-wrap gap-2 p-2">
          <div v-for="(image, index) in images" :key="index" @click="setMainImage(image)" class="flex thumbnail">
            <img :src="image" :alt="'Apartment view ' + (index + 1)" class="object-cover h-full w-full" />
          </div>
        </div>
      </div>
    </section>

    <!-- Image Modal -->
    <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-75" @click="showModal = false">
      <div class="relative max-w-7xl max-h-[90vh] p-4" @click.stop>
        <img 
          :src="mainImage" 
          alt="Main apartment view" 
          class="max-h-[85vh] w-auto object-contain"
        />
        <button 
          class="absolute top-2 right-2 text-white bg-black bg-opacity-50 rounded-full p-2 hover:bg-opacity-75"
          @click="showModal = false"
        >
          <i class="fas fa-times"></i>
        </button>
      </div>
    </div>

    <!-- Property Details -->
    <section class="property-details">
      <div class="container mx-auto px-4 py-8 flex flex-col items-center mt-5">
        <div class="text-gray-700 font-medium">Te koop</div>
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
            <span>{{ property.area }} m²</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-calendar-alt"></i>
            <span>Beschikbaar vanaf {{ property.available_from }}</span>
          </div>
        </div>
      </div>
    </section>

    <section class="py-8 flex justify-center items-center">  
      <div class="mx-auto px-4 text-center">
        <h2 class="text-2xl font-semibold mb-6">Waarom dit appartement?</h2>
        <div class="why-grid grid grid-cols-2 md:grid-cols-4 gap-4">
          <div v-for="(why, index) in property.why" :key="index" class="why-item">
            <h3 class="text-lg font-semibold">{{ why.title }}</h3>
            <p class="text-gray-700">{{ why.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Video Section -->
    <section class="video-section bg-gray-50 py-8 p-10">
      <div class="container mx-auto px-4">
        <div class="video-container aspect-w-16 aspect-h-9 rounded-xl">
          <iframe
            :src="property.videoUrl"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          ></iframe>
        </div>
      </div>
    </section>

    <section class="flex gap-2 w-full p-10">
      <div class="basis-1/3 p-4">
        <h2 class="text-2xl font-semibold mb-4">Locatie</h2>
        <p class="text-gray-700">Ferdinand Lousbergkaai 101/003, 9000 Gent</p>
      </div>
      <div class="basis-2/3">
        <GoogleMaps/>
      </div>
    </section>

    <section class="gallery-section py-8">
      <div class="mx-auto px-4 text-center">
        <h2 class="text-2xl font-semibold mb-6">In de buurt</h2>
        <div class="gallery-grid grid grid-cols-2 md:grid-cols-4 gap-4">
          <div v-for="(surrounding, index) in property.surroundings" :key="index" class="gallery-item">
            <img :src="surrounding.image" :alt="surrounding.title" />
            <h3 class="text-lg font-semibold">{{ surrounding.title }}</h3>
            <p class="text-gray-700">{{ surrounding.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Location Map -->
    <!-- <section class="location-section py-8">
      <div class="container mx-auto px-4">
        <h2 class="text-2xl font-semibold mb-6">Location</h2>
        <div class="map-container h-96 rounded-lg overflow-hidden">
          <iframe
            :src="property.mapUrl"
            width="100%"
            height="100%"
            style="border:0;"
            allowfullscreen=""
            loading="lazy"
          ></iframe>
        </div>
      </div>
    </section> -->

 

    <section class="why-section py-8 text-center">  
      <div class="mx-auto px-4">
        <h2 class="text-2xl font-semibold mb-6">Voor wie?</h2>
        <div class="why-grid grid grid-cols-2 md:grid-cols-4 gap-4">
          <div v-for="(why, index) in property.for_whom" :key="index" class="why-item">
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
      <div class="mx-auto px-4 text-center">
        <h2 class="text-3xl font-semibold">Plan een bezoek in</h2>
        <div class="calendly-container h-[700px]">
          <div class="calendly-inline-widget h-full" :data-url="property.calendlyUrl"></div>
        </div>
      </div>
    </section>

    <section class="bg-gray-100 text-gray-500 py-8 text-center" >  
      Made with ❤️ by Pieter
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
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




// Convert data() to refs
const mainImage = ref(living1)
const images = ref([
  buiten1,
  buiten2,
  buiten3,
  buiten4,
  buiten5,
  keuken1,
  keuken2,
  keuken3,
  living1,
  living2,
  living3,
  living4,
  living5,
  living6,
])

const property = ref({
  title: 'Gezellig appartement op toplocatie in Gent',
  price: 295000,
  bedrooms: 1,
  bathrooms: 1,
  area: 59,
  location: 'Ferdinand Lousbergkaai 101/003, 9000 Gent',
  description: 'Met veel plezier hebben wij hier als koppel gewoond. Een warm, licht en gezellig appartement waar we mooie herinneringen hebben opgebouwd. Nu er een baby op komst is, verhuizen we naar iets ruimer, en nemen we met pijn in het hart afscheid van deze plek. We hopen dat de volgende bewoners er net zo graag zullen wonen als wij. Kom gerust eens kijken — misschien wordt dit ook jouw nieuwe thuis.',
  videoUrl: 'https://www.youtube.com/embed/your-video-id',
  mapUrl: 'https://www.google.com/maps/embed?pb=your-location-embed-code',
  calendlyUrl: 'https://calendly.com/gistelinckpieter/bezoek-appartement',
  available_from: '14 juli 2025',
  parking: {
    title: 'Parking',
    description: 'Parking ook te koop.',
    image: '/images/surroundings/1.jpg',
  },
  why: [{
    title: 'Dicht bij de natuur',
    description: 'Het parkje achter het appartement is een perfecte plek om te genieten van de zon.',
  },
  { 
    title: 'Dicht bij de stad',
    description: 'Werk of studeer in Gent? Dit appartement is de perfecte keuze.'  
  },
  {
    title: 'Snelle toegang tot autostrades',
    description: 'Moet je de baan op voor je werk? Dan is dit appartement de perfecte keuze.'
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
    image: '/images/surroundings/1.jpg',
  },
  {
    title: 'Koningin Astridpark',
    description: 'De Koningin Astridpark is een van de grootste parken in Gent. Het is een perfecte plek om te wandelen of te fietsen.',
    image: koninginAstridpark,
  },
  {
    title: 'Buurtwinkel',
    description: 'De buurtwinkel is een van de grootste winkels in Gent. Het is een perfecte plek om te eten of te kopen.',
    image: '/images/surroundings/1.jpg',
  }],
  for_whom: [{
    title: 'Studenten',
    description: 'De jongeren zijn een van de grootste groepen in Gent. Het is een perfecte plek om te eten of te kopen.'
  },
  {
    title: 'Investeerders',
    description: 'De familie is een van de grootste groepen in Gent. Het is een perfecte plek om te eten of te kopen.'
  },
  {
    title: 'Singles',
    description: 'De singles zijn een van de grootste groepen in Gent. Het is een perfecte plek om te eten of te kopen.'
  },
  {
    title: 'Koppels',
    description: 'De koppel is een van de grootste groepen in Gent. Het is een perfecte plek om te eten of te kopen.'
  }]
})

// Convert methods to functions
const setMainImage = (image) => {
  mainImage.value = image
}

// Convert mounted hook
onMounted(() => {
  // Load Calendly script
  const script = document.createElement('script')
  script.src = 'https://assets.calendly.com/assets/external/widget.js'
  script.async = true
  document.head.appendChild(script)
})

// Add modal state
const showModal = ref(false)
</script>

<style scoped>
.apartment-listing {
  font-family: 'Inter', sans-serif;
}

.hero-section {
  position: relative;
  height: 70vh;
  overflow: hidden;
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