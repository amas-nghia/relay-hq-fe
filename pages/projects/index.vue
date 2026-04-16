<script setup lang="ts">
import { 
  FolderKanban, 
  Users, 
  Activity, 
  ChevronRight,
  MoreVertical,
  Plus,
  Search,
  Filter
} from 'lucide-vue-next'

const projects = [
  { 
    id: 'PRJ-001', 
    name: 'Auth Project', 
    description: 'Authentication and Authorization service layer.',
    participants: ['Alice PM', 'agent-dev', 'agent-backend'],
    progress: 65,
    status: 'active',
    lastUpdated: '2h ago'
  },
  { 
    id: 'PRJ-002', 
    name: 'Infra Project', 
    description: 'Cloud infrastructure and database management.',
    participants: ['Alice PM', 'agent-db', 'agent-infra'],
    progress: 30,
    status: 'active',
    lastUpdated: '5h ago'
  },
  { 
    id: 'PRJ-003', 
    name: 'Frontend Project', 
    description: 'RelayHQ Web Interface development.',
    participants: ['Alice PM', 'agent-ui'],
    progress: 85,
    status: 'active',
    lastUpdated: '10m ago'
  },
]

const getStatusClass = (status: string) => {
  switch (status) {
    case 'active': return 'bg-emerald-50 text-emerald-700 border-emerald-100'
    case 'on-hold': return 'bg-amber-50 text-amber-700 border-amber-100'
    case 'completed': return 'bg-blue-50 text-blue-700 border-blue-100'
    default: return 'bg-slate-50 text-slate-700 border-slate-100'
  }
}
</script>

<template>
  <div class="max-w-6xl mx-auto space-y-8">
    <!-- Header -->
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Workspace</p>
        <h1 class="text-3xl font-bold tracking-tight text-slate-950">Projects</h1>
        <p class="text-slate-500 text-sm">Overview of all active and archived coordination projects.</p>
      </div>
      
      <div class="flex items-center gap-3">
        <div class="relative">
          <Search class="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" :size="14" />
          <input type="text" placeholder="Search projects..." class="pl-9 pr-4 py-2 bg-white border border-slate-200 rounded-lg text-xs focus:outline-none focus:ring-2 focus:ring-violet-500/20 focus:border-violet-500 transition-all w-64" />
        </div>
        <button class="flex items-center gap-2 px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors shadow-lg shadow-violet-200">
          <Plus :size="16" />
          New Project
        </button>
      </div>
    </header>

    <!-- Projects Table -->
    <div class="glass-card overflow-hidden">
      <table class="w-full text-left border-collapse">
        <thead>
          <tr class="border-bottom border-slate-100 bg-slate-50/50">
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Project Name</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Participants</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Progress</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Status</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest text-right">Actions</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-slate-100">
          <tr v-for="project in projects" :key="project.id" class="group hover:bg-slate-50/50 transition-colors">
            <td class="px-6 py-5">
              <div class="flex items-center gap-4">
                <div class="w-10 h-10 rounded-xl bg-violet-50 flex items-center justify-center text-violet-600">
                  <FolderKanban :size="20" />
                </div>
                <div>
                  <h3 class="text-sm font-bold text-slate-900 group-hover:text-violet-600 transition-colors">{{ project.name }}</h3>
                  <p class="text-[10px] font-mono text-slate-400 uppercase mt-0.5">{{ project.id }}</p>
                </div>
              </div>
            </td>
            <td class="px-6 py-5">
              <div class="flex items-center -space-x-2">
                <div v-for="(p, i) in project.participants.slice(0, 3)" :key="i" class="w-7 h-7 rounded-full border-2 border-white bg-slate-100 flex items-center justify-center text-[10px] font-bold text-slate-600" :title="p">
                  {{ p.charAt(0) }}
                </div>
                <div v-if="project.participants.length > 3" class="w-7 h-7 rounded-full border-2 border-white bg-slate-50 flex items-center justify-center text-[10px] font-bold text-slate-400">
                  +{{ project.participants.length - 3 }}
                </div>
              </div>
            </td>
            <td class="px-6 py-5">
              <div class="w-32">
                <div class="flex items-center justify-between mb-1.5">
                  <span class="text-[10px] font-bold text-slate-500">{{ project.progress }}%</span>
                </div>
                <div class="w-full h-1.5 bg-slate-100 rounded-full overflow-hidden">
                  <div class="h-full bg-violet-500 rounded-full" :style="{ width: `${project.progress}%` }"></div>
                </div>
              </div>
            </td>
            <td class="px-6 py-5">
              <span class="px-2 py-1 rounded text-[10px] font-bold uppercase border" :class="getStatusClass(project.status)">
                {{ project.status }}
              </span>
            </td>
            <td class="px-6 py-5 text-right">
              <div class="flex items-center justify-end gap-2">
                <NuxtLink :to="`/boards/auth-board`" class="p-2 text-slate-400 hover:text-violet-600 transition-colors">
                  <ChevronRight :size="18" />
                </NuxtLink>
                <button class="p-2 text-slate-400 hover:text-slate-600">
                  <MoreVertical :size="18" />
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Empty State if needed -->
    <div v-if="projects.length === 0" class="glass-card p-12 text-center space-y-4">
      <div class="w-16 h-16 bg-slate-50 rounded-full flex items-center justify-center mx-auto text-slate-300">
        <FolderKanban :size="32" />
      </div>
      <div class="space-y-1">
        <h3 class="text-lg font-bold text-slate-900">No projects found</h3>
        <p class="text-sm text-slate-500">Get started by creating your first coordination project.</p>
      </div>
      <button class="px-6 py-2 bg-violet-600 text-white rounded-lg text-sm font-bold hover:bg-violet-700 transition-colors">
        Create Project
      </button>
    </div>
  </div>
</template>
