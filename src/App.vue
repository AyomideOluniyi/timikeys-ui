<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-rose-900 relative overflow-hidden">
    <!-- Animated Background Elements -->
    <div class="absolute inset-0">
      <!-- Gradient Orbs -->
      <div class="absolute top-20 left-20 w-96 h-96 bg-gradient-to-r from-purple-400/20 to-pink-400/20 rounded-full blur-3xl animate-pulse"></div>
      <div class="absolute bottom-20 right-20 w-80 h-80 bg-gradient-to-r from-cyan-400/20 to-blue-400/20 rounded-full blur-3xl animate-pulse" style="animation-delay: 1s;"></div>
      <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-64 h-64 bg-gradient-to-r from-emerald-400/20 to-teal-400/20 rounded-full blur-3xl animate-pulse" style="animation-delay: 2s;"></div>
      
      <!-- Floating Musical Notes -->
      <div v-for="i in 12" :key="i" 
           :class="`absolute text-white/10 text-2xl animate-float`"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 5 + 's',
             animationDuration: (3 + Math.random() * 4) + 's'
           }">
        {{ ['♪', '♫', '♬', '♩', '♭', '♯'][Math.floor(Math.random() * 6)] }}
      </div>
      
      <!-- Grid Pattern -->
      <div class="absolute inset-0 opacity-5">
        <div class="h-full w-full" style="background-image: linear-gradient(rgba(255,255,255,0.1) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,0.1) 1px, transparent 1px); background-size: 40px 40px;"></div>
      </div>
    </div>

    <!-- Main Content -->
    <div class="relative z-10 min-h-screen flex flex-col">
      <!-- Header -->
      <header class="p-8 text-center">
        <div class="mb-8">
          <h1 class="text-6xl font-bold bg-gradient-to-r from-white via-purple-200 to-pink-200 bg-clip-text text-transparent mb-4 tracking-tight">
            TimiKeys
          </h1>
          <p class="text-white/60 text-lg">Discover music through the art of piano</p>
        </div>
        
        <!-- Enhanced Search Bar -->
        <div class="max-w-2xl mx-auto relative">
          <div class="relative group">
            <div class="absolute inset-0 bg-gradient-to-r from-purple-500/20 via-pink-500/20 to-cyan-500/20 rounded-2xl blur-xl group-hover:blur-2xl transition-all duration-500"></div>
            <div class="relative backdrop-blur-xl bg-white/10 border border-white/20 rounded-2xl p-1 shadow-2xl">
              <div class="flex items-center">
                <div class="pl-6 pr-4">
                  <svg class="w-6 h-6 text-white/50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
                  </svg>
                </div>
                <input
                  v-model="searchTerm"
                  @focus="showSuggestions = true"
                  @blur="hideSuggestions"
                  type="text"
                  placeholder="Search for songs, artists, or albums..."
                  class="flex-1 bg-transparent text-white placeholder-white/50 text-lg py-4 pr-6 focus:outline-none"
                />
                <div class="pr-4">
                  <div class="w-3 h-3 bg-gradient-to-r from-green-400 to-emerald-400 rounded-full animate-pulse shadow-lg shadow-green-400/50"></div>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Enhanced Search Suggestions -->
          <div v-if="showSuggestions && searchSuggestions.length > 0" 
               class="absolute z-20 w-full mt-4 backdrop-blur-2xl bg-white/10 border border-white/20 rounded-2xl shadow-2xl max-h-80 overflow-auto">
            <div v-for="suggestion in searchSuggestions" 
                 :key="suggestion.id"
                 @mousedown="selectSuggestion(suggestion)"
                 class="px-6 py-4 hover:bg-white/10 cursor-pointer transition-all duration-300 flex items-center space-x-4 border-b border-white/10 last:border-b-0 group">
              <div class="relative">
                <img :src="suggestion.coverUrl" :alt="suggestion.song" class="w-14 h-14 rounded-xl object-cover shadow-lg">
                <div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent rounded-xl"></div>
              </div>
              <div class="flex-1">
                <div class="text-white font-semibold text-lg group-hover:text-purple-200 transition-colors">{{ suggestion.song }}</div>
                <div class="text-white/60">{{ suggestion.artist }}</div>
              </div>
              <div class="text-sm text-purple-300 bg-purple-500/20 px-3 py-1 rounded-full border border-purple-400/30">
                {{ suggestion.genre }}
              </div>
            </div>
          </div>
        </div>
      </header>

      <!-- Main Piano Interface -->
      <main class="flex-1 flex items-center justify-center px-8 pb-16">
        <div class="w-full max-w-7xl">
          <!-- Genre Bubbles Container -->
          <div class="relative">
            <!-- Piano Container -->
            <div class="relative mx-auto" style="width: fit-content;">
              <!-- Piano Background Glow -->
              <div class="absolute inset-0 bg-gradient-to-r from-purple-500/20 via-pink-500/20 to-cyan-500/20 rounded-3xl blur-2xl transform scale-110"></div>
              
              <!-- Piano Interface -->
              <div class="relative backdrop-blur-2xl bg-white/10 border border-white/20 rounded-3xl p-8 shadow-2xl">
                <div class="text-center mb-6">
                  <h2 class="text-2xl font-bold text-white mb-2">Piano Discovery</h2>
                  <p class="text-white/60">Touch a key to explore artists</p>
                </div>
                
                <!-- Piano Keyboard -->
                <div class="relative flex justify-center">
                  <div class="flex relative" style="height: 280px;">
                    <!-- White Keys -->
                    <div v-for="(key, index) in pianoKeys.white" 
                         :key="'white-' + index"
                         @click="playKey(key)"
                         @mouseenter="hoverKey(key)"
                         @mouseleave="unhoverKey(key)"
                         :class="[
                           'relative bg-gradient-to-b from-gray-50 to-gray-200 border-2 border-gray-300 cursor-pointer transition-all duration-300 flex flex-col items-center justify-end pb-6 text-gray-800 font-bold text-sm shadow-lg hover:shadow-2xl hover:shadow-purple-500/30 group',
                           activeKeys.includes(key.note) ? 'from-purple-200 to-purple-400 border-purple-500 shadow-2xl shadow-purple-500/50 transform scale-95' : '',
                           hoveredKey === key.note ? 'from-cyan-100 to-cyan-300 border-cyan-400 shadow-xl shadow-cyan-400/40' : '',
                           selectedArtists.includes(key.artist) ? 'from-pink-200 to-pink-400 border-pink-500 shadow-xl shadow-pink-500/50' : ''
                         ]"
                         style="width: 80px; height: 280px; margin-right: 2px;">
                      <div class="text-lg font-bold mb-2 group-hover:text-purple-600 transition-colors">{{ key.note }}</div>
                      <div class="text-xs text-center leading-tight opacity-70 group-hover:opacity-100 transition-opacity px-2">{{ key.artist }}</div>
                      
                      <!-- Key Glow Effect -->
                      <div v-if="activeKeys.includes(key.note)" 
                           class="absolute inset-0 bg-gradient-to-t from-purple-400/30 to-transparent rounded-b-lg pointer-events-none"></div>
                    </div>
                    
                    <!-- Black Keys -->
                    <div v-for="(key, index) in pianoKeys.black" 
                         :key="'black-' + index"
                         @click="playKey(key)"
                         @mouseenter="hoverKey(key)"
                         @mouseleave="unhoverKey(key)"
                         :class="[
                           'absolute bg-gradient-to-b from-gray-800 to-gray-900 border-2 border-gray-700 cursor-pointer transition-all duration-300 flex flex-col items-center justify-end pb-4 text-white font-bold text-xs shadow-xl hover:shadow-2xl hover:shadow-purple-500/40 group',
                           activeKeys.includes(key.note) ? 'from-purple-600 to-purple-800 border-purple-400 shadow-2xl shadow-purple-500/60 transform scale-95' : '',
                           hoveredKey === key.note ? 'from-cyan-600 to-cyan-800 border-cyan-400 shadow-xl shadow-cyan-400/50' : '',
                           selectedArtists.includes(key.artist) ? 'from-pink-600 to-pink-800 border-pink-400 shadow-xl shadow-pink-500/60' : ''
                         ]"
                         :style="{
                           width: '50px',
                           height: '180px',
                           left: key.position + 'px',
                           zIndex: 10
                         }">
                      <div class="text-sm font-bold mb-1 group-hover:text-purple-200 transition-colors">{{ key.note }}</div>
                      <div class="text-xs text-center leading-tight opacity-70 group-hover:opacity-100 transition-opacity px-1">{{ key.artist }}</div>
                      
                      <!-- Key Glow Effect -->
                      <div v-if="activeKeys.includes(key.note)" 
                           class="absolute inset-0 bg-gradient-to-t from-purple-500/40 to-transparent rounded-b-lg pointer-events-none"></div>
                    </div>
                  </div>
                </div>
                
                <!-- Piano Controls -->
                <div class="mt-8 flex justify-center space-x-4">
                  <button @click="clearAll" 
                          class="px-6 py-3 bg-gradient-to-r from-red-500/20 to-red-600/20 border border-red-500/30 text-red-300 rounded-xl hover:from-red-500/30 hover:to-red-600/30 transition-all duration-300 font-medium shadow-lg hover:shadow-red-500/20">
                    Clear All
                  </button>
                  <button @click="playRandomChord" 
                          class="px-6 py-3 bg-gradient-to-r from-green-500/20 to-emerald-600/20 border border-green-500/30 text-green-300 rounded-xl hover:from-green-500/30 hover:to-emerald-600/30 transition-all duration-300 font-medium shadow-lg hover:shadow-green-500/20">
                    Random Chord
                  </button>
                  <button @click="showAllArtists" 
                          class="px-6 py-3 bg-gradient-to-r from-blue-500/20 to-cyan-600/20 border border-blue-500/30 text-blue-300 rounded-xl hover:from-blue-500/30 hover:to-cyan-600/30 transition-all duration-300 font-medium shadow-lg hover:shadow-blue-500/20">
                    Show All
                  </button>
                </div>
              </div>
            </div>

            <!-- Floating Genre Bubbles -->
            <div class="absolute inset-0 pointer-events-none">
              <!-- Left Side Genres -->
              <div class="absolute left-0 top-1/2 transform -translate-y-1/2 -translate-x-32 space-y-6">
                <div v-for="(genre, index) in leftGenres" 
                     :key="'left-' + genre"
                     @click="toggleGenre(genre)"
                     :class="[
                       'pointer-events-auto cursor-pointer transition-all duration-500 transform hover:scale-110 group',
                       selectedGenres.includes(genre) ? 'scale-110' : ''
                     ]"
                     :style="{ animationDelay: index * 0.2 + 's' }"
                     class="animate-float">
                  <div class="relative">
                    <div :class="[
                      'w-32 h-32 rounded-full flex items-center justify-center text-white font-bold text-sm text-center shadow-2xl transition-all duration-500',
                      selectedGenres.includes(genre) 
                        ? 'bg-gradient-to-br from-purple-500 to-pink-500 shadow-purple-500/50' 
                        : 'bg-gradient-to-br from-purple-500/30 to-pink-500/30 backdrop-blur-xl border border-white/20 hover:from-purple-500/50 hover:to-pink-500/50'
                    ]">
                      {{ genre }}
                    </div>
                    <div v-if="selectedGenres.includes(genre)" 
                         class="absolute inset-0 bg-gradient-to-br from-purple-500/20 to-pink-500/20 rounded-full blur-xl animate-pulse"></div>
                  </div>
                </div>
              </div>
              
              <!-- Right Side Genres -->
              <div class="absolute right-0 top-1/2 transform -translate-y-1/2 translate-x-32 space-y-6">
                <div v-for="(genre, index) in rightGenres" 
                     :key="'right-' + genre"
                     @click="toggleGenre(genre)"
                     :class="[
                       'pointer-events-auto cursor-pointer transition-all duration-500 transform hover:scale-110 group',
                       selectedGenres.includes(genre) ? 'scale-110' : ''
                     ]"
                     :style="{ animationDelay: (index + 3) * 0.2 + 's' }"
                     class="animate-float">
                  <div class="relative">
                    <div :class="[
                      'w-32 h-32 rounded-full flex items-center justify-center text-white font-bold text-sm text-center shadow-2xl transition-all duration-500',
                      selectedGenres.includes(genre) 
                        ? 'bg-gradient-to-br from-cyan-500 to-blue-500 shadow-cyan-500/50' 
                        : 'bg-gradient-to-br from-cyan-500/30 to-blue-500/30 backdrop-blur-xl border border-white/20 hover:from-cyan-500/50 hover:to-blue-500/50'
                    ]">
                      {{ genre }}
                    </div>
                    <div v-if="selectedGenres.includes(genre)" 
                         class="absolute inset-0 bg-gradient-to-br from-cyan-500/20 to-blue-500/20 rounded-full blur-xl animate-pulse"></div>
                  </div>
                </div>
              </div>
              
              <!-- Top Genres -->
              <div class="absolute top-0 left-1/2 transform -translate-x-1/2 -translate-y-20 flex space-x-8">
                <div v-for="(genre, index) in topGenres" 
                     :key="'top-' + genre"
                     @click="toggleGenre(genre)"
                     :class="[
                       'pointer-events-auto cursor-pointer transition-all duration-500 transform hover:scale-110 group',
                       selectedGenres.includes(genre) ? 'scale-110' : ''
                     ]"
                     :style="{ animationDelay: (index + 6) * 0.2 + 's' }"
                     class="animate-float">
                  <div class="relative">
                    <div :class="[
                      'w-28 h-28 rounded-full flex items-center justify-center text-white font-bold text-sm text-center shadow-2xl transition-all duration-500',
                      selectedGenres.includes(genre) 
                        ? 'bg-gradient-to-br from-emerald-500 to-teal-500 shadow-emerald-500/50' 
                        : 'bg-gradient-to-br from-emerald-500/30 to-teal-500/30 backdrop-blur-xl border border-white/20 hover:from-emerald-500/50 hover:to-teal-500/50'
                    ]">
                      {{ genre }}
                    </div>
                    <div v-if="selectedGenres.includes(genre)" 
                         class="absolute inset-0 bg-gradient-to-br from-emerald-500/20 to-teal-500/20 rounded-full blur-xl animate-pulse"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>

      <!-- Selected Artists Display -->
      <div v-if="selectedArtists.length > 0" class="px-8 pb-8">
        <div class="max-w-4xl mx-auto backdrop-blur-2xl bg-white/10 border border-white/20 rounded-2xl p-6 shadow-2xl">
          <h3 class="text-white font-bold text-xl mb-4 text-center">Selected Artists</h3>
          <div class="flex flex-wrap justify-center gap-3">
            <div v-for="artist in selectedArtists" 
                 :key="artist"
                 class="px-4 py-2 bg-gradient-to-r from-purple-500/30 to-pink-500/30 border border-purple-400/30 text-white rounded-full text-sm font-medium backdrop-blur-sm shadow-lg">
              {{ artist }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'

export default {
  name: 'EnhancedTimiKeysPiano',
  setup() {
    // Reactive data
    const searchTerm = ref('')
    const selectedGenres = ref([])
    const selectedArtists = ref([])
    const showSuggestions = ref(false)
    const activeKeys = ref([])
    const hoveredKey = ref(null)

    // Piano keyboard configuration
    const pianoKeys = {
      white: [
        { note: 'C', artist: 'Drake' },
        { note: 'D', artist: 'Ariana Grande' },
        { note: 'E', artist: 'Kendrick Lamar' },
        { note: 'F', artist: 'Frank Ocean' },
        { note: 'G', artist: 'Tyler, The Creator' },
        { note: 'A', artist: 'Migos' },
        { note: 'B', artist: 'Young Thug' }
      ],
      black: [
        { note: 'C#', artist: 'Jay-Z', position: 57 },
        { note: 'D#', artist: 'Nas', position: 139 },
        { note: 'F#', artist: 'Billie Eilish', position: 303 },
        { note: 'G#', artist: 'Daniel Caesar', position: 385 },
        { note: 'A#', artist: 'SZA', position: 467 }
      ]
    }

    // Genre organization
    const leftGenres = ['Hip Hop', 'R&B', 'Neo Soul']
    const rightGenres = ['Rock', 'Indie', 'Rap']
    const topGenres = ['Theme Tunes', 'Pop']

    const allGenres = [...leftGenres, ...rightGenres, ...topGenres]

    // Sample tracks data
    const tracks = ref([
      {
        id: 1,
        song: "God's Plan",
        artist: "Drake",
        genre: "Hip Hop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 2,
        song: "Thank U, Next",
        artist: "Ariana Grande",
        genre: "Pop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 3,
        song: "HUMBLE.",
        artist: "Kendrick Lamar",
        genre: "Hip Hop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 4,
        song: "Thinkin Bout You",
        artist: "Frank Ocean",
        genre: "R&B",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 5,
        song: "EARFQUAKE",
        artist: "Tyler, The Creator",
        genre: "Hip Hop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 6,
        song: "Bad and Boujee",
        artist: "Migos",
        genre: "Hip Hop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 7,
        song: "The Story of O.J.",
        artist: "Jay-Z",
        genre: "Hip Hop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      },
      {
        id: 8,
        song: "Bad Guy",
        artist: "Billie Eilish",
        genre: "Pop",
        coverUrl: "/placeholder.svg?height=200&width=200"
      }
    ])

    // Computed properties
    const searchSuggestions = computed(() => {
      if (!searchTerm.value.trim()) return []
      
      return tracks.value
        .filter(track => 
          track.song.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
          track.artist.toLowerCase().includes(searchTerm.value.toLowerCase())
        )
        .slice(0, 5)
    })

    // Methods
    const playKey = (key) => {
      activeKeys.value.push(key.note)
      
      // Toggle artist selection
      const artistIndex = selectedArtists.value.indexOf(key.artist)
      if (artistIndex > -1) {
        selectedArtists.value.splice(artistIndex, 1)
      } else {
        selectedArtists.value.push(key.artist)
      }
      
      // Remove active key after animation
      setTimeout(() => {
        const index = activeKeys.value.indexOf(key.note)
        if (index > -1) {
          activeKeys.value.splice(index, 1)
        }
      }, 300)
    }

    const hoverKey = (key) => {
      hoveredKey.value = key.note
    }

    const unhoverKey = (key) => {
      hoveredKey.value = null
    }

    const toggleGenre = (genre) => {
      const index = selectedGenres.value.indexOf(genre)
      if (index > -1) {
        selectedGenres.value.splice(index, 1)
      } else {
        selectedGenres.value.push(genre)
      }
    }

    const selectSuggestion = (suggestion) => {
      searchTerm.value = suggestion.song
      showSuggestions.value = false
    }

    const hideSuggestions = () => {
      setTimeout(() => {
        showSuggestions.value = false
      }, 200)
    }

    const clearAll = () => {
      selectedArtists.value = []
      selectedGenres.value = []
      activeKeys.value = []
    }

    const playRandomChord = () => {
      const randomKeys = []
      const allKeys = [...pianoKeys.white, ...pianoKeys.black]
      
      // Select 3 random keys
      for (let i = 0; i < 3; i++) {
        const randomKey = allKeys[Math.floor(Math.random() * allKeys.length)]
        if (!randomKeys.includes(randomKey)) {
          randomKeys.push(randomKey)
        }
      }
      
      // Play them with slight delay
      randomKeys.forEach((key, index) => {
        setTimeout(() => {
          playKey(key)
        }, index * 150)
      })
    }

    const showAllArtists = () => {
      const allArtists = [...pianoKeys.white.map(k => k.artist), ...pianoKeys.black.map(k => k.artist)]
      selectedArtists.value = [...allArtists]
    }

    return {
      searchTerm,
      selectedGenres,
      selectedArtists,
      showSuggestions,
      activeKeys,
      hoveredKey,
      pianoKeys,
      leftGenres,
      rightGenres,
      topGenres,
      allGenres,
      tracks,
      searchSuggestions,
      playKey,
      hoverKey,
      unhoverKey,
      toggleGenre,
      selectSuggestion,
      hideSuggestions,
      clearAll,
      playRandomChord,
      showAllArtists
    }
  }
}
</script>

<style scoped>
/* Enhanced animations */
@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  25% { transform: translateY(-10px) rotate(1deg); }
  50% { transform: translateY(-5px) rotate(0deg); }
  75% { transform: translateY(-15px) rotate(-1deg); }
}

.animate-float {
  animation: float 6s ease-in-out infinite;
}

/* Enhanced glassmorphism */
.backdrop-blur-2xl {
  backdrop-filter: blur(40px);
  -webkit-backdrop-filter: blur(40px);
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(to bottom, #8b5cf6, #ec4899);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(to bottom, #7c3aed, #db2777);
}

/* Text gradient */
.bg-clip-text {
  background-clip: text;
  -webkit-background-clip: text;
}

/* Enhanced hover effects */
.group:hover .group-hover\:scale-110 {
  transform: scale(1.1);
}

/* Smooth transitions */
* {
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}

/* Piano key shadows */
.shadow-2xl {
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}

/* Glow effects */
.shadow-purple-500\/50 {
  box-shadow: 0 0 30px rgba(168, 85, 247, 0.5);
}

.shadow-cyan-500\/50 {
  box-shadow: 0 0 30px rgba(6, 182, 212, 0.5);
}

.shadow-pink-500\/50 {
  box-shadow: 0 0 30px rgba(236, 72, 153, 0.5);
}

.shadow-emerald-500\/50 {
  box-shadow: 0 0 30px rgba(16, 185, 129, 0.5);
}

/* Blur effects for background orbs */
.blur-3xl {
  filter: blur(64px);
}
</style>