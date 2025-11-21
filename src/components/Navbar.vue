<template>
  <div>

    <nav :class="[
      'fixed top-0 left-0 w-full backdrop-blur-lg px-6 py-4 flex justify-center z-50 transition-all duration-500',
      {
        'bg-white/80 dark:bg-gray-800/80 border-b border-gray-200/50 dark:border-gray-800/50': !isScrolled,
        'bg-white/95 dark:bg-gray-800/95 shadow-xl shadow-gray-900/5 dark:shadow-gray-900/20 border-b border-gray-200/70 dark:border-gray-800/70': isScrolled
      }
    ]">
      <div class="w-full max-w-[1400px] flex items-center justify-between">
        
        <!-- Logo Section -->
        <div class="flex items-center">
          <a href="/" class="group flex items-center space-x-2" @click="scrollToTop">
            <div class="relative">
              <span class="text-2xl font-bold bg-gradient-to-r from-blue-600 via-purple-600 to-blue-800 dark:from-blue-400 dark:via-purple-400 dark:to-blue-600 bg-clip-text text-transparent group-hover:scale-110 transition-all duration-300 cursor-pointer">
                Soller
              </span>
              <div class="absolute -bottom-1 left-0 w-0 h-0.5 bg-gradient-to-r from-blue-600 to-purple-600 group-hover:w-full transition-all duration-500"></div>
            </div>
          </a>
        </div>
        
        <!-- Desktop Navigation -->
        <div class="hidden md:flex items-center justify-center">
          <div class="flex items-center gap-8">
            <a 
              v-for="(item, index) in navItems" 
              :key="item.name"
              :href="item.href" 
              class="relative text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-all duration-300 text-sm font-medium py-2 group"
              :style="{ animationDelay: `${index * 100}ms` }"
              @click="handleNavClick(item.href, $event)"
            >
              {{ item.name }}
              <span class="absolute bottom-0 left-0 w-full h-0.5 bg-gradient-to-r from-blue-600 to-purple-600 dark:from-blue-400 dark:to-purple-400 transform scale-x-0 group-hover:scale-x-100 transition-transform duration-300"></span>
              <!-- Active indicator -->
              <span v-if="activeSection === item.href.substring(1)" class="absolute -top-1 -right-1 w-2 h-2 bg-blue-600 dark:bg-blue-400 rounded-full animate-pulse"></span>
            </a>
          </div>
        </div>
        
        <!-- Desktop Actions -->
        <div class="hidden md:flex items-center gap-4">
          <button 
            @click="toggleDark"
            class="relative p-2 rounded-xl bg-gray-100 dark:bg-gray-800/50 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 transition-all duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-blue-500/50 overflow-hidden"
            :title="isDark ? 'Switch to light mode' : 'Switch to dark mode'"
          >
            <div class="relative w-5 h-5">
              <svg 
                v-show="!isDark" 
                class="absolute inset-0 w-5 h-5 transition-all duration-500 rotate-0 scale-100" 
                fill="currentColor" 
                viewBox="0 0 20 20"
              >
                <path d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"></path>
              </svg>
              <svg 
                v-show="isDark" 
                class="absolute inset-0 w-5 h-5 transition-all duration-500 rotate-0 scale-100" 
                fill="currentColor" 
                viewBox="0 0 20 20"
              >
                <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path>
              </svg>
            </div>
          </button>

          <!-- Login Button -->
          <button class="relative px-6 py-2.5 text-sm font-medium text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-all duration-300 rounded-xl border border-transparent hover:border-gray-200 dark:hover:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-800/50 focus:outline-none focus:ring-2 focus:ring-blue-500/50">
            Log in
          </button>

          <!-- Join Now Button -->
          <button class="relative px-6 py-2.5 bg-gradient-to-r from-blue-600 via-purple-600 to-blue-700 dark:from-blue-500 dark:via-purple-500 dark:to-blue-600 text-white text-sm font-medium rounded-xl hover:from-blue-700 hover:via-purple-700 hover:to-blue-800 transition-all duration-300 hover:shadow-xl hover:shadow-purple-500/30 hover:-translate-y-0.5 focus:outline-none focus:ring-2 focus:ring-purple-500/50 overflow-hidden group">
            <span class="relative z-10 flex items-center gap-2">
              Join Now
              <svg class="w-4 h-4 group-hover:translate-x-1 transition-transform duration-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"></path>
              </svg>
            </span>
            <div class="absolute inset-0 bg-white/20 transform scale-x-0 group-hover:scale-x-100 transition-transform duration-500 origin-left"></div>
          </button>
        </div>
        
        <!-- Mobile Actions -->
        <div class="md:hidden flex items-center gap-2">
          <!-- Dark Mode Toggle Mobile -->
          <button 
            @click="toggleDark"
            class="p-2 rounded-xl bg-gray-100 dark:bg-gray-800/50 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 transition-all duration-300"
          >
            <div class="w-5 h-5">
              <svg v-if="isDark" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                <path d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"></path>
              </svg>
              <svg v-else class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path>
              </svg>
            </div>
          </button>

          <!-- Mobile Menu Button -->
          <button 
            class="p-2 text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-all duration-300 hover:scale-105"
            @click="toggleMenu"
          >
            <div class="w-6 h-6 relative">
              <span 
                class="absolute block w-6 h-0.5 bg-current transform transition-all duration-300"
                :class="menuOpen ? 'rotate-45 top-2.5' : 'top-1.5'"
              ></span>
              <span 
                class="absolute block w-6 h-0.5 bg-current top-2.5 transition-all duration-300"
                :class="menuOpen ? 'opacity-0' : 'opacity-100'"
              ></span>
              <span 
                class="absolute block w-6 h-0.5 bg-current transform transition-all duration-300"
                :class="menuOpen ? '-rotate-45 top-2.5' : 'top-3.5'"
              ></span>
            </div>
          </button>
        </div>
      </div>
    </nav>

    <!-- Search Overlay -->
    <Transition name="fade">
      <div v-show="searchOpen" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-60" @click="closeSearch">
        <div class="flex items-start justify-center pt-20">
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-6 w-full max-w-2xl mx-4 shadow-2xl" @click.stop>
            <div class="flex items-center gap-4 mb-4">
              <svg class="w-6 h-6 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
              </svg>
              <input 
                ref="searchInput"
                type="text" 
                placeholder="Search anything..." 
                class="flex-1 bg-transparent text-gray-900 dark:text-white placeholder-gray-500 dark:placeholder-gray-400 text-lg focus:outline-none"
                v-model="searchQuery"
              >
              <button @click="closeSearch" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition-colors">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                </svg>
              </button>
            </div>
            <div class="text-sm text-gray-500 dark:text-gray-400">
              Press <kbd class="px-2 py-1 bg-gray-100 dark:bg-gray-800 rounded">Esc</kbd> to close
            </div>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Mobile Menu -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="transform -translate-y-2 opacity-0 scale-95"
      enter-to-class="transform translate-y-0 opacity-100 scale-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="transform translate-y-0 opacity-100 scale-100"
      leave-to-class="transform -translate-y-2 opacity-0 scale-95"
    >
      <div 
        v-show="menuOpen"
        class="fixed top-[88px] left-0 w-full bg-white/95 dark:bg-gray-900/95 backdrop-blur-lg border-t border-gray-200/70 dark:border-gray-800/70 md:hidden z-40 shadow-2xl"
      >
        <div class="px-6 py-6 space-y-4">
          <!-- Mobile Navigation Links -->
          <div class="space-y-2">
            <a 
              v-for="(item, index) in navItems" 
              :key="item.name"
              :href="item.href"
              class="flex items-center gap-3 text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-all duration-300 text-sm font-medium py-3 px-4 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-800/50 group"
              :style="{ animationDelay: `${index * 50}ms` }"
              @click="handleNavClick(item.href, $event)"
            >
              <div class="w-2 h-2 rounded-full bg-blue-600 dark:bg-blue-400 opacity-0 group-hover:opacity-100 transition-all duration-300 transform group-hover:scale-125"></div>
              <span>{{ item.name }}</span>
              <div class="ml-auto opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
              </div>
            </a>
          </div>

          <!-- Mobile Search -->
          <div class="pt-4 border-t border-gray-200 dark:border-gray-800">
            <button 
              @click="toggleSearch" 
              class="flex items-center gap-3 w-full text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-all duration-300 text-sm font-medium py-3 px-4 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-800/50"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
              </svg>
              Search
            </button>
          </div>

          <!-- Mobile Action Buttons -->
          <div class="pt-4 border-t border-gray-200 dark:border-gray-800 space-y-3">
            <button class="w-full px-4 py-3 text-sm font-medium text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-all duration-300 border border-gray-200 dark:border-gray-700 rounded-xl hover:border-blue-600 dark:hover:border-blue-400 hover:bg-gray-50 dark:hover:bg-gray-800/50">
              Log in
            </button>
            <button class="w-full px-4 py-3 bg-gradient-to-r from-blue-600 via-purple-600 to-blue-700 dark:from-blue-500 dark:via-purple-500 dark:to-blue-600 text-white text-sm font-medium rounded-xl hover:from-blue-700 hover:via-purple-700 hover:to-blue-800 transition-all duration-300 hover:shadow-lg hover:shadow-purple-500/25">
              Join Now
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Background overlay for mobile menu -->
    <Transition name="fade">
      <div 
        v-show="menuOpen"
        class="fixed inset-0 bg-black/20 dark:bg-black/40 backdrop-blur-sm md:hidden z-30 mt-[88px]"
        @click="closeMenu"
      ></div>
    </Transition>

    <!-- Spacer for fixed navbar -->
    <div class="h-[88px]"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Reactive states
const isScrolled = ref(false)
const menuOpen = ref(false)
const searchOpen = ref(false)
const searchQuery = ref('')
const searchInput = ref(null)
const activeSection = ref('')

// Check for saved dark mode preference
const isDark = ref(false)

// Navigation items
const navItems = [
  { name: 'About Us', href: '#about' },
  { name: 'Solutions', href: '#solutions' },
  { name: 'Pricing', href: '#pricing' },
  { name: 'Resources', href: '#resources' }
]

// Functions
function toggleDark() {
  isDark.value = !isDark.value
  localStorage.setItem('darkMode', isDark.value.toString())
  document.documentElement.classList.toggle('dark', isDark.value)
}

function toggleMenu() {
  menuOpen.value = !menuOpen.value
  document.body.style.overflow = menuOpen.value ? 'hidden' : ''
}

function closeMenu() {
  menuOpen.value = false
  document.body.style.overflow = ''
}

function toggleSearch() {
  searchOpen.value = !searchOpen.value
  if (searchOpen.value) {
    nextTick(() => {
      searchInput.value?.focus()
    })
  }
  closeMenu() // Close mobile menu if open
}

function closeSearch() {
  searchOpen.value = false
  searchQuery.value = ''
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

function handleNavClick(href, event) {
  if (href.startsWith('#')) {
    event.preventDefault()
    const element = document.querySelector(href)
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' })
      closeMenu()
    }
  }
}

function handleScroll() {
  isScrolled.value = window.scrollY > 20
  
  // Update active section
  const sections = navItems.map(item => item.href.substring(1))
  const scrollPosition = window.scrollY + 100
  
  for (const section of sections) {
    const element = document.getElementById(section)
    if (element) {
      const top = element.offsetTop
      const height = element.offsetHeight
      
      if (scrollPosition >= top && scrollPosition <= top + height) {
        activeSection.value = section
        break
      }
    }
  }
}

function handleKeydown(event) {
  if (event.key === 'Escape') {
    if (searchOpen.value) {
      closeSearch()
    } else if (menuOpen.value) {
      closeMenu()
    }
  }
  
  // CMD/Ctrl + K for search
  if ((event.metaKey || event.ctrlKey) && event.key === 'k') {
    event.preventDefault()
    toggleSearch()
  }
}

// Lifecycle hooks
onMounted(() => {
  // Initialize dark mode
  const savedDarkMode = localStorage.getItem('darkMode')
  if (savedDarkMode !== null) {
    isDark.value = savedDarkMode === 'true'
  } else {
    // Check system preference
    isDark.value = window.matchMedia('(prefers-color-scheme: dark)').matches
  }
  
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  }
  
  // Add event listeners
  window.addEventListener('scroll', handleScroll)
  document.addEventListener('keydown', handleKeydown)
  
  // Initial scroll check
  handleScroll()
})

onUnmounted(() => {
  // Cleanup
  window.removeEventListener('scroll', handleScroll)
  document.removeEventListener('keydown', handleKeydown)
  document.body.style.overflow = ''
})
</script>

<style scoped>
/* Transitions */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

/* Keyboard shortcut styling */
kbd {
  font-family: ui-monospace, SFMono-Regular, "SF Mono", Menlo, Monaco, Consolas, monospace;
  font-size: 0.75rem;
  font-weight: 500;
}

/* Smooth scrolling */
html {
  scroll-behavior: smooth;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: rgba(156, 163, 175, 0.3);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(156, 163, 175, 0.5);
}

/* Dark mode scrollbar */
.dark ::-webkit-scrollbar-thumb {
  background: rgba(75, 85, 99, 0.5);
}

.dark ::-webkit-scrollbar-thumb:hover {
  background: rgba(75, 85, 99, 0.7);
}
</style>