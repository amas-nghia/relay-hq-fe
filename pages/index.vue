<script setup lang="ts">
import { 
  FolderKanban, 
  Activity, 
  CheckSquare, 
  ShieldAlert,
  ArrowUpRight,
  Clock,
  ChevronRight,
  Users,
  Bot
} from 'lucide-vue-next'

const stats = [
  { label: 'Active Projects', value: '3', icon: FolderKanban, color: 'text-violet-600', bg: 'bg-violet-50' },
  { label: 'Live Boards', value: '2', icon: Activity, color: 'text-blue-600', bg: 'bg-blue-50' },
  { label: 'Total Tasks', value: '12', icon: CheckSquare, color: 'text-emerald-600', bg: 'bg-emerald-50' },
  { label: 'Pending Approvals', value: '2', icon: ShieldAlert, color: 'text-amber-600', bg: 'bg-amber-50', alert: true },
]

const recentTasks = [
  { id: 'task-007', title: 'Deploy Auth Service', status: 'waiting-approval', priority: 'high', time: '2h ago', project: 'Auth Project' },
  { id: 'task-012', title: 'Database Migration', status: 'waiting-approval', priority: 'critical', time: '4h ago', project: 'Infra Project' },
]

const workload = [
  { type: 'Human', count: 4, color: 'bg-blue-500', icon: Users },
  { type: 'Agent', count: 8, color: 'bg-violet-500', icon: Bot },
]
</script>

<template>
  <div class="max-w-6xl mx-auto space-y-10">
    <!-- Header -->
    <header class="flex justify-between items-start">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Control Plane Dashboard</p>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">Acme Workspace</h1>
      </div>
      <div class="flex items-center gap-2 px-3 py-1.5 bg-emerald-50 border border-emerald-100 rounded-full">
        <div class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></div>
        <span class="text-[10px] font-bold text-emerald-700 uppercase tracking-wider">Vault Connected</span>
      </div>
    </header>

    <!-- Stats Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <div v-for="stat in stats" :key="stat.label" class="glass-card p-6 flex flex-col justify-between group hover:border-violet-200 transition-colors">
        <div class="flex justify-between items-start">
          <div :class="[stat.bg, stat.color, 'p-3 rounded-xl']">
            <component :is="stat.icon" :size="24" />
          </div>
          <ArrowUpRight :size="16" class="text-slate-300 group-hover:text-violet-400 transition-colors" />
        </div>
        <div class="mt-4">
          <p class="text-3xl font-bold text-slate-900">{{ stat.value }}</p>
          <p class="text-xs font-medium text-slate-500">{{ stat.label }}</p>
        </div>
      </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
      <!-- Urgent Actions -->
      <section class="lg:col-span-2 space-y-6">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2">
            <h2 class="text-xl font-bold tracking-tight">Waiting for My Action</h2>
            <span class="bg-amber-100 text-amber-700 text-[10px] font-bold px-2 py-0.5 rounded-full">2 URGENT</span>
          </div>
          <NuxtLink to="/approvals" class="text-xs font-bold text-violet-600 hover:text-violet-700">View All Approvals</NuxtLink>
        </div>
        
        <div class="space-y-3">
          <NuxtLink 
            v-for="task in recentTasks" 
            :key="task.id"
            :to="`/tasks/${task.id}`"
            class="glass-card p-5 flex items-center justify-between group hover:bg-white transition-all border-l-4 border-l-amber-500"
          >
            <div class="flex items-center gap-4">
              <div class="w-12 h-12 rounded-xl bg-amber-50 flex items-center justify-center text-amber-600">
                <ShieldAlert :size="24" />
              </div>
              <div>
                <div class="flex items-center gap-2 mb-1">
                  <span class="text-[9px] font-bold text-violet-600 bg-violet-50 px-1.5 py-0.5 rounded uppercase tracking-wider">{{ task.project }}</span>
                  <span class="text-[10px] font-mono text-slate-400 uppercase">{{ task.id }}</span>
                </div>
                <h3 class="text-base font-bold text-slate-900 group-hover:text-violet-600 transition-colors">{{ task.title }}</h3>
              </div>
            </div>
            <div class="flex items-center gap-6">
              <div class="text-right hidden sm:block">
                <p class="text-[10px] font-bold text-slate-400 uppercase">Requested</p>
                <p class="text-xs font-medium text-slate-600">{{ task.time }}</p>
              </div>
              <button class="px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors">
                Review
              </button>
            </div>
          </NuxtLink>
        </div>
      </section>

      <!-- Workload & Distribution -->
      <section class="space-y-6">
        <h2 class="text-xl font-bold tracking-tight">Workload Distribution</h2>
        <div class="glass-card p-6 space-y-8">
          <div class="space-y-4">
            <div v-for="item in workload" :key="item.type" class="space-y-2">
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <component :is="item.icon" :size="16" class="text-slate-400" />
                  <span class="text-xs font-bold text-slate-700">{{ item.type }}</span>
                </div>
                <span class="text-xs font-bold text-slate-900">{{ item.count }} tasks</span>
              </div>
              <div class="w-full h-2 bg-slate-100 rounded-full overflow-hidden">
                <div class="h-full rounded-full transition-all duration-1000" :class="item.color" :style="{ width: `${(item.count / 12) * 100}%` }"></div>
              </div>
            </div>
          </div>

          <div class="pt-6 border-t border-slate-100">
            <p class="text-[10px] font-bold text-slate-400 uppercase mb-4">Active Agents</p>
            <div class="grid grid-cols-2 gap-3">
              <div v-for="i in 4" :key="i" class="p-3 bg-slate-50 rounded-xl border border-slate-100 flex items-center gap-3">
                <div class="w-2 h-2 rounded-full bg-emerald-500"></div>
                <span class="text-[10px] font-bold text-slate-600 uppercase">Agent-0{{ i }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

