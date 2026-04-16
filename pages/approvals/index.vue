<script setup lang="ts">
import { 
  ShieldCheck, 
  ShieldAlert,
  Clock,
  CheckCircle2,
  XCircle,
  ChevronRight,
  User,
  Bot
} from 'lucide-vue-next'

const approvals = [
  { id: 'app-001', task: 'Deploy Auth Service', taskId: 'task-007', requester: 'agent-dev', type: 'agent', reason: 'Needs production vault access to write environment variables.', status: 'pending', time: '2h ago' },
  { id: 'app-002', task: 'Delete Legacy Data', taskId: 'task-012', requester: 'agent-db', type: 'agent', reason: 'Cleanup of deprecated user tables in production database.', status: 'pending', time: '5h ago' },
  { id: 'app-003', task: 'Infrastructure Scale Up', taskId: 'task-015', requester: 'human-bob', type: 'human', reason: 'Scaling k8s cluster for expected traffic spike.', status: 'approved', time: '1d ago' },
]
</script>

<template>
  <div class="space-y-8">
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Governance</p>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">Approvals</h1>
        <p class="text-slate-500 text-sm">Review and manage high-stakes action requests from agents and team members.</p>
      </div>
    </header>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
      <div class="lg:col-span-2 space-y-6">
        <h2 class="text-xl font-bold tracking-tight flex items-center gap-2">
          Pending Review
          <span class="bg-amber-100 text-amber-700 text-[10px] font-bold px-2 py-0.5 rounded-full">2 ACTION REQUIRED</span>
        </h2>
        
        <div class="space-y-4">
          <div v-for="app in approvals.filter(a => a.status === 'pending')" :key="app.id" class="glass-card p-6 space-y-6 border-l-4 border-l-amber-500">
            <div class="flex items-start justify-between">
              <div class="space-y-1">
                <div class="flex items-center gap-2">
                  <span class="text-[10px] font-mono text-slate-400 uppercase">{{ app.id }}</span>
                  <span class="text-[10px] font-bold text-violet-600 bg-violet-50 px-1.5 py-0.5 rounded uppercase tracking-wider">{{ app.taskId }}</span>
                </div>
                <h3 class="text-lg font-bold text-slate-900">{{ app.task }}</h3>
              </div>
              <div class="text-right">
                <div class="flex items-center justify-end gap-1.5 text-xs text-slate-500 mb-1">
                  <Clock :size="12" />
                  {{ app.time }}
                </div>
                <span class="status-pill bg-amber-50 text-amber-700 border-amber-200">Pending</span>
              </div>
            </div>

            <div class="p-4 bg-slate-50 rounded-xl space-y-2">
              <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Reason for request</p>
              <p class="text-sm text-slate-700 leading-relaxed">{{ app.reason }}</p>
            </div>

            <div class="flex items-center justify-between pt-4 border-t border-slate-100">
              <div class="flex items-center gap-3">
                <div class="w-8 h-8 rounded-full bg-slate-100 flex items-center justify-center text-slate-400 border border-slate-200">
                  <component :is="app.type === 'agent' ? Bot : User" :size="16" />
                </div>
                <div>
                  <p class="text-[10px] font-bold text-slate-400 uppercase">Requester</p>
                  <p class="text-xs font-bold text-slate-700">{{ app.requester }}</p>
                </div>
              </div>
              <div class="flex items-center gap-2">
                <button class="px-4 py-2 bg-white border border-slate-200 text-slate-600 rounded-lg text-xs font-bold hover:bg-slate-50 transition-colors">
                  Reject
                </button>
                <button class="px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors shadow-lg shadow-violet-200">
                  Approve
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="space-y-6">
        <h2 class="text-xl font-bold tracking-tight">Recent History</h2>
        <div class="glass-card divide-y divide-slate-50">
          <div v-for="app in approvals.filter(a => a.status !== 'pending')" :key="app.id" class="p-4 flex items-center justify-between group hover:bg-slate-50 transition-colors">
            <div class="space-y-1">
              <h4 class="text-xs font-bold text-slate-900">{{ app.task }}</h4>
              <p class="text-[10px] text-slate-400">Approved by Alice PM • {{ app.time }}</p>
            </div>
            <div class="w-8 h-8 rounded-full bg-emerald-50 text-emerald-600 flex items-center justify-center">
              <ShieldCheck :size="16" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
