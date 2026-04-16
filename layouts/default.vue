<script setup lang="ts">
import { 
  LayoutDashboard, 
  FolderKanban, 
  CheckSquare, 
  ShieldCheck, 
  History,
  ChevronRight,
  Activity
} from 'lucide-vue-next'

const navItems = [
  { label: 'Overview', icon: LayoutDashboard, to: '/' },
  { label: 'Projects', icon: FolderKanban, to: '/projects' },
  { label: 'Board', icon: Activity, to: '/boards/auth-board' },
  { label: 'Tasks', icon: CheckSquare, to: '/tasks' },
]

const managementItems = [
  { label: 'Agents', icon: Bot, to: '/agents' },
  { label: 'Team', icon: Users, to: '/team' },
]

const governanceItems = [
  { label: 'Approvals', icon: ShieldCheck, to: '/approvals', badge: 2 },
  { label: 'Audit', icon: History, to: '/audit' },
]
</script>

<template>
  <div class="flex w-full min-h-screen">
    <!-- Sidebar -->
    <aside class="w-72 border-r border-slate-200 bg-white/50 backdrop-blur-xl sticky top-0 h-screen flex flex-col">
      <div class="p-6 overflow-y-auto flex-1">
        <div class="flex items-center gap-3 mb-8">
          <div class="w-10 h-10 bg-violet-600 rounded-xl flex items-center justify-center text-white shadow-lg shadow-violet-200">
            <Activity :size="24" />
          </div>
          <div>
            <h1 class="font-bold text-lg tracking-tight">RelayHQ</h1>
            <p class="text-[10px] uppercase tracking-widest text-slate-500 font-bold">Control Plane</p>
          </div>
        </div>

        <div class="space-y-8">
          <!-- Workspace Section -->
          <div>
            <p class="text-[10px] uppercase tracking-widest text-slate-400 font-bold mb-4 px-3">Workspace</p>
            <nav class="space-y-1">
              <NuxtLink 
                v-for="item in navItems" 
                :key="item.label"
                :to="item.to"
                class="flex items-center justify-between px-3 py-2 rounded-lg text-sm font-medium transition-all group"
                :class="[
                  $route.path === item.to 
                    ? 'bg-violet-50 text-violet-700' 
                    : 'text-slate-600 hover:bg-slate-50 hover:text-slate-900'
                ]"
              >
                <div class="flex items-center gap-3">
                  <component :is="item.icon" :size="18" :class="$route.path === item.to ? 'text-violet-600' : 'text-slate-400 group-hover:text-slate-600'" />
                  {{ item.label }}
                </div>
                <ChevronRight :size="14" class="opacity-0 group-hover:opacity-100 transition-opacity" />
              </NuxtLink>
            </nav>
          </div>

          <!-- Management Section -->
          <div>
            <p class="text-[10px] uppercase tracking-widest text-slate-400 font-bold mb-4 px-3">Management</p>
            <nav class="space-y-1">
              <NuxtLink 
                v-for="item in managementItems" 
                :key="item.label"
                :to="item.to"
                class="flex items-center justify-between px-3 py-2 rounded-lg text-sm font-medium transition-all group"
                :class="[
                  $route.path === item.to 
                    ? 'bg-violet-50 text-violet-700' 
                    : 'text-slate-600 hover:bg-slate-50 hover:text-slate-900'
                ]"
              >
                <div class="flex items-center gap-3">
                  <component :is="item.icon" :size="18" :class="$route.path === item.to ? 'text-violet-600' : 'text-slate-400 group-hover:text-slate-600'" />
                  {{ item.label }}
                </div>
                <ChevronRight :size="14" class="opacity-0 group-hover:opacity-100 transition-opacity" />
              </NuxtLink>
            </nav>
          </div>

          <!-- Governance Section -->
          <div>
            <p class="text-[10px] uppercase tracking-widest text-slate-400 font-bold mb-4 px-3">Governance</p>
            <nav class="space-y-1">
              <NuxtLink 
                v-for="item in governanceItems" 
                :key="item.label"
                :to="item.to"
                class="flex items-center justify-between px-3 py-2 rounded-lg text-sm font-medium transition-all group"
                :class="[
                  $route.path === item.to 
                    ? 'bg-violet-50 text-violet-700' 
                    : 'text-slate-600 hover:bg-slate-50 hover:text-slate-900'
                ]"
              >
                <div class="flex items-center gap-3">
                  <component :is="item.icon" :size="18" :class="$route.path === item.to ? 'text-violet-600' : 'text-slate-400 group-hover:text-slate-600'" />
                  {{ item.label }}
                </div>
                <div v-if="item.badge" class="bg-amber-100 text-amber-700 text-[10px] font-bold px-1.5 py-0.5 rounded-full">
                  {{ item.badge }}
                </div>
              </NuxtLink>
            </nav>
          </div>
        </div>
      </div>

      <div class="mt-auto p-6 border-t border-slate-100">
        <div class="flex items-center gap-3">
          <div class="w-8 h-8 rounded-full bg-slate-200 overflow-hidden">
            <ClientOnly>
              <img src="https://picsum.photos/seed/user/100/100" alt="User" referrerPolicy="no-referrer" />
            </ClientOnly>
          </div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-bold text-slate-900 truncate">Alice PM</p>
            <p class="text-[10px] text-slate-500 truncate">amasnghia@gmail.com</p>
          </div>
        </div>
      </div>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 overflow-y-auto p-8 lg:p-12">
      <slot />
    </main>
  </div>
</template>

<style scoped>
.router-link-active {
  @apply bg-violet-50 text-violet-700;
}
</style>
