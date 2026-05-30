<template>
  <div class="min-h-screen bg-gray-50 flex">
    <!-- Sidebar Desktop - Selalu tampil di lg: keatas -->
    <aside class="hidden lg:block fixed lg:sticky top-0 left-0 z-40 w-64 bg-white shadow-lg h-screen overflow-y-auto">
      <div class="flex flex-col h-full">
        <div class="p-5 border-b border-gray-100">
          <div class="flex items-center gap-2">
            <div class="w-8 h-8 rounded-lg bg-gray-800 flex items-center justify-center">
              <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/>
              </svg>
            </div>
            <h2 class="text-lg font-semibold text-gray-800">LynxxSpace</h2>
          </div>
        </div>

        <div class="p-4 m-3 rounded-lg bg-gray-50">
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 rounded-full bg-gray-300 flex items-center justify-center text-gray-700 font-medium">
              {{ userInitial }}
            </div>
            <div class="flex-1 min-w-0">
              <p class="font-medium text-gray-800 truncate text-sm">{{ user.name }}</p>
              <p class="text-xs text-gray-500 truncate">{{ user.email }}</p>
            </div>
          </div>
        </div>

        <nav class="flex-1 px-3 space-y-1">
          <button
            v-for="item in menuItems"
            :key="item.id"
            @click="activeTab = item.id"
            :class="[
              'w-full flex items-center gap-3 px-3 py-2.5 rounded-lg transition-all duration-200 text-sm',
              activeTab === item.id ? 'bg-gray-100 text-gray-900' : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900'
            ]"
          >
            <span v-html="item.icon" class="w-4 h-4"></span>
            <span>{{ item.label }}</span>
          </button>
        </nav>

        <div class="p-4 border-t border-gray-100">
          <button @click="handleLogout" class="w-full flex items-center justify-center gap-2 px-3 py-2 rounded-lg text-gray-600 hover:bg-gray-50 hover:text-gray-900 transition-all duration-200 text-sm">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"/>
            </svg>
            <span>Keluar</span>
          </button>
        </div>
      </div>
    </aside>

    <!-- Mobile Navbar & Sidebar Drawer -->
    <div class="lg:hidden fixed top-0 left-0 right-0 z-50 bg-white shadow-md px-4 py-3 flex justify-between items-center">
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 rounded-lg bg-gray-800 flex items-center justify-center">
          <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/>
          </svg>
        </div>
        <h2 class="text-lg font-semibold text-gray-800">LynxxSpace</h2>
      </div>
      <button @click="mobileMenuOpen = !mobileMenuOpen" class="p-2 rounded-lg hover:bg-gray-100">
        <svg v-if="!mobileMenuOpen" class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
        </svg>
        <svg v-else class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
        </svg>
      </button>
    </div>

    <!-- Mobile Sidebar Drawer -->
    <div v-if="mobileMenuOpen" class="fixed inset-0 z-40 bg-black/50 lg:hidden" @click="mobileMenuOpen = false"></div>
    
    <aside :class="[
      'fixed top-0 left-0 z-50 w-64 bg-white shadow-lg h-screen overflow-y-auto transform transition-transform duration-300 ease-in-out lg:hidden',
      mobileMenuOpen ? 'translate-x-0' : '-translate-x-full'
    ]">
      <div class="flex flex-col h-full">
        <div class="p-5 border-b border-gray-100 flex justify-between items-center">
          <div class="flex items-center gap-2">
            <div class="w-8 h-8 rounded-lg bg-gray-800 flex items-center justify-center">
              <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/>
              </svg>
            </div>
            <h2 class="text-lg font-semibold text-gray-800">LynxxSpace</h2>
          </div>
          <button @click="mobileMenuOpen = false" class="text-gray-500 hover:text-gray-700">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
          </button>
        </div>

        <div class="p-4 m-3 rounded-lg bg-gray-50">
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 rounded-full bg-gray-300 flex items-center justify-center text-gray-700 font-medium">
              {{ userInitial }}
            </div>
            <div class="flex-1 min-w-0">
              <p class="font-medium text-gray-800 truncate text-sm">{{ user.name }}</p>
              <p class="text-xs text-gray-500 truncate">{{ user.email }}</p>
            </div>
          </div>
        </div>

        <nav class="flex-1 px-3 space-y-1">
          <button
            v-for="item in menuItems"
            :key="item.id"
            @click="activeTab = item.id; mobileMenuOpen = false"
            :class="[
              'w-full flex items-center gap-3 px-3 py-2.5 rounded-lg transition-all duration-200 text-sm',
              activeTab === item.id ? 'bg-gray-100 text-gray-900' : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900'
            ]"
          >
            <span v-html="item.icon" class="w-4 h-4"></span>
            <span>{{ item.label }}</span>
          </button>
        </nav>

        <div class="p-4 border-t border-gray-100">
          <button @click="handleLogout" class="w-full flex items-center justify-center gap-2 px-3 py-2 rounded-lg text-gray-600 hover:bg-gray-50 hover:text-gray-900 transition-all duration-200 text-sm">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"/>
            </svg>
            <span>Keluar</span>
          </button>
        </div>
      </div>
    </aside>

    <main class="flex-1 min-w-0 bg-gray-50">
      <!-- Top Bar Desktop -->
      <div class="bg-white border-b border-gray-100 sticky top-0 z-30 hidden lg:block">
        <div class="px-4 sm:px-6 lg:px-8 py-4">
          <div class="flex justify-between items-center">
            <div>
              <p class="text-xs text-gray-500">{{ currentDateString }}</p>
              <h1 class="text-xl font-semibold text-gray-800">{{ greetingText }}</h1>
            </div>
            <div class="flex items-center gap-2">
              <div class="flex items-center gap-1.5">
                <span class="w-1.5 h-1.5 bg-green-500 rounded-full"></span>
                <span class="text-xs text-gray-500">Online</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Top Bar Mobile -->
      <div class="bg-white border-b border-gray-100 sticky top-0 z-30 lg:hidden mt-14">
        <div class="px-4 py-3">
          <p class="text-xs text-gray-500">{{ currentDateString }}</p>
          <h1 class="text-base font-semibold text-gray-800">{{ greetingText }}</h1>
        </div>
      </div>

      <div class="px-4 sm:px-6 lg:px-8 py-6 pb-12">
        <!-- Toast -->
        <div v-if="toast.message" class="fixed top-20 right-4 z-50 animate-slide-in">
          <div :class="['flex items-center gap-2 px-3 py-2 rounded-lg shadow-lg text-sm', toast.type === 'success' ? 'bg-gray-800 text-white' : 'bg-red-500 text-white']">
            <svg v-if="toast.type === 'success'" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
            </svg>
            <svg v-else class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
            <p>{{ toast.message }}</p>
          </div>
        </div>

        <!-- Workspace Tab (sama seperti sebelumnya) -->
        <div v-if="activeTab === 'workspace'" class="space-y-5">
          <div class="bg-white rounded-lg border border-gray-200 p-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-1">Selamat Datang, {{ user.name }}!</h2>
            <p class="text-sm text-gray-500 mb-4">Temukan fokus terbaik Anda hari ini.</p>
            <div class="bg-gray-50 rounded-md p-3 border border-gray-100">
              <p class="text-xs text-gray-600 italic">"Kunci utama produktivitas adalah mengelola fokus, bukan waktu."</p>
            </div>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div class="bg-white rounded-lg border border-gray-200 p-4">
              <div class="flex items-center justify-between mb-2">
                <div class="w-10 h-10 rounded-lg bg-gray-100 flex items-center justify-center">
                  <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/>
                  </svg>
                </div>
                <span class="text-2xl font-semibold text-gray-800">{{ tasks.length }}</span>
              </div>
              <h3 class="font-medium text-gray-800 text-sm">Total Tugas</h3>
            </div>

            <div class="bg-white rounded-lg border border-gray-200 p-4">
              <div class="flex items-center justify-between mb-2">
                <div class="w-10 h-10 rounded-lg bg-gray-100 flex items-center justify-center">
                  <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>
                  </svg>
                </div>
                <span class="text-2xl font-semibold text-gray-800">{{ completedTasksCount }}</span>
              </div>
              <h3 class="font-medium text-gray-800 text-sm">Tugas Selesai</h3>
            </div>

            <div class="bg-white rounded-lg border border-gray-200 p-4">
              <div class="flex items-center justify-between mb-2">
                <div class="w-10 h-10 rounded-lg bg-gray-100 flex items-center justify-center">
                  <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/>
                  </svg>
                </div>
                <span class="text-2xl font-semibold text-gray-800">{{ notes.length }}</span>
              </div>
              <h3 class="font-medium text-gray-800 text-sm">Catatan Aktif</h3>
            </div>
          </div>

          <!-- Ambient Sound -->
          <div class="bg-white rounded-lg border border-gray-200 p-5">
            <div class="mb-4">
              <div class="flex items-center gap-2">
                <svg class="w-4 h-4 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15.536a5 5 0 010-7.072m-2.828 9.9a9 9 0 010-12.728"/>
                </svg>
                <h3 class="font-medium text-gray-800 text-sm">Lynxx Ambient Sound</h3>
              </div>
              <p class="text-xs text-gray-500 mt-1">Pilih musik latar yang nyaman untuk fokus</p>
            </div>

            <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 mb-5">
              <button v-for="sound in soundOptions" :key="sound.id" @click="selectSound(sound.id)" :class="[
                'px-3 py-3 rounded-xl text-center transition-all duration-200',
                currentSound === sound.id && isNoisePlaying ? 'bg-gray-800 text-white shadow-md scale-105' :
                currentSound === sound.id && !isNoisePlaying ? 'bg-gray-200 text-gray-800 border border-gray-300' :
                'bg-gray-50 text-gray-600 hover:bg-gray-100 border border-gray-200'
              ]">
                <div class="flex flex-col items-center gap-1">
                  <span v-html="sound.icon" class="text-xl"></span>
                  <span class="text-xs font-medium">{{ sound.name }}</span>
                </div>
              </button>
            </div>

            <div class="bg-gray-50 rounded-xl p-4">
              <div class="flex flex-col sm:flex-row items-center gap-4">
                <div class="flex gap-1 items-end h-12">
                  <div :class="['w-1.5 bg-gray-500 rounded-full transition-all duration-150', isNoisePlaying ? 'animate-bounce' : 'h-3', isNoisePlaying ? 'h-6' : '']"></div>
                  <div :class="['w-1.5 bg-gray-600 rounded-full transition-all duration-150', isNoisePlaying ? 'animate-bounce animation-delay-200' : 'h-4', isNoisePlaying ? 'h-10' : '']"></div>
                  <div :class="['w-1.5 bg-gray-700 rounded-full transition-all duration-150', isNoisePlaying ? 'animate-bounce animation-delay-400' : 'h-5', isNoisePlaying ? 'h-12' : '']"></div>
                  <div :class="['w-1.5 bg-gray-600 rounded-full transition-all duration-150', isNoisePlaying ? 'animate-bounce animation-delay-100' : 'h-4', isNoisePlaying ? 'h-9' : '']"></div>
                  <div :class="['w-1.5 bg-gray-500 rounded-full transition-all duration-150', isNoisePlaying ? 'animate-bounce animation-delay-300' : 'h-3', isNoisePlaying ? 'h-7' : '']"></div>
                </div>

                <button @click="toggleAmbientNoise" :class="[
                  'px-6 py-2.5 rounded-lg text-sm font-medium transition-all flex items-center gap-2',
                  isNoisePlaying ? 'bg-gray-200 text-gray-700 hover:bg-gray-300' : 'bg-gray-800 text-white hover:bg-gray-700'
                ]">
                  <svg v-if="isNoisePlaying" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <rect x="6" y="4" width="4" height="16" rx="1" />
                    <rect x="14" y="4" width="4" height="16" rx="1" />
                  </svg>
                  <svg v-else class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <polygon points="5 3 19 12 5 21 5 3" />
                  </svg>
                  <span>{{ isNoisePlaying ? "Hentikan" : "Putar " + getCurrentSoundName() }}</span>
                </button>

                <div v-if="isNoisePlaying" class="flex items-center gap-3 flex-1">
                  <svg class="w-4 h-4 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15.536a5 5 0 010-7.072m-2.828 9.9a9 9 0 010-12.728"/>
                  </svg>
                  <input type="range" min="0" max="0.5" step="0.01" v-model="noiseVolume" @input="updateNoiseVolume" class="flex-1 h-1.5 rounded-full appearance-none bg-gray-300" />
                  <span class="text-xs text-gray-500 w-10">{{ Math.round(noiseVolume * 100) }}%</span>
                </div>
              </div>

              <div v-if="isNoisePlaying" class="mt-3 text-center border-t border-gray-200 pt-3">
                <p class="text-xs text-gray-500">
                  <svg class="w-3 h-3 inline mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3"/>
                  </svg>
                  Sedang memutar: <span class="font-medium text-gray-700">{{ getSoundDescription() }}</span>
                </p>
              </div>
              <div v-else class="mt-3 text-center border-t border-gray-200 pt-3">
                <p class="text-xs text-gray-400">
                  <svg class="w-3 h-3 inline mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3"/>
                  </svg>
                  Pilih musik di atas untuk menemani aktivitas Anda
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Tasks Tab -->
        <div v-if="activeTab === 'tasks'" class="space-y-5">
          <div class="flex justify-between items-center">
            <div>
              <h2 class="text-lg font-semibold text-gray-800">Manajemen Tugas</h2>
              <p class="text-xs text-gray-500">Kelola prioritas harian Anda</p>
            </div>
            <button @click="showTaskModal = true" class="px-3 py-1.5 bg-gray-800 text-white rounded-md text-sm hover:bg-gray-700 transition flex items-center gap-1">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/>
              </svg>
              Tambah Tugas
            </button>
          </div>

          <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
            <!-- Todo Column -->
            <div class="bg-gray-50 rounded-lg p-3">
              <div class="flex items-center justify-between mb-3">
                <div class="flex items-center gap-2">
                  <div class="w-2 h-2 bg-gray-400 rounded-full"></div>
                  <h3 class="font-medium text-gray-700 text-sm">Belum Mulai</h3>
                  <span class="text-xs text-gray-500">{{ todoTasks.length }}</span>
                </div>
              </div>
              <div class="space-y-2">
                <div v-for="task in todoTasks" :key="task.id" class="bg-white rounded-md p-3 border border-gray-200">
                  <div class="flex justify-between items-start mb-1">
                    <span :class="['text-xs px-1.5 py-0.5 rounded', task.priority === 'high' ? 'bg-red-50 text-red-600' : task.priority === 'medium' ? 'bg-orange-50 text-orange-600' : 'bg-green-50 text-green-600']">{{ task.priority }}</span>
                    <div class="flex gap-1">
                      <button @click="moveTask(task, 'in_progress')" class="text-gray-400 hover:text-gray-600 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
                      </button>
                      <button @click="deleteTask(task.id)" class="text-gray-400 hover:text-red-500 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                      </button>
                    </div>
                  </div>
                  <h4 class="font-medium text-gray-800 text-sm">{{ task.title }}</h4>
                  <p class="text-xs text-gray-500 mt-1">{{ task.description }}</p>
                </div>
                <div v-if="todoTasks.length === 0" class="text-center py-6 text-gray-400 text-xs">Tidak ada tugas</div>
              </div>
            </div>

            <!-- In Progress Column -->
            <div class="bg-gray-50 rounded-lg p-3">
              <div class="flex items-center justify-between mb-3">
                <div class="flex items-center gap-2">
                  <div class="w-2 h-2 bg-gray-500 rounded-full"></div>
                  <h3 class="font-medium text-gray-700 text-sm">Dikerjakan</h3>
                  <span class="text-xs text-gray-500">{{ inProgressTasks.length }}</span>
                </div>
              </div>
              <div class="space-y-2">
                <div v-for="task in inProgressTasks" :key="task.id" class="bg-white rounded-md p-3 border border-gray-200">
                  <div class="flex justify-between items-start mb-1">
                    <span :class="['text-xs px-1.5 py-0.5 rounded', task.priority === 'high' ? 'bg-red-50 text-red-600' : task.priority === 'medium' ? 'bg-orange-50 text-orange-600' : 'bg-green-50 text-green-600']">{{ task.priority }}</span>
                    <div class="flex gap-1">
                      <button @click="moveTask(task, 'todo')" class="text-gray-400 hover:text-gray-600 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/></svg>
                      </button>
                      <button @click="moveTask(task, 'done')" class="text-gray-400 hover:text-gray-600 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                      </button>
                      <button @click="deleteTask(task.id)" class="text-gray-400 hover:text-red-500 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                      </button>
                    </div>
                  </div>
                  <h4 class="font-medium text-gray-800 text-sm">{{ task.title }}</h4>
                  <p class="text-xs text-gray-500 mt-1">{{ task.description }}</p>
                </div>
                <div v-if="inProgressTasks.length === 0" class="text-center py-6 text-gray-400 text-xs">Tidak ada tugas</div>
              </div>
            </div>

            <!-- Done Column -->
            <div class="bg-gray-50 rounded-lg p-3">
              <div class="flex items-center justify-between mb-3">
                <div class="flex items-center gap-2">
                  <div class="w-2 h-2 bg-gray-600 rounded-full"></div>
                  <h3 class="font-medium text-gray-700 text-sm">Selesai</h3>
                  <span class="text-xs text-gray-500">{{ doneTasks.length }}</span>
                </div>
              </div>
              <div class="space-y-2">
                <div v-for="task in doneTasks" :key="task.id" class="bg-white rounded-md p-3 border border-gray-200 opacity-75">
                  <div class="flex justify-between items-start mb-1">
                    <span :class="['text-xs px-1.5 py-0.5 rounded', task.priority === 'high' ? 'bg-red-50 text-red-600' : task.priority === 'medium' ? 'bg-orange-50 text-orange-600' : 'bg-green-50 text-green-600']">{{ task.priority }}</span>
                    <div class="flex gap-1">
                      <button @click="moveTask(task, 'in_progress')" class="text-gray-400 hover:text-gray-600 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/></svg>
                      </button>
                      <button @click="deleteTask(task.id)" class="text-gray-400 hover:text-red-500 text-xs">
                        <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                      </button>
                    </div>
                  </div>
                  <h4 class="font-medium text-gray-500 text-sm line-through">{{ task.title }}</h4>
                  <p class="text-xs text-gray-400 line-through">{{ task.description }}</p>
                </div>
                <div v-if="doneTasks.length === 0" class="text-center py-6 text-gray-400 text-xs">Belum ada tugas</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Notes Tab -->
        <div v-if="activeTab === 'notes'" class="space-y-5">
          <div class="flex justify-between items-center">
            <div>
              <h2 class="text-lg font-semibold text-gray-800">Sticky Notes</h2>
              <p class="text-xs text-gray-500">Catat ide-ide Anda</p>
            </div>
            <button @click="createNewNote" class="px-3 py-1.5 bg-gray-800 text-white rounded-md text-sm hover:bg-gray-700 transition flex items-center gap-1">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/>
              </svg>
              Catatan Baru
            </button>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
            <div v-for="note in notes" :key="note.id" class="bg-white rounded-lg border border-gray-200 p-3 hover:shadow-sm transition">
              <div class="flex justify-between items-start mb-2">
                <div class="flex gap-1">
                  <button @click="updateNoteColor(note, 'pastel-blue')" class="w-4 h-4 rounded-full bg-blue-200 hover:ring-1 hover:ring-gray-400"></button>
                  <button @click="updateNoteColor(note, 'pastel-pink')" class="w-4 h-4 rounded-full bg-pink-200 hover:ring-1 hover:ring-gray-400"></button>
                  <button @click="updateNoteColor(note, 'pastel-purple')" class="w-4 h-4 rounded-full bg-purple-200 hover:ring-1 hover:ring-gray-400"></button>
                  <button @click="updateNoteColor(note, 'pastel-green')" class="w-4 h-4 rounded-full bg-green-200 hover:ring-1 hover:ring-gray-400"></button>
                  <button @click="updateNoteColor(note, 'pastel-amber')" class="w-4 h-4 rounded-full bg-amber-200 hover:ring-1 hover:ring-gray-400"></button>
                </div>
                <button @click="deleteNote(note.id)" class="text-gray-400 hover:text-gray-600">
                  <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                </button>
              </div>
              <input type="text" v-model="note.title" @blur="updateNote(note)" placeholder="Judul..." class="w-full bg-transparent border-none text-sm font-medium text-gray-800 focus:outline-none mb-1 placeholder-gray-400 p-0" />
              <textarea v-model="note.content" @blur="updateNote(note)" placeholder="Tulis sesuatu..." rows="3" class="w-full bg-transparent border-none text-xs text-gray-600 focus:outline-none resize-none placeholder-gray-400 p-0"></textarea>
            </div>
            <div v-if="notes.length === 0" class="col-span-full">
              <div class="text-center py-10 bg-gray-50 rounded-lg border border-gray-200">
                <svg class="w-12 h-12 mx-auto text-gray-400 mb-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/>
                </svg>
                <p class="text-sm text-gray-500">Belum ada catatan. Klik tombol di atas untuk membuat catatan!</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Zen Timer Tab -->
        <div v-if="activeTab === 'zen'" class="space-y-5">
          <div class="flex justify-between items-center">
            <div>
              <h2 class="text-lg font-semibold text-gray-800">Lynxx Focus Timer</h2>
              <p class="text-xs text-gray-500">Fokus dengan teknik Pomodoro</p>
            </div>
          </div>

          <div class="grid grid-cols-1 lg:grid-cols-2 gap-5">
            <div class="bg-white rounded-lg border border-gray-200 p-6 text-center">
              <div class="flex justify-center mb-4">
                <svg class="w-12 h-12 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
                </svg>
              </div>
              <h3 class="font-semibold text-gray-800 mb-1">Timer</h3>
              <p class="text-xs text-gray-500 mb-6">25 menit fokus, 5 menit istirahat</p>
              <div class="relative w-48 h-48 mx-auto mb-6">
                <svg class="w-full h-full transform -rotate-90">
                  <circle cx="96" cy="96" r="88" stroke="#e5e7eb" stroke-width="6" fill="none"/>
                  <circle cx="96" cy="96" r="88" stroke="#9ca3af" stroke-width="6" fill="none" stroke-dasharray="552.92" :stroke-dashoffset="timerProgress" class="transition-all duration-1000"/>
                </svg>
                <div class="absolute inset-0 flex flex-col items-center justify-center">
                  <div class="text-3xl font-semibold text-gray-800 font-mono">{{ formattedTime }}</div>
                  <div class="text-xs text-gray-500 mt-1">{{ isBreakTime ? "Istirahat" : "Fokus" }}</div>
                </div>
              </div>
              <div class="flex justify-center gap-3">
                <button @click="resetTimer" class="w-9 h-9 rounded-full bg-gray-100 text-gray-600 hover:bg-gray-200 flex items-center justify-center">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/>
                  </svg>
                </button>
                <button @click="toggleTimer" class="px-5 py-2 bg-gray-800 text-white rounded-md text-sm hover:bg-gray-700 flex items-center gap-1">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <polygon v-if="!isTimerRunning" points="5 3 19 12 5 21 5 3"/>
                    <template v-else>
                      <rect x="6" y="4" width="4" height="16" rx="1" />
                      <rect x="14" y="4" width="4" height="16" rx="1" />
                    </template>
                  </svg>
                  {{ isTimerRunning ? "Jeda" : "Mulai" }}
                </button>
                <button @click="skipTimer" class="w-9 h-9 rounded-full bg-gray-100 text-gray-600 hover:bg-gray-200 flex items-center justify-center">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5l7 7-7 7M5 5l7 7-7 7"/>
                  </svg>
                </button>
              </div>
            </div>

            <div class="bg-white rounded-lg border border-gray-200 p-6">
              <div class="flex items-center gap-2 mb-4">
                <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"/>
                </svg>
                <h3 class="font-semibold text-gray-800">Pilih Tema</h3>
              </div>
              <p class="text-xs text-gray-500 mb-4">Sesuaikan tampilan dashboard Anda</p>
              <div class="space-y-2">
                <button v-for="theme in themes" :key="theme.id" @click="applyTheme(theme.id)" :class="[
                  'w-full flex items-center gap-3 p-3 rounded-md transition-all text-sm',
                  currentTheme === theme.id ? 'bg-gray-100 border border-gray-200 ring-1 ring-gray-400' : 'bg-gray-50 hover:bg-gray-100 border border-gray-200'
                ]">
                  <div :class="[
                    'w-8 h-8 rounded-md',
                    theme.id === 'space' ? 'bg-gradient-to-br from-gray-600 to-gray-800' : 
                    theme.id === 'sunset' ? 'bg-gradient-to-br from-orange-400 to-red-500' : 
                    'bg-gradient-to-br from-emerald-400 to-teal-500'
                  ]"></div>
                  <div class="flex-1 text-left">
                    <h4 class="font-medium text-gray-800">{{ theme.name }}</h4>
                    <p class="text-xs text-gray-500">{{ theme.description }}</p>
                  </div>
                  <div v-if="currentTheme === theme.id" class="w-5 h-5 rounded-full bg-gray-800 flex items-center justify-center">
                    <svg class="w-3 h-3 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
                    </svg>
                  </div>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Task Modal -->
    <div v-if="showTaskModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/20" @click.self="showTaskModal = false">
      <div class="bg-white rounded-lg max-w-md w-full p-5 shadow-xl">
        <h3 class="text-lg font-semibold text-gray-800 mb-4">Tambah Tugas</h3>
        <form @submit.prevent="addTask" class="space-y-3">
          <div>
            <label class="block text-xs font-medium text-gray-700 mb-1">Judul</label>
            <input type="text" v-model="newTaskData.title" required class="w-full px-3 py-2 border border-gray-300 rounded-md text-sm focus:outline-none focus:ring-1 focus:ring-gray-400" />
          </div>
          <div>
            <label class="block text-xs font-medium text-gray-700 mb-1">Deskripsi</label>
            <textarea v-model="newTaskData.description" rows="3" class="w-full px-3 py-2 border border-gray-300 rounded-md text-sm focus:outline-none focus:ring-1 focus:ring-gray-400"></textarea>
          </div>
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-xs font-medium text-gray-700 mb-1">Prioritas</label>
              <select v-model="newTaskData.priority" class="w-full px-3 py-2 border border-gray-300 rounded-md text-sm">
                <option value="low">Rendah</option>
                <option value="medium">Sedang</option>
                <option value="high">Tinggi</option>
              </select>
            </div>
            <div>
              <label class="block text-xs font-medium text-gray-700 mb-1">Status</label>
              <select v-model="newTaskData.status" class="w-full px-3 py-2 border border-gray-300 rounded-md text-sm">
                <option value="todo">Belum Mulai</option>
                <option value="in_progress">Dikerjakan</option>
                <option value="done">Selesai</option>
              </select>
            </div>
          </div>
          <div class="flex gap-2 pt-3">
            <button type="button" @click="showTaskModal = false" class="flex-1 px-3 py-2 border border-gray-300 rounded-md text-sm text-gray-700 hover:bg-gray-50">Batal</button>
            <button type="submit" class="flex-1 px-3 py-2 bg-gray-800 text-white rounded-md text-sm hover:bg-gray-700">Simpan</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const mobileMenuOpen = ref(false); // Untuk mobile sidebar

// User data
const user = ref({ name: "User", email: "" });
const userInitial = computed(() => user.value.name ? user.value.name.charAt(0).toUpperCase() : "U");

// Date and greeting
const currentDateString = computed(() => {
  const options = { weekday: "long", year: "numeric", month: "long", day: "numeric" };
  return new Date().toLocaleDateString("id-ID", options);
});

const greetingText = computed(() => {
  const hours = new Date().getHours();
  if (hours < 11) return `Selamat Pagi, ${user.value.name}`;
  if (hours < 15) return `Selamat Siang, ${user.value.name}`;
  if (hours < 19) return `Selamat Sore, ${user.value.name}`;
  return `Selamat Malam, ${user.value.name}`;
});

// Menu items
const activeTab = ref("workspace");
const menuItems = [
  { id: "workspace", label: "Workspace", icon: '<svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/></svg>' },
  { id: "tasks", label: "Tugas", icon: '<svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"/></svg>' },
  { id: "notes", label: "Catatan", icon: '<svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/></svg>' },
  { id: "zen", label: "Lynxx Timer", icon: '<svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>' }
];

// Toast
const toast = ref({ message: "", type: "success" });
const showToast = (message, type = "success") => {
  toast.value = { message, type };
  setTimeout(() => { toast.value.message = ""; }, 3000);
};

// Sound Options
const soundOptions = [
  { id: "lofi-chill", name: "Lo-fi Chill", icon: '<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3"/></svg>', description: "Musik lo-fi santai untuk fokus maksimal", frequency: 400, type: "melody" },
  { id: "jazz-smooth", name: "Jazz Santai", icon: '<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 9h8M8 13h6M8 17h4M6 5h12a2 2 0 012 2v10a2 2 0 01-2 2H6a2 2 0 01-2-2V7a2 2 0 012-2z"/><path d="M12 9v8"/></svg>', description: "Jazz lembut yang menenangkan pikiran", frequency: 380, type: "melody" },
  { id: "piano-relax", name: "Piano Relaks", icon: '<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3h14a2 2 0 012 2v14a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2zm0 0v4h14V3M7 7v10M12 7v10M17 7v10"/></svg>', description: "Alunan piano yang damai dan menyejukkan", frequency: 420, type: "melody" },
  { id: "acoustic", name: "Akustik", icon: '<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4"/></svg>', description: "Guitar akustik untuk suasana hangat", frequency: 390, type: "melody" },
  { id: "rain-nature", name: "Hujan & Alam", icon: '<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 15a4 4 0 004 4h9a5 5 0 10-.1-9.999 5.002 5.002 0 10-9.78 2.096A4.001 4.001 0 003 15z"/><path d="M12 19v2m-4-2v2m8-2v2"/></svg>', description: "Suara hujan dan alam yang natural", frequency: 350, type: "noise" },
  { id: "ocean-waves", name: "Ombak Laut", icon: '<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12h.01M7 12h.01M11 12h.01M15 12h.01M19 12h.01M21 12h.01M4 8h.01M8 8h.01M12 8h.01M16 8h.01M20 8h.01M5 16h.01M9 16h.01M13 16h.01M17 16h.01M21 16h.01"/></svg>', description: "Suara ombak yang menenangkan hati", frequency: 280, type: "noise" }
];

const currentSound = ref("lofi-chill");
let audioCtx = null;
let currentSource = null;
let currentGain = null;
let melodyInterval = null;
const isNoisePlaying = ref(false);
const noiseVolume = ref(0.15);

const getCurrentSoundName = () => {
  const sound = soundOptions.find((s) => s.id === currentSound.value);
  return sound ? sound.name : "Lo-fi Chill";
};

const getSoundDescription = () => {
  const sound = soundOptions.find((s) => s.id === currentSound.value);
  return sound ? sound.description : "Musik lo-fi santai untuk fokus";
};

const selectSound = (soundId) => {
  if (isNoisePlaying.value) {
    stopAmbientNoise();
    currentSound.value = soundId;
    startAmbientNoise();
  } else {
    currentSound.value = soundId;
  }
};

// Audio functions (sama seperti sebelumnya)
const playLofiMelody = () => {
  const chords = [
    { notes: [261.63, 329.63, 392.0], duration: 2.0 },
    { notes: [293.66, 349.23, 440.0], duration: 2.0 },
    { notes: [329.63, 392.0, 493.88], duration: 2.0 },
    { notes: [261.63, 329.63, 392.0], duration: 2.0 }
  ];
  let chordIndex = 0;
  const playChord = () => {
    if (!isNoisePlaying.value || currentSound.value !== "lofi-chill") return;
    const chord = chords[chordIndex % chords.length];
    const now = audioCtx.currentTime;
    chord.notes.forEach((freq) => {
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = "sine";
      osc.frequency.value = freq;
      gain.gain.value = noiseVolume.value * 0.25;
      osc.detune.value = (Math.random() - 0.5) * 5;
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      gain.gain.exponentialRampToValueAtTime(0.00001, now + chord.duration);
      osc.stop(now + chord.duration);
    });
    chordIndex++;
    setTimeout(playChord, chord.duration * 1000);
  };
  playChord();
};

const playJazzMelody = () => {
  const notes = [261.63, 293.66, 311.13, 349.23, 392.0, 440.0, 493.88, 523.25];
  let noteIndex = 0;
  const playNote = () => {
    if (!isNoisePlaying.value || currentSound.value !== "jazz-smooth") return;
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = "sine";
    osc.frequency.value = notes[noteIndex % notes.length];
    gain.gain.value = noiseVolume.value * 0.2;
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start();
    gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 1.2);
    osc.stop(audioCtx.currentTime + 1.2);
    noteIndex++;
    setTimeout(playNote, 1500);
  };
  playNote();
};

const playPianoMelody = () => {
  const melody = [261.63, 293.66, 329.63, 261.63, 329.63, 349.23, 392.0, 349.23, 329.63, 293.66, 261.63];
  let step = 0;
  const playPianoNote = () => {
    if (!isNoisePlaying.value || currentSound.value !== "piano-relax") return;
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = "sine";
    osc.frequency.value = melody[step % melody.length];
    gain.gain.value = noiseVolume.value * 0.22;
    gain.gain.setValueAtTime(0, audioCtx.currentTime);
    gain.gain.linearRampToValueAtTime(noiseVolume.value * 0.22, audioCtx.currentTime + 0.02);
    gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 1.5);
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start();
    osc.stop(audioCtx.currentTime + 1.5);
    step++;
    setTimeout(playPianoNote, 1800);
  };
  playPianoNote();
};

const playAcousticMelody = () => {
  const arpeggio = [130.81, 164.81, 196.0, 261.63, 196.0, 164.81, 130.81];
  let index = 0;
  const playArpeggio = () => {
    if (!isNoisePlaying.value || currentSound.value !== "acoustic") return;
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = "triangle";
    osc.frequency.value = arpeggio[index % arpeggio.length];
    gain.gain.value = noiseVolume.value * 0.18;
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start();
    gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 0.8);
    osc.stop(audioCtx.currentTime + 0.8);
    index++;
    setTimeout(playArpeggio, 1000);
  };
  playArpeggio();
};

const playAmbientNoise = (frequency) => {
  const bufferSize = 2 * audioCtx.sampleRate;
  const noiseBuffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
  const output = noiseBuffer.getChannelData(0);
  for (let i = 0; i < bufferSize; i++) {
    output[i] = Math.random() * 2 - 1;
  }
  currentSource = audioCtx.createBufferSource();
  currentSource.buffer = noiseBuffer;
  currentSource.loop = true;
  const filter = audioCtx.createBiquadFilter();
  filter.type = "lowpass";
  filter.frequency.value = frequency;
  currentGain = audioCtx.createGain();
  currentGain.gain.value = noiseVolume.value;
  currentSource.connect(filter);
  filter.connect(currentGain);
  currentGain.connect(audioCtx.destination);
  currentSource.start();
};

const startAmbientNoise = () => {
  try {
    if (!audioCtx) {
      audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }
    if (audioCtx.state === "suspended") {
      audioCtx.resume();
    }
    const sound = soundOptions.find((s) => s.id === currentSound.value);
    isNoisePlaying.value = true;
    if (sound.type === "melody") {
      switch (currentSound.value) {
        case "lofi-chill": playLofiMelody(); break;
        case "jazz-smooth": playJazzMelody(); break;
        case "piano-relax": playPianoMelody(); break;
        case "acoustic": playAcousticMelody(); break;
      }
    } else {
      playAmbientNoise(sound.frequency);
    }
  } catch (e) {
    console.error("Audio error:", e);
    showToast("Gagal memutar audio", "error");
    isNoisePlaying.value = false;
  }
};

const toggleAmbientNoise = () => {
  if (isNoisePlaying.value) {
    stopAmbientNoise();
  } else {
    startAmbientNoise();
  }
};

const updateNoiseVolume = () => {
  if (currentGain) {
    currentGain.gain.value = noiseVolume.value;
  }
};

const stopAmbientNoise = () => {
  if (currentSource) {
    try { currentSource.stop(); } catch (e) {}
    currentSource = null;
  }
  if (melodyInterval) {
    clearTimeout(melodyInterval);
    melodyInterval = null;
  }
  isNoisePlaying.value = false;
};

// Tasks
const tasks = ref([]);
const showTaskModal = ref(false);
const newTaskData = ref({ title: "", description: "", priority: "medium", status: "todo" });

const todoTasks = computed(() => tasks.value.filter((t) => t.status === "todo"));
const inProgressTasks = computed(() => tasks.value.filter((t) => t.status === "in_progress"));
const doneTasks = computed(() => tasks.value.filter((t) => t.status === "done"));
const completedTasksCount = computed(() => doneTasks.value.length);

const fetchTasks = async () => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch("https://backendrepo-production-2f75.up.railway.app/api/tasks", {
      headers: { Authorization: `Bearer ${token}` }
    });
    if (res.ok) tasks.value = await res.json();
  } catch (err) { console.error(err); }
};

const addTask = async () => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch("https://backendrepo-production-2f75.up.railway.app/api/tasks", {
      method: "POST",
      headers: { "Content-Type": "application/json", Authorization: `Bearer ${token}` },
      body: JSON.stringify(newTaskData.value)
    });
    if (res.ok) {
      const task = await res.json();
      tasks.value.push(task);
      newTaskData.value = { title: "", description: "", priority: "medium", status: "todo" };
      showTaskModal.value = false;
      showToast("Tugas ditambahkan");
    }
  } catch (err) { showToast("Gagal menambahkan", "error"); }
};

const moveTask = async (task, newStatus) => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch(`https://backendrepo-production-2f75.up.railway.app/api/tasks/${task.id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json", Authorization: `Bearer ${token}` },
      body: JSON.stringify({ status: newStatus })
    });
    if (res.ok) {
      const updated = await res.json();
      const index = tasks.value.findIndex((t) => t.id === task.id);
      if (index !== -1) tasks.value[index] = updated;
    }
  } catch (err) { console.error(err); }
};

const deleteTask = async (taskId) => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch(`https://backendrepo-production-2f75.up.railway.app/api/tasks/${taskId}`, {
      method: "DELETE",
      headers: { Authorization: `Bearer ${token}` }
    });
    if (res.ok) {
      tasks.value = tasks.value.filter((t) => t.id !== taskId);
      showToast("Tugas dihapus");
    }
  } catch (err) { showToast("Gagal menghapus", "error"); }
};

// Notes
const notes = ref([]);
const noteColors = ["pastel-blue", "pastel-pink", "pastel-purple", "pastel-green", "pastel-amber"];

const fetchNotes = async () => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch("https://backendrepo-production-2f75.up.railway.app/api/notes", {
      headers: { Authorization: `Bearer ${token}` }
    });
    if (res.ok) notes.value = await res.json();
  } catch (err) { console.error(err); }
};

const createNewNote = async () => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch("https://backendrepo-production-2f75.up.railway.app/api/notes", {
      method: "POST",
      headers: { "Content-Type": "application/json", Authorization: `Bearer ${token}` },
      body: JSON.stringify({ title: "", content: "", color: noteColors[0] })
    });
    if (res.ok) {
      const note = await res.json();
      notes.value.unshift(note);
    }
  } catch (err) { showToast("Gagal membuat catatan", "error"); }
};

const updateNote = async (note) => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    await fetch(`https://backendrepo-production-2f75.up.railway.app/api/notes/${note.id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json", Authorization: `Bearer ${token}` },
      body: JSON.stringify({ title: note.title, content: note.content, color: note.color })
    });
  } catch (err) { console.error(err); }
};

const updateNoteColor = async (note, color) => {
  note.color = color;
  await updateNote(note);
};

const deleteNote = async (noteId) => {
  try {
    const token = localStorage.getItem("LynxxSpace_token");
    const res = await fetch(`https://backendrepo-production-2f75.up.railway.app/api/notes/${noteId}`, {
      method: "DELETE",
      headers: { Authorization: `Bearer ${token}` }
    });
    if (res.ok) {
      notes.value = notes.value.filter((n) => n.id !== noteId);
      showToast("Catatan dihapus");
    }
  } catch (err) { showToast("Gagal menghapus", "error"); }
};

// Timer
const timerSeconds = ref(25 * 60);
const isTimerRunning = ref(false);
const isBreakTime = ref(false);
let timerInterval = null;

const formattedTime = computed(() => {
  const m = Math.floor(timerSeconds.value / 60).toString().padStart(2, "0");
  const s = (timerSeconds.value % 60).toString().padStart(2, "0");
  return `${m}:${s}`;
});

const timerProgress = computed(() => {
  const total = isBreakTime.value ? 5 * 60 : 25 * 60;
  const circumference = 2 * Math.PI * 88;
  const progress = (timerSeconds.value / total) * circumference;
  return circumference - progress;
});

const toggleTimer = () => {
  if (isTimerRunning.value) {
    clearInterval(timerInterval);
    isTimerRunning.value = false;
  } else {
    isTimerRunning.value = true;
    timerInterval = setInterval(() => {
      if (timerSeconds.value > 0) {
        timerSeconds.value--;
      } else {
        playAlertTone();
        isBreakTime.value = !isBreakTime.value;
        timerSeconds.value = isBreakTime.value ? 5 * 60 : 25 * 60;
        showToast(isBreakTime.value ? "Waktu istirahat! 🎉" : "Waktu fokus selesai! 💪");
      }
    }, 1000);
  }
};

const resetTimer = () => {
  clearInterval(timerInterval);
  isTimerRunning.value = false;
  isBreakTime.value = false;
  timerSeconds.value = 25 * 60;
};

const skipTimer = () => {
  isBreakTime.value = !isBreakTime.value;
  timerSeconds.value = isBreakTime.value ? 5 * 60 : 25 * 60;
};

const playAlertTone = () => {
  try {
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.type = "sine";
    osc.frequency.value = 880;
    gain.gain.value = 0.2;
    osc.connect(gain);
    gain.connect(ctx.destination);
    osc.start();
    osc.stop(ctx.currentTime + 0.3);
    setTimeout(() => ctx.close(), 400);
  } catch (e) {}
};

// Themes
const currentTheme = ref("space");
const themes = [
  { id: "space", name: "Default", description: "Netral dan tenang" },
  { id: "sunset", name: "Sunset", description: "Hangat dan nyaman" },
  { id: "emerald", name: "Emerald", description: "Segar dan alami" }
];

const applyTheme = (themeId) => {
  currentTheme.value = themeId;
  document.body.classList.remove("theme-space", "theme-sunset", "theme-emerald");
  document.body.classList.add(`theme-${themeId}`);
  showToast(`Tema: ${themes.find(t => t.id === themeId).name}`);
};

// Logout
const handleLogout = () => {
  stopAmbientNoise();
  clearInterval(timerInterval);
  if (audioCtx) audioCtx.close();
  localStorage.removeItem("LynxxSpace_token");
  localStorage.removeItem("LynxxSpace_user");
  router.push("/login");
};

// Lifecycle
onMounted(() => {
  const storedUser = localStorage.getItem("LynxxSpace_user");
  if (storedUser) user.value = JSON.parse(storedUser);
  fetchTasks();
  fetchNotes();
});

onUnmounted(() => {
  stopAmbientNoise();
  clearInterval(timerInterval);
  if (audioCtx) audioCtx.close();
});
</script>

<style scoped>
@keyframes slide-in {
  from { transform: translateX(100%); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}
.animate-slide-in { animation: slide-in 0.3s ease-out; }
.animation-delay-100 { animation-delay: 100ms; }
.animation-delay-200 { animation-delay: 200ms; }
.animation-delay-300 { animation-delay: 300ms; }
.animation-delay-400 { animation-delay: 400ms; }
@keyframes bounce {
  0%, 100% { transform: scaleY(0.5); }
  50% { transform: scaleY(1); }
}
.animate-bounce { animation: bounce 0.6s ease-in-out infinite; }
</style>

<style>
/* Tema Space (Default) */
body.theme-space {
  background-color: #f9fafb;
}
body.theme-space .bg-white,
body.theme-space aside.bg-white,
body.theme-space .modal-card.bg-white,
body.theme-space .rounded-lg.bg-white {
  background-color: #ffffff !important;
}
body.theme-space .bg-gray-50,
body.theme-space .rounded-lg.bg-gray-50 {
  background-color: #f9fafb !important;
}
body.theme-space .bg-gray-100 {
  background-color: #f3f4f6 !important;
}
body.theme-space .border-gray-200,
body.theme-space .border.border-gray-200 {
  border-color: #e5e7eb !important;
}
body.theme-space .text-gray-800,
body.theme-space h1.text-gray-800,
body.theme-space h2.text-gray-800 {
  color: #1f2937 !important;
}
body.theme-space .text-gray-500,
body.theme-space .text-gray-600 {
  color: #6b7280 !important;
}
body.theme-space .bg-gray-800,
body.theme-space button.bg-gray-800 {
  background-color: #1f2937 !important;
}
body.theme-space .bg-gray-800:hover {
  background-color: #374151 !important;
}

/* Tema Sunset */
body.theme-sunset {
  background-color: #fff7ed;
}
body.theme-sunset .bg-white,
body.theme-sunset aside.bg-white {
  background-color: #ffffff !important;
}
body.theme-sunset .bg-gray-50 {
  background-color: #fff7ed !important;
}
body.theme-sunset .bg-gray-100 {
  background-color: #ffedd5 !important;
}
body.theme-sunset .border-gray-200 {
  border-color: #fed7aa !important;
}
body.theme-sunset .text-gray-800 {
  color: #7c2d12 !important;
}
body.theme-sunset .text-gray-500,
body.theme-sunset .text-gray-600 {
  color: #9a3412 !important;
}
body.theme-sunset .bg-gray-800 {
  background-color: #9a3412 !important;
}
body.theme-sunset .bg-gray-800:hover {
  background-color: #7c2d12 !important;
}

/* Tema Emerald */
body.theme-emerald {
  background-color: #ecfdf5;
}
body.theme-emerald .bg-white,
body.theme-emerald aside.bg-white {
  background-color: #ffffff !important;
}
body.theme-emerald .bg-gray-50 {
  background-color: #ecfdf5 !important;
}
body.theme-emerald .bg-gray-100 {
  background-color: #d1fae5 !important;
}
body.theme-emerald .border-gray-200 {
  border-color: #a7f3d0 !important;
}
body.theme-emerald .text-gray-800 {
  color: #064e3b !important;
}
body.theme-emerald .text-gray-500,
body.theme-emerald .text-gray-600 {
  color: #047857 !important;
}
body.theme-emerald .bg-gray-800 {
  background-color: #047857 !important;
}
body.theme-emerald .bg-gray-800:hover {
  background-color: #065f46 !important;
}
</style>