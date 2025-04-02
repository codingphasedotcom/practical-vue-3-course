<template>
    <div class="min-h-screen bg-gradient-to-br from-green-900 to-green-700 p-3 sm:p-6 md:p-10 flex items-center justify-center">
      <div class="w-full max-w-[1400px] backdrop-blur-xl bg-black/40 rounded-3xl shadow-2xl border border-white/10 overflow-hidden">
        <!-- Mobile Header - Only visible on small screens -->
        <div class="lg:hidden p-4 border-b border-white/10">
          <div class="flex justify-between items-center">
            <button class="text-white/60 hover:text-white transition-colors cursor-pointer p-2">
              <ArrowLeft class="w-5 h-5" />
            </button>
            <h1 class="text-xl font-bold text-white">Pokédex</h1>
            <button class="text-white/60 hover:text-white transition-colors cursor-pointer p-2">
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
                  type="text" 
                  placeholder="Search Pokémon" 
                  class="bg-transparent text-white outline-none w-full placeholder-white/60 text-sm sm:text-base"
                />
                <button class="bg-white/20 rounded-full p-1 hover:bg-white/30 transition-colors cursor-pointer">
                  <Search class="w-3 h-3 sm:w-4 sm:h-4 text-white" />
                </button>
              </div>
  
              <!-- Recent Searches section -->
              <div class="hidden sm:block">
                <h2 class="text-lg sm:text-xl font-bold text-white mb-3 sm:mb-4">Recent</h2>
                <div class="space-y-2 sm:space-y-3">
                  <!-- Example recent search item -->
                  <div class="flex items-center gap-2 sm:gap-3 p-2 sm:p-3 rounded-xl hover:bg-white/10 cursor-pointer transition-colors">
                    <img 
                      src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/1.png" 
                      alt="Bulbasaur" 
                      class="w-10 h-10 sm:w-12 sm:h-12 object-contain"
                    />
                    <div>
                      <div class="text-white font-medium capitalize text-sm sm:text-base">Bulbasaur</div>
                      <div class="text-white/60 text-xs sm:text-sm">#001</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
  
          <!-- Main Content Area -->
          <div class="lg:col-span-9 overflow-y-auto max-h-[calc(100vh-80px)] lg:max-h-none">
            <div class="grid grid-cols-1 lg:grid-cols-2 h-full">
              <!-- Pokemon Image Section -->
              <div class="p-4 sm:p-6 md:p-8 flex flex-col">
                <!-- Desktop Header - Only visible on large screens -->
                <div class="hidden lg:flex justify-between items-center mb-4">
                  <button class="flex items-center gap-2 text-white/60 hover:text-white transition-colors px-4 py-2 rounded-lg hover:bg-white/10 cursor-pointer">
                    <ArrowLeft class="w-5 h-5" />
                    <span class="text-sm capitalize">Venusaur</span>
                  </button>
  
                  <button class="flex items-center gap-2 text-white/60 hover:text-white transition-colors px-4 py-2 rounded-lg hover:bg-white/10 cursor-pointer">
                    <span class="text-sm capitalize">Charmander</span>
                    <ArrowRight class="w-5 h-5" />
                  </button>
                </div>
  
                <!-- Pokemon Image -->
                <div class="flex-1 relative flex items-center justify-center py-6 sm:py-8">
                  <div class="absolute inset-0 bg-gradient-to-b from-green-500/30 to-green-700/30 rounded-full blur-3xl"></div>
                  <img 
                    src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/1.png" 
                    alt="Bulbasaur" 
                    class="w-48 h-48 sm:w-64 sm:h-64 md:w-80 md:h-80 object-contain z-10"
                  />
                </div>
  
                <!-- Pokemon Name and Type -->
                <div class="text-center mt-2 sm:mt-4">
                  <h1 class="text-2xl sm:text-3xl md:text-4xl font-bold text-white mb-1 capitalize">Bulbasaur</h1>
                  <p class="text-white/70 mb-3 sm:mb-4">Seed Pokémon</p>
  
                  <!-- Type Tags -->
                  <div class="flex justify-center gap-2 sm:gap-3 mb-4 sm:mb-6 flex-wrap">
                    <div class="rounded-full px-3 sm:px-5 py-1 sm:py-1.5 flex items-center bg-green-500">
                      <div class="w-3 h-3 sm:w-4 sm:h-4 mr-1 sm:mr-2 bg-white/20 rounded-full"></div>
                      <span class="text-white capitalize text-sm sm:text-base">Grass</span>
                    </div>
                    <div class="rounded-full px-3 sm:px-5 py-1 sm:py-1.5 flex items-center bg-purple-500">
                      <div class="w-3 h-3 sm:w-4 sm:h-4 mr-1 sm:mr-2 bg-white/20 rounded-full"></div>
                      <span class="text-white capitalize text-sm sm:text-base">Poison</span>
                    </div>
                  </div>
                </div>
              </div>
  
              <!-- Pokemon Details Section -->
              <div class="border-t lg:border-t-0 lg:border-l border-white/10">
                <!-- Tabs -->
                <div class="flex border-b border-white/10 overflow-x-auto scrollbar-hide">
                  <button class="py-3 sm:py-4 px-3 sm:px-6 font-medium transition-colors whitespace-nowrap cursor-pointer hover:bg-white/5 text-white border-b-2 border-white">
                    About
                  </button>
                  <button class="py-3 sm:py-4 px-3 sm:px-6 font-medium transition-colors whitespace-nowrap cursor-pointer hover:bg-white/5 text-white/60">
                    Stats
                  </button>
                  <button class="py-3 sm:py-4 px-3 sm:px-6 font-medium transition-colors whitespace-nowrap cursor-pointer hover:bg-white/5 text-white/60">
                    Moves
                  </button>
                  <button class="py-3 sm:py-4 px-3 sm:px-6 font-medium transition-colors whitespace-nowrap cursor-pointer hover:bg-white/5 text-white/60">
                    Evolutions
                  </button>
                </div>
  
                <!-- Tab Content -->
                <div class="p-4 sm:p-6 md:p-8">
                  <!-- About Tab -->
                  <div class="text-white/80">
                    <p class="mb-4 sm:mb-6 text-base sm:text-lg">
                      A strange seed was planted on its back at birth. The plant sprouts and grows with this Pokémon.
                    </p>
                    
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 sm:gap-6 mb-6 sm:mb-8">
                      <div>
                        <h3 class="text-white/60 mb-2">Height</h3>
                        <div class="flex items-center">
                          <Ruler class="w-4 h-4 sm:w-5 sm:h-5 mr-2 text-white/60" />
                          <span class="text-white text-base sm:text-lg">0.7 m</span>
                        </div>
                      </div>
                      <div>
                        <h3 class="text-white/60 mb-2">Weight</h3>
                        <div class="flex items-center">
                          <Weight class="w-4 h-4 sm:w-5 sm:h-5 mr-2 text-white/60" />
                          <span class="text-white text-base sm:text-lg">6.9 kg</span>
                        </div>
                      </div>
                    </div>
  
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 sm:gap-6">
                      <div>
                        <h3 class="text-white/60 mb-2">Abilities</h3>
                        <ul class="space-y-1 sm:space-y-2">
                          <li class="text-white text-sm sm:text-base">Overgrow</li>
                          <li class="text-white text-sm sm:text-base">Chlorophyll</li>
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
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
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