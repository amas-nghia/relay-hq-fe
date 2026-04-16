<script setup lang="ts">
import { 
  Bot, 
  Search, 
  Plus, 
  Activity, 
  Settings, 
  Cpu, 
  Zap,
  MoreVertical,
  ChevronRight
} from 'lucide-vue-next'

const agents = [
  { id: 'agent-001', name: 'agent-dev', role: 'Backend Developer', status: 'online', tasks: 3, successRate: '98%', model: 'Gemini 2.0 Flash', uptime: '14d 2h' },
  { id: 'agent-002', name: 'agent-db', role: 'Database Administrator', status: 'online', tasks: 1, successRate: '95%', model: 'Gemini 1.5 Pro', uptime: '8d 5h' },
  { id: 'agent-003', name: 'agent-qa', role: 'Quality Assurance', status: 'idle', tasks: 0, successRate: '99%', model: 'Gemini 2.0 Flash', uptime: '2d 1h' },
  { id: 'agent-004', name: 'agent-ops', role: 'DevOps Engineer', status: 'offline', tasks: 0, successRate: '92%', model: 'Gemini 1.5 Pro', uptime: '0d 0h' },
]
</script>

<template>
  <div class="space-y-8">
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Management</p>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">Agent Registry</h1>
        <p class="text-slate-500 text-sm">Manage and monitor autonomous agents operating within the workspace.</p>
      </div>
      
      <div class="flex items-center gap-3">
        <button class="flex items-center gap-2 px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors shadow-lg shadow-violet-200">
          <Plus :size="16" />
          Register Agent
        </button>
      </div>
    </header>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <div v-for="agent in agents" :key="agent.id" class="glass-card p-6 space-y-6 group hover:border-violet-200 transition-all">
        <div class="flex items-start justify-between">
          <div class="w-12 h-12 rounded-xl bg-violet-50 text-violet-600 flex items-center justify-center">
            <Bot :size="28" />
          </div>
          <div class="flex items-center gap-2">
            <div class="w-2 h-2 rounded-full" :class="agent.status === 'online' ? 'bg-emerald-500 animate-pulse' : agent.status === 'idle' ? 'bg-amber-500' : 'bg-slate-300'"></div>
            <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">{{ agent.status }}</span>
          </div>
        </div>

        <div class="space-y-1">
          <h3 class="text-lg font-bold text-slate-900 group-hover:text-violet-600 transition-colors">{{ agent.name }}</h3>
          <p class="text-xs text-slate-500 font-medium">{{ agent.role }}</p>
        </div>

        <div class="grid grid-cols-2 gap-4 pt-4 border-t border-slate-100">
          <div class="space-y-1">
            <p class="text-[9px] font-bold text-slate-400 uppercase tracking-widest">Model</p>
            <div class="flex items-center gap-1.5 text-[10px] font-bold text-slate-700">
              <Cpu :size="10" />
              {{ agent.model }}
            </div>
          </div>
          <div class="space-y-1">
            <p class="text-[9px] font-bold text-slate-400 uppercase tracking-widest">Success</p>
            <div class="flex items-center gap-1.5 text-[10px] font-bold text-emerald-600">
              <Zap :size="10" />
              {{ agent.successRate }}
            </div>
          </div>
        </div>

        <div class="flex items-center justify-between pt-4">
          <div class="flex items-center gap-2">
            <Activity :size="12" class="text-slate-400" />
            <span class="text-[10px] font-bold text-slate-600">{{ agent.tasks }} Active Tasks</span>
          </div>
          <button class="p-2 text-slate-300 hover:text-slate-600">
            <Settings :size="16" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
