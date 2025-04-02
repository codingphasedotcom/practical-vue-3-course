<template>
  <div class="min-h-screen bg-gradient-to-br from-green-900 to-green-700 p-3 sm:p-6 md:p-10 flex items-center justify-center">
    <div class="w-full max-w-[1400px] backdrop-blur-xl bg-black/40 rounded-3xl shadow-2xl border border-white/10 overflow-hidden">
      <!-- Mobile Header - Only visible on small screens -->
      <div class="lg:hidden p-4 border-b border-white/10">
        <div class="flex justify-between items-center">
          <button 
            @click="navigatePokemon(-1)"
            class="text-white/60 hover:text-white transition-colors cursor-pointer p-2"
          >
            <ArrowLeft class="w-5 h-5" />
          </button>
          <h1 class="text-xl font-bold text-white">Pokédex</h1>
          <button 
            @click="navigatePokemon(1)"
            class="text-white/60 hover:text-white transition-colors cursor-pointer p-2"
          >
            <ArrowRight class="w-5 h-5" />
          </button>
        </div>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-12">
        <!-- Left Sidebar - Search Section -->
        <div class="lg:col-span-3 border-r border-white/10 lg:min-h-[800px] overflow-y-auto max-h-[50vh] lg:max-h-none">
          <div class="p-4 sm:p-6">
            


            <!-- Search Heading -->
            <h1 class="text-xl sm:text-2xl font-bold text-white mb-4 sm:mb-6">
              What Pokémon are<br>you looking for?
            </h1>

            <!-- Search Bar -->
            <div class="bg-white/10 rounded-full flex items-center p-2 sm:p-3 mb-4 sm:mb-6">
              <Search class="w-4 h-4 sm:w-5 sm:h-5 text-white/60 mr-2" />
              <input 
                v-model="searchQuery"
                type="text" 
                placeholder="Search Pokémon" 
                class="bg-transparent text-white outline-none w-full placeholder-white/60 text-sm sm:text-base"
                @keyup.enter="searchPokemon"
              />
              <button 
                @click="searchPokemon" 
                class="bg-white/20 rounded-full p-1 hover:bg-white/30 transition-colors cursor-pointer"
              >
                <Search class="w-3 h-3 sm:w-4 sm:h-4 text-white" />
              </button>
            </div>

            <!-- Add error message -->
            <div 
              v-if="searchError" 
              class="mb-4 sm:mb-6 p-3 bg-red-500/20 border border-red-500/30 rounded-xl text-red-300 text-sm animate-fade-in"
            >
              {{ searchError }}
            </div>

            <!-- Update the Recent Searches section -->
            <!-- Find the Recent Searches heading and section and wrap them in a hidden div -->
            <div class="hidden sm:block"> <!-- This will hide on mobile but show on sm and up -->
              <h2 class="text-lg sm:text-xl font-bold text-white mb-3 sm:mb-4">Recent</h2>
              <div class="space-y-2 sm:space-y-3">
                <div 
                  v-for="pokemon in recentSearches" 
                  :key="pokemon.id"
                  class="flex items-center gap-2 sm:gap-3 p-2 sm:p-3 rounded-xl hover:bg-white/10 cursor-pointer transition-colors"
                  @click="selectPokemon(pokemon)"
                >
                  <img 
                    :src="pokemon.image" 
                    :alt="pokemon.name" 
                    class="w-10 h-10 sm:w-12 sm:h-12 object-contain"
                  />
                  <div>
                    <div class="text-white font-medium capitalize text-sm sm:text-base">{{ pokemon.name }}</div>
                    <div class="text-white/60 text-xs sm:text-sm">#{{ pokemon.id.toString().padStart(3, '0') }}</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Main Content Area -->
        <div class="lg:col-span-9 overflow-y-auto max-h-[calc(100vh-80px)] lg:max-h-none" v-if="selectedPokemon">
          <div class="grid grid-cols-1 lg:grid-cols-2 h-full">
            <!-- Pokemon Image Section -->
            <div class="p-4 sm:p-6 md:p-8 flex flex-col">
              <!-- Desktop Header - Only visible on large screens -->
              <div class="hidden lg:flex justify-between items-center mb-4">
                <!-- Left arrow navigation -->
                <button 
                  @click="navigatePokemon(-1)" 
                  class="flex items-center gap-2 text-white/60 hover:text-white transition-colors px-4 py-2 rounded-lg hover:bg-white/10 cursor-pointer"
                >
                  <ArrowLeft class="w-5 h-5" />
                  <span v-if="previousPokemon" class="text-sm capitalize">{{ previousPokemon.name }}</span>
                </button>

                <!-- Right arrow navigation -->
                <button 
                  @click="navigatePokemon(1)"
                  class="flex items-center gap-2 text-white/60 hover:text-white transition-colors px-4 py-2 rounded-lg hover:bg-white/10 cursor-pointer"
                >
                  <span v-if="nextPokemon" class="text-sm capitalize">{{ nextPokemon.name }}</span>
                  <ArrowRight class="w-5 h-5" />
                </button>
              </div>

              <!-- Pokemon Image -->
              <div class="flex-1 relative flex items-center justify-center py-6 sm:py-8">
                <div class="absolute inset-0 bg-gradient-to-b from-green-500/30 to-green-700/30 rounded-full blur-3xl"></div>
                <img 
                  :src="selectedPokemon.image" 
                  :alt="selectedPokemon.name" 
                  class="w-48 h-48 sm:w-64 sm:h-64 md:w-80 md:h-80 object-contain z-10"
                />
              </div>

              <!-- Pokemon Name and Type -->
              <div class="text-center mt-2 sm:mt-4">
                <h1 class="text-2xl sm:text-3xl md:text-4xl font-bold text-white mb-1 capitalize">{{ selectedPokemon.name }}</h1>
                <p class="text-white/70 mb-3 sm:mb-4">{{ selectedPokemon.category }}</p>

                <!-- Type Tags -->
                <div class="flex justify-center gap-2 sm:gap-3 mb-4 sm:mb-6 flex-wrap">
                  <div 
                    v-for="(type, index) in selectedPokemon.types" 
                    :key="index"
                    class="rounded-full px-3 sm:px-5 py-1 sm:py-1.5 flex items-center"
                    :class="getTypeColor(type)"
                  >
                    <div class="w-3 h-3 sm:w-4 sm:h-4 mr-1 sm:mr-2 bg-white/20 rounded-full"></div>
                    <span class="text-white capitalize text-sm sm:text-base">{{ type }}</span>
                  </div>
                </div>
              </div>
            </div>

            <!-- Pokemon Details Section -->
            <div class="border-t lg:border-t-0 lg:border-l border-white/10">
              <!-- Tabs -->
              <div class="flex border-b border-white/10 overflow-x-auto scrollbar-hide">
                <button 
                  v-for="tab in tabs" 
                  :key="tab"
                  class="py-3 sm:py-4 px-3 sm:px-6 font-medium transition-colors whitespace-nowrap cursor-pointer hover:bg-white/5"
                  :class="activeTab === tab ? 'text-white border-b-2 border-white' : 'text-white/60'"
                  @click="activeTab = tab"
                >
                  {{ tab.charAt(0).toUpperCase() + tab.slice(1) }}
                </button>
              </div>

              <!-- Tab Content -->
              <div class="p-4 sm:p-6 md:p-8">
                <!-- About Tab -->
                <div v-if="activeTab === 'about'" class="text-white/80">
                  <p class="mb-4 sm:mb-6 text-base sm:text-lg">
                    {{ selectedPokemon.description }}
                  </p>
                  
                  <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 sm:gap-6 mb-6 sm:mb-8">
                    <div>
                      <h3 class="text-white/60 mb-2">Height</h3>
                      <div class="flex items-center">
                        <Ruler class="w-4 h-4 sm:w-5 sm:h-5 mr-2 text-white/60" />
                        <span class="text-white text-base sm:text-lg">{{ selectedPokemon.height }}</span>
                      </div>
                    </div>
                    <div>
                      <h3 class="text-white/60 mb-2">Weight</h3>
                      <div class="flex items-center">
                        <Weight class="w-4 h-4 sm:w-5 sm:h-5 mr-2 text-white/60" />
                        <span class="text-white text-base sm:text-lg">{{ selectedPokemon.weight }}</span>
                      </div>
                    </div>
                  </div>

                  <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 sm:gap-6">
                    <div>
                      <h3 class="text-white/60 mb-2">Abilities</h3>
                      <ul class="space-y-1 sm:space-y-2">
                        <li v-for="(ability, index) in selectedPokemon.abilities" :key="index" class="text-white text-sm sm:text-base">
                          {{ ability }}
                        </li>
                      </ul>
                    </div>
                    <div>
                      <h3 class="text-white/60 mb-2">Breeding</h3>
                      <div class="mb-2">
                        <div class="text-white/60 text-xs sm:text-sm">Gender</div>
                        <div class="flex items-center gap-3">
                          <div class="flex items-center">
                            <div class="text-blue-400 mr-1">♂</div>
                            <span class="text-white text-sm sm:text-base">87.5%</span>
                          </div>
                          <div class="flex items-center">
                            <div class="text-pink-400 mr-1">♀</div>
                            <span class="text-white text-sm sm:text-base">12.5%</span>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Stats Tab -->
                <div v-if="activeTab === 'stats'" class="text-white/80">
                  <div class="space-y-4 sm:space-y-6 mb-6 sm:mb-8">
                    <div v-for="(stat, index) in selectedPokemon.stats" :key="index" class="flex items-center">
                      <span class="text-white/80 w-16 sm:w-24 text-sm sm:text-base">{{ formatStatName(stat.name) }}</span>
                      <span class="text-white font-medium w-8 sm:w-12 text-right text-sm sm:text-base">{{ stat.value }}</span>
                      <div class="flex-1 ml-2 sm:ml-4">
                        <div class="h-2 sm:h-3 bg-white/10 rounded-full overflow-hidden">
                          <div 
                            class="h-full rounded-full" 
                            :class="getStatColor(stat.name, stat.value)"
                            :style="{ width: `${(stat.value / 255) * 100}%` }"
                          ></div>
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- Description -->
                  <div class="mb-6 sm:mb-8 p-3 sm:p-4 bg-white/5 rounded-xl">
                    <p class="text-white/80 text-sm sm:text-base">
                      Based on this pokemon's stats we consider the best nature for {{ selectedPokemon.name }} to have is 
                      <span class="text-white font-medium">Sassy</span>, this will increase it's 
                      <span class="text-white font-medium">Sp. Def</span> and decrease it's 
                      <span class="text-white font-medium">Speed</span> stats.
                    </p>
                  </div>

                  <!-- Type Defenses -->
                  <div>
                    <h3 class="text-white font-medium text-lg sm:text-xl mb-1 sm:mb-2">Type Defenses</h3>
                    <p class="text-white/60 mb-3 sm:mb-4 text-xs sm:text-sm">
                      The effectiveness of each type on {{ selectedPokemon.name }}.
                    </p>
                    
                    <!-- Weaknesses -->
                    <div v-if="organizedTypeDefenses.weaknesses.length > 0" class="mb-6">
                      <h4 class="text-red-400 font-medium mb-2">Weak Against</h4>
                      <div class="grid grid-cols-4 sm:grid-cols-4 md:grid-cols-8 gap-2 sm:gap-3">
                        <div 
                          v-for="type in organizedTypeDefenses.weaknesses" 
                          :key="type.name" 
                          class="text-center"
                        >
                          <div 
                            class="w-10 h-10 sm:w-12 sm:h-12 mx-auto mb-1 rounded-full flex items-center justify-center" 
                            :class="getEffectivenessColor(type.effectiveness)"
                          >
                            <span class="text-white font-bold text-sm sm:text-base">
                              {{ type.effectiveness }}
                            </span>
                          </div>
                          <div class="text-white/80 text-xs capitalize">{{ type.name }}</div>
                        </div>
                      </div>
                    </div>

                    <!-- Normal -->
                    <div v-if="organizedTypeDefenses.normal.length > 0" class="mb-6">
                      <h4 class="text-gray-400 font-medium mb-2">Normal Damage</h4>
                      <div class="grid grid-cols-4 sm:grid-cols-4 md:grid-cols-8 gap-2 sm:gap-3">
                        <div 
                          v-for="type in organizedTypeDefenses.normal" 
                          :key="type.name" 
                          class="text-center"
                        >
                          <div 
                            class="w-10 h-10 sm:w-12 sm:h-12 mx-auto mb-1 rounded-full flex items-center justify-center" 
                            :class="getTypeColor(type.name)"
                          >
                            <span class="text-white font-bold text-sm sm:text-base">
                              {{ type.effectiveness }}
                            </span>
                          </div>
                          <div class="text-white/80 text-xs capitalize">{{ type.name }}</div>
                        </div>
                      </div>
                    </div>

                    <!-- Resistances -->
                    <div v-if="organizedTypeDefenses.resistances.length > 0">
                      <h4 class="text-green-400 font-medium mb-2">Resistant Against</h4>
                      <div class="grid grid-cols-4 sm:grid-cols-4 md:grid-cols-8 gap-2 sm:gap-3">
                        <div 
                          v-for="type in organizedTypeDefenses.resistances" 
                          :key="type.name" 
                          class="text-center"
                        >
                          <div 
                            class="w-10 h-10 sm:w-12 sm:h-12 mx-auto mb-1 rounded-full flex items-center justify-center" 
                            :class="getEffectivenessColor(type.effectiveness)"
                          >
                            <span class="text-white font-bold text-sm sm:text-base">
                              {{ type.effectiveness }}
                            </span>
                          </div>
                          <div class="text-white/80 text-xs capitalize">{{ type.name }}</div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Moves Tab -->
                <div v-if="activeTab === 'moves'" class="text-white/80">
                  <div class="space-y-3 sm:space-y-4">
                    <div v-for="(move, index) in selectedPokemon.moves" :key="index" class="p-3 sm:p-4 bg-white/5 rounded-xl">
                      <div class="flex justify-between items-center flex-wrap gap-2">
                        <div>
                          <h4 class="text-white font-medium capitalize text-sm sm:text-base">{{ move.name }}</h4>
                          <div class="text-white/60 text-xs sm:text-sm">Level {{ move.level }}</div>
                        </div>
                        <div class="px-2 sm:px-3 py-0.5 sm:py-1 rounded-lg text-white text-xs sm:text-sm" :class="getTypeColor(move.type)">
                          {{ move.type }}
                        </div>
                      </div>
                      <div class="mt-2 flex items-center gap-3 sm:gap-4">
                        <div class="flex items-center">
                          <Zap class="w-3 h-3 sm:w-4 sm:h-4 mr-1 text-white/60" />
                          <span class="text-sm sm:text-base">{{ move.power || '-' }}</span>
                        </div>
                        <div class="flex items-center">
                          <Target class="w-3 h-3 sm:w-4 sm:h-4 mr-1 text-white/60" />
                          <span class="text-sm sm:text-base">{{ move.accuracy || '-' }}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Evolutions Tab -->
                <div v-if="activeTab === 'evolutions'" class="text-white/80">
                  <div v-if="selectedPokemon.evolutions?.length" class="flex flex-col sm:flex-row justify-between items-center gap-6">
                    <div 
                      v-for="(evo, index) in selectedPokemon.evolutions" 
                      :key="index" 
                      class="text-center group cursor-pointer hover:scale-105 transition-all duration-200"
                      @click="loadEvolution(evo.id)"
                    >
                      <div class="relative">
                        <div class="absolute inset-0 bg-white/5 rounded-full opacity-0 group-hover:opacity-100 transition-opacity"></div>
                        <img 
                          :src="evo.image" 
                          :alt="evo.name" 
                          class="w-24 h-24 sm:w-32 sm:h-32 object-contain mx-auto relative z-10" 
                        />
                      </div>
                      <div class="text-white font-medium capitalize mt-2 text-sm sm:text-base">{{ evo.name }}</div>
                      <div class="text-white/60 text-xs sm:text-sm">#{{ evo.id.toString().padStart(3, '0') }}</div>
                      
                      <div v-if="index < selectedPokemon.evolutions.length - 1" class="hidden sm:flex items-center justify-center mt-4">
                        <ArrowRight class="w-5 h-5 sm:w-6 sm:h-6 text-white/60" />
                        <div class="text-white/60 text-xs sm:text-sm ml-2">
                          {{ evo.evolveLevel ? `Level ${evo.evolveLevel}` : 'Special' }}
                        </div>
                      </div>
                      
                      <div v-if="index < selectedPokemon.evolutions.length - 1" class="flex sm:hidden items-center justify-center mt-4 mb-4">
                        <ArrowDown class="w-5 h-5 text-white/60" />
                        <div class="text-white/60 text-xs ml-2">
                          {{ evo.evolveLevel ? `Level ${evo.evolveLevel}` : 'Special' }}
                        </div>
                      </div>
                    </div>
                  </div>
                  
                  <!-- No evolutions message -->
                  <div v-else class="text-center py-8">
                    <div class="text-white/60">This Pokémon has no evolutions.</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Empty State -->
        <div class="lg:col-span-9 flex items-center justify-center" v-if="!selectedPokemon">
          <div v-if="isLoading" class="text-center p-6 sm:p-10">
            <!-- Animated Pokeball -->
            <div class="w-16 h-16 sm:w-24 sm:h-24 mx-auto mb-4 sm:mb-6 animate-spin">
              <svg 
                viewBox="0 0 24 24" 
                class="w-full h-full text-white opacity-80"
              >
                <circle 
                  cx="12" 
                  cy="12" 
                  r="10" 
                  stroke="currentColor" 
                  stroke-width="2" 
                  fill="none"
                />
                <path 
                  d="M2 12h20" 
                  stroke="currentColor" 
                  stroke-width="2"
                />
                <circle 
                  cx="12" 
                  cy="12" 
                  r="3" 
                  stroke="currentColor" 
                  stroke-width="2"
                  fill="currentColor"
                />
              </svg>
            </div>
            <h2 class="text-xl sm:text-2xl font-bold text-white mb-1 sm:mb-2">Loading Pokémon...</h2>
            <p class="text-white/60 text-sm sm:text-base">Please wait while we catch it!</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, h, watch, onMounted } from 'vue'
import { 
  Search, 
  Zap, 
  GitBranch, 
  MapPin, 
  Heart, 
  Weight,
  Ruler,
  Target,
  ArrowRight,
  ArrowDown,
  BarChart,
  ArrowLeft
} from 'lucide-vue-next'

// Custom Pokeball icon component
const Pokeball = (props) => {
  return {
    render() {
      return h('svg', {
        xmlns: 'http://www.w3.org/2000/svg',
        width: '24',
        height: '24',
        viewBox: '0 0 24 24',
        fill: 'none',
        stroke: 'currentColor',
        'stroke-width': '2',
        'stroke-linecap': 'round',
        'stroke-linejoin': 'round',
        ...props
      }, [
        h('circle', { cx: '12', cy: '12', r: '10' }),
        h('path', { d: 'M2 12h20' }),
        h('circle', { cx: '12', cy: '12', r: '3' })
      ])
    }
  }
}

// State
const searchQuery = ref('')
const activeTab = ref('about')
const activeSection = ref('pokedex')
const isFavorite = ref(false)
const selectedPokemon = ref(null)
const showSearchResults = ref(false)
const searchHistory = ref([])
const searchError = ref('')
const isLoading = ref(true)

// Available tabs
const tabs = ['about', 'stats', 'moves', 'evolutions']

// Replace the pokemonDatabase and add these new functions
const API_BASE_URL = 'https://pokeapi.co/api/v2'

// Add this at the top of your script
const typeDataCache = new Map()

// Function to format Pokemon data from API response
const formatPokemonData = async (pokemonData) => {
  try {
    const species = await fetch(pokemonData.species.url).then(res => res.json())
    const types = pokemonData.types.map(t => t.type.name)
    const typeDefenses = await calculateTypeEffectiveness(types)
    
    // Fetch evolution chain
    const evolutionChainResponse = await fetch(species.evolution_chain.url)
    const evolutionChainData = await evolutionChainResponse.json()
    
    // Get all evolutions
    const evolutions = []
    let evoChain = evolutionChainData.chain
    
    while (evoChain) {
      // Get Pokemon data for this evolution
      const pokemonSpeciesUrl = evoChain.species.url
      const pokemonSpeciesResponse = await fetch(pokemonSpeciesUrl)
      const pokemonSpeciesData = await pokemonSpeciesResponse.json()
      
      // Get Pokemon details
      const pokemonResponse = await fetch(`${API_BASE_URL}/pokemon/${pokemonSpeciesData.id}`)
      const pokemonDetails = await pokemonResponse.json()
      
      evolutions.push({
        id: pokemonSpeciesData.id,
        name: evoChain.species.name,
        image: pokemonDetails.sprites.other['official-artwork'].front_default,
        evolveLevel: evoChain.evolution_details?.[0]?.min_level || null
      })
      
      // Move to next evolution in chain
      evoChain = evoChain.evolves_to[0]
    }
    
    return {
      id: pokemonData.id,
      name: pokemonData.name,
      image: pokemonData.sprites.other['official-artwork'].front_default,
      types: types,
      category: species.genera.find(g => g.language.name === 'en')?.genus || 'Unknown',
      description: species.flavor_text_entries.find(f => f.language.name === 'en')?.flavor_text.replace(/\f/g, ' ') || '',
      height: `${pokemonData.height / 10} m`,
      weight: `${pokemonData.weight / 10} kg`,
      abilities: pokemonData.abilities.map(a => a.ability.name),
      stats: pokemonData.stats.map(s => ({
        name: s.stat.name,
        value: s.base_stat
      })),
      moves: pokemonData.moves.slice(0, 5).map(m => ({
        name: m.move.name,
        level: m.version_group_details[0]?.level_learned_at || 1,
        type: 'normal',
        power: null,
        accuracy: null
      })),
      typeDefenses: typeDefenses,
      evolutions: evolutions // Add the evolutions array
    }
  } catch (error) {
    console.error('Error formatting Pokemon data:', error)
    throw error
  }
}

// Replace searchPokemon function
const searchPokemon = async () => {
  if (!searchQuery.value) return
  
  isLoading.value = true
  try {
    searchError.value = ''
    const response = await fetch(`${API_BASE_URL}/pokemon/${searchQuery.value.toLowerCase()}`)
    if (!response.ok) {
      throw new Error('Pokemon not found')
    }
    
    const data = await response.json()
    const formattedData = await formatPokemonData(data)
    await selectPokemon(formattedData)
    searchQuery.value = ''
    
  } catch (error) {
    console.error('Error fetching Pokemon:', error)
    searchError.value = 'Pokemon not found. Please try another name or ID.'
    searchQuery.value = ''
  } finally {
    isLoading.value = false
  }
}

// Add this new function to handle evolution chains
const getEvolutionChain = async (chain) => {
  const evolutions = []
  let currentChain = chain
  
  while (currentChain) {
    const pokemonData = await fetch(
      `${API_BASE_URL}/pokemon/${currentChain.species.name}`
    ).then(res => res.json())
    
    evolutions.push({
      id: pokemonData.id,
      name: currentChain.species.name,
      image: pokemonData.sprites.other['official-artwork'].front_default,
      evolveLevel: currentChain.evolution_details[0]?.min_level || null
    })
    
    currentChain = currentChain.evolves_to[0]
  }
  
  return evolutions
}

// Recent searches
const recentSearches = computed(() => {
  return searchHistory.value.slice(0, 3)
})

// Modify the calculateTypeEffectiveness function to use caching
const calculateTypeEffectiveness = async (types) => {
  const effectiveness = {}
  
  // Initialize all types with 1x effectiveness
  const allTypes = [
    'normal', 'fire', 'water', 'electric', 'grass', 'ice',
    'fighting', 'poison', 'ground', 'flying', 'psychic',
    'bug', 'rock', 'ghost', 'dragon', 'dark', 'steel', 'fairy'
  ]
  allTypes.forEach(type => effectiveness[type] = 1)

  // Fetch damage relations for each of the Pokemon's types
  await Promise.all(types.map(async (type) => {
    let typeData
    
    // Check cache first
    if (typeDataCache.has(type)) {
      typeData = typeDataCache.get(type)
    } else {
      const response = await fetch(`${API_BASE_URL}/type/${type}`)
      typeData = await response.json()
      typeDataCache.set(type, typeData) // Cache the result
    }
    
    // Double damage from
    typeData.damage_relations.double_damage_from.forEach(t => {
      effectiveness[t.name] *= 2
    })
    
    // Half damage from
    typeData.damage_relations.half_damage_from.forEach(t => {
      effectiveness[t.name] *= 0.5
    })
    
    // No damage from
    typeData.damage_relations.no_damage_from.forEach(t => {
      effectiveness[t.name] *= 0
    })
  }))

  // Convert to array format and sort by type name
  return allTypes.map(type => ({
    name: type,
    effectiveness: effectiveness[type] === 0 ? '0×' :
                  effectiveness[type] === 0.25 ? '¼×' :
                  effectiveness[type] === 0.5 ? '½×' :
                  effectiveness[type] === 1 ? '1×' :
                  effectiveness[type] === 2 ? '2×' :
                  effectiveness[type] === 4 ? '4×' : `${effectiveness[type]}×`
  }))
}

// Functions
const selectPokemon = async (pokemon) => {
  isLoading.value = true // Start loading
  try {
    selectedPokemon.value = pokemon
    activeTab.value = 'about'
    
    // Update adjacent pokemon info
    await updateAdjacentPokemon(pokemon.id)
    
    // Add to search history if not already there
    if (!searchHistory.value.some(p => p.id === pokemon.id)) {
      searchHistory.value.unshift(pokemon)
      if (searchHistory.value.length > 5) {
        searchHistory.value.pop()
      }
    } else {
      searchHistory.value = [
        pokemon,
        ...searchHistory.value.filter(p => p.id !== pokemon.id)
      ]
    }
  } finally {
    isLoading.value = false // End loading
  }
}

const toggleFavorite = () => {
  isFavorite.value = !isFavorite.value
}

const formatStatName = (name) => {
  switch (name) {
    case 'hp': return 'HP'
    case 'attack': return 'Attack'
    case 'defense': return 'Defense'
    case 'special-attack': return 'Sp. Atk'
    case 'special-defense': return 'Sp. Def'
    case 'speed': return 'Speed'
    default: return name
  }
}

const getTypeColor = (type) => {
  const typeColors = {
    normal: 'bg-gray-400',
    fire: 'bg-red-500',
    water: 'bg-blue-500',
    electric: 'bg-yellow-400',
    grass: 'bg-green-500',
    ice: 'bg-blue-200',
    fighting: 'bg-red-700',
    poison: 'bg-purple-500',
    ground: 'bg-yellow-700',
    flying: 'bg-indigo-300',
    psychic: 'bg-pink-500',
    bug: 'bg-lime-500',
    rock: 'bg-yellow-600',
    ghost: 'bg-purple-700',
    dragon: 'bg-indigo-700',
    dark: 'bg-gray-700',
    steel: 'bg-gray-400',
    fairy: 'bg-pink-300'
  }
  
  return typeColors[type] || 'bg-gray-500'
}

const getStatColor = (name, value) => {
  if (value >= 70) return 'bg-green-400'
  if (value >= 50) return 'bg-yellow-400'
  return 'bg-white'
}

// Add this helper function
const getEffectivenessColor = (effectiveness) => {
  switch (effectiveness) {
    case '0×':
      return 'bg-gray-500'
    case '¼×':
    case '½×':
      return 'bg-green-500'
    case '1×':
      return 'bg-gray-400'
    case '2×':
      return 'bg-red-500'
    case '4×':
      return 'bg-red-700'
    default:
      return 'bg-gray-400'
  }
}

// Add this computed property in the script section
const organizedTypeDefenses = computed(() => {
  if (!selectedPokemon.value?.typeDefenses) return { weaknesses: [], normal: [], resistances: [] }
  
  return {
    weaknesses: selectedPokemon.value.typeDefenses.filter(t => 
      t.effectiveness === '2×' || t.effectiveness === '4×'
    ),
    normal: selectedPokemon.value.typeDefenses.filter(t => 
      t.effectiveness === '1×'
    ),
    resistances: selectedPokemon.value.typeDefenses.filter(t => 
      t.effectiveness === '½×' || t.effectiveness === '¼×' || t.effectiveness === '0×'
    )
  }
})

// Add these refs and computed properties
const previousPokemon = ref(null)
const nextPokemon = ref(null)

// Add this function to fetch a pokemon by ID
const fetchPokemonById = async (id) => {
  try {
    const response = await fetch(`${API_BASE_URL}/pokemon/${id}`)
    if (!response.ok) return null
    const data = await response.json()
    return {
      id: data.id,
      name: data.name,
      image: data.sprites.other['official-artwork'].front_default
    }
  } catch (error) {
    console.error('Error fetching pokemon:', error)
    return null
  }
}

// Add this function to update adjacent pokemon info
const updateAdjacentPokemon = async (currentId) => {
  // Fetch previous pokemon if not #1
  if (currentId > 1) {
    previousPokemon.value = await fetchPokemonById(currentId - 1)
  } else {
    previousPokemon.value = null
  }
  
  // Fetch next pokemon
  nextPokemon.value = await fetchPokemonById(currentId + 1)
}

// Add navigation function
const navigatePokemon = async (direction) => {
  if (!selectedPokemon.value) return
  
  const nextId = selectedPokemon.value.id + direction
  if (nextId < 1) return
  
  isLoading.value = true
  try {
    const response = await fetch(`${API_BASE_URL}/pokemon/${nextId}`)
    if (!response.ok) return
    
    const data = await response.json()
    const formattedData = await formatPokemonData(data)
    await selectPokemon(formattedData)
  } catch (error) {
    console.error('Error navigating pokemon:', error)
  } finally {
    isLoading.value = false
  }
}

// Add this new function to handle evolution clicks
const loadEvolution = async (pokemonId) => {
  isLoading.value = true
  try {
    const response = await fetch(`${API_BASE_URL}/pokemon/${pokemonId}`)
    if (!response.ok) throw new Error('Pokemon not found')
    
    const data = await response.json()
    const formattedData = await formatPokemonData(data)
    await selectPokemon(formattedData)
  } catch (error) {
    console.error('Error loading evolution:', error)
  } finally {
    isLoading.value = false
  }
}

// Add this function to get a random Pokemon ID
const getRandomPokemonId = () => {
  return Math.floor(Math.random() * 1000) + 1 // Random number between 1 and 1000
}

// Update the onMounted hook
onMounted(async () => {
  try {
    const randomId = getRandomPokemonId()
    const response = await fetch(`${API_BASE_URL}/pokemon/${randomId}`)
    const data = await response.json()
    const formattedData = await formatPokemonData(data)
    await selectPokemon(formattedData)
  } catch (error) {
    console.error('Error fetching initial Pokemon:', error)
    // If random Pokemon fails, fallback to Bulbasaur
    try {
      const response = await fetch(`${API_BASE_URL}/pokemon/1`)
      const data = await response.json()
      const formattedData = await formatPokemonData(data)
      await selectPokemon(formattedData)
    } catch (fallbackError) {
      console.error('Error fetching fallback Pokemon:', fallbackError)
    }
  }
})
</script>

<style>
/* Hide scrollbar for Chrome, Safari and Opera */
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.scrollbar-hide {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

.animate-fade-in {
  animation: fadeIn 0.3s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.animate-spin {
  animation: spin 1s linear infinite;
}
</style>