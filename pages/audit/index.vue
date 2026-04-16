<script setup lang="ts">
import { 
  History, 
  Search, 
  Filter, 
  Download,
  Lock,
  User,
  Bot,
  ShieldCheck,
  Activity
} from 'lucide-vue-next'

const logs = [
  { id: 'log-101', action: 'Task Approved', actor: 'human-alice', type: 'human', target: 'task-007', project: 'Auth Project', time: '12m ago', details: 'Production access granted for deployment.' },
  { id: 'log-102', action: 'Status Updated', actor: 'agent-dev', type: 'agent', target: 'task-005', project: 'Auth Project', time: '25m ago', details: 'Moved from Todo to In Progress.' },
  { id: 'log-103', action: 'Vault Secret Written', actor: 'agent-db', type: 'agent', target: 'task-006', project: 'Infra Project', time: '1h ago', details: 'Database credentials updated in vault.' },
  { id: 'log-104', action: 'Project Created', actor: 'human-bob', type: 'human', target: 'Infra Project', project: 'N/A', time: '4h ago', details: 'New infrastructure workspace initialized.' },
  { id: 'log-105', action: 'Approval Requested', actor: 'agent-db', type: 'agent', target: 'task-012', project: 'Infra Project', time: '5h ago', details: 'Requested permission to delete legacy data.' },
]

const getActionIcon = (action: string) => {
  if (action.includes('Approved')) return ShieldCheck
  if (action.includes('Vault')) return Lock
  return Activity
}
</script>

<template>
  <div class="space-y-8">
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Governance</p>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">Audit Trail</h1>
        <p class="text-slate-500 text-sm">Immutable record of all actions and decisions within the control plane.</p>
      </div>
      
      <div class="flex items-center gap-3">
        <button class="flex items-center gap-2 px-4 py-2 bg-white border border-slate-200 rounded-lg text-xs font-bold text-slate-600 hover:bg-slate-50 transition-colors">
          <Download :size="16" />
          Export Logs
        </button>
      </div>
    </header>

    <div class="flex items-center gap-4 mb-6">
      <div class="relative flex-1">
        <Search class="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" :size="14" />
        <input type="text" placeholder="Filter audit logs..." class="pl-9 pr-4 py-2 bg-white border border-slate-200 rounded-lg text-xs focus:outline-none focus:ring-2 focus:ring-violet-500/20 focus:border-violet-500 transition-all w-full" />
      </div>
      <button class="flex items-center gap-2 px-4 py-2 bg-white border border-slate-200 rounded-lg text-xs font-bold text-slate-600 hover:bg-slate-50 transition-colors">
        <Filter :size="16" />
        Filters
      </button>
    </div>

    <div class="space-y-4">
      <div v-for="log in logs" :key="log.id" class="glass-card p-5 flex items-start gap-6 group hover:border-violet-200 transition-all">
        <div class="w-12 h-12 rounded-xl bg-slate-50 flex items-center justify-center text-slate-400 group-hover:bg-violet-50 group-hover:text-violet-600 transition-colors">
          <component :is="getActionIcon(log.action)" :size="24" />
        </div>
        
        <div class="flex-1 grid grid-cols-1 md:grid-cols-4 gap-6">
          <div class="md:col-span-2 space-y-1">
            <div class="flex items-center gap-2">
              <h3 class="text-sm font-bold text-slate-900">{{ log.action }}</h3>
              <span class="text-[10px] font-mono text-slate-400 uppercase">{{ log.id }}</span>
            </div>
            <p class="text-xs text-slate-500 leading-relaxed">{{ log.details }}</p>
          </div>

          <div class="space-y-1">
            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Actor</p>
            <div class="flex items-center gap-2">
              <component :is="log.type === 'agent' ? Bot : User" :size="12" class="text-slate-400" />
              <span class="text-xs font-medium text-slate-700">{{ log.actor }}</span>
            </div>
          </div>

          <div class="space-y-1 text-right">
            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Timestamp</p>
            <div class="flex items-center justify-end gap-2 text-xs text-slate-600">
              <Clock :size="12" />
              {{ log.time }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
