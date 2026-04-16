<script setup lang="ts">
import { 
  Search, 
  Plus, 
  Filter, 
  MoreHorizontal, 
  User, 
  Bot,
  Clock,
  ShieldAlert,
  ChevronRight
} from 'lucide-vue-next'

const tasks = [
  { id: 'task-001', title: 'Implement Password Reset API', status: 'todo', priority: 'high', assignee: 'agent-dev', type: 'agent', progress: 0, project: 'Auth Project', updated: '2h ago' },
  { id: 'task-005', title: 'Deploy Auth Service', status: 'in-progress', priority: 'high', assignee: 'agent-dev', type: 'agent', progress: 60, project: 'Auth Project', updated: '12m ago' },
  { id: 'task-006', title: 'Database Schema Update', status: 'in-progress', priority: 'critical', assignee: 'agent-db', type: 'agent', progress: 20, project: 'Infra Project', updated: '4h ago' },
  { id: 'task-007', title: 'Security Audit Logs', status: 'in-progress', priority: 'medium', assignee: 'human-alice', type: 'human', progress: 45, project: 'Auth Project', updated: '1h ago' },
  { id: 'task-009', title: 'PR Review: Auth Module', status: 'review', priority: 'medium', assignee: 'human-alice', type: 'human', progress: 90, project: 'Auth Project', updated: '30m ago' },
  { id: 'task-012', title: 'Delete Legacy Data', status: 'review', priority: 'critical', assignee: 'agent-db', type: 'agent', progress: 0, approvalNeeded: true, project: 'Infra Project', updated: '5h ago' },
]

const getStatusColor = (status: string) => {
  switch (status) {
    case 'todo': return 'bg-slate-100 text-slate-600'
    case 'in-progress': return 'bg-blue-50 text-blue-700'
    case 'review': return 'bg-amber-50 text-amber-700'
    case 'done': return 'bg-emerald-50 text-emerald-700'
    default: return 'bg-slate-100 text-slate-600'
  }
}
</script>

<template>
  <div class="space-y-8">
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Workspace</p>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">All Tasks</h1>
        <p class="text-slate-500 text-sm">Comprehensive view of all work items across the coordination boundary.</p>
      </div>
      
      <div class="flex items-center gap-3">
        <div class="relative">
          <Search class="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" :size="14" />
          <input type="text" placeholder="Search tasks..." class="pl-9 pr-4 py-2 bg-white border border-slate-200 rounded-lg text-xs focus:outline-none focus:ring-2 focus:ring-violet-500/20 focus:border-violet-500 transition-all w-64" />
        </div>
        <button class="flex items-center gap-2 px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors shadow-lg shadow-violet-200">
          <Plus :size="16" />
          New Task
        </button>
      </div>
    </header>

    <div class="glass-card overflow-hidden">
      <table class="w-full text-left border-collapse">
        <thead>
          <tr class="border-b border-slate-100 bg-slate-50/50">
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Task</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Project</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Assignee</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Status</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Progress</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest text-right">Actions</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-slate-50">
          <tr v-for="task in tasks" :key="task.id" class="group hover:bg-slate-50/50 transition-colors">
            <td class="px-6 py-4">
              <NuxtLink :to="`/tasks/${task.id}`" class="block space-y-1">
                <div class="flex items-center gap-2">
                  <span class="text-sm font-bold text-slate-900 group-hover:text-violet-600 transition-colors">{{ task.title }}</span>
                  <span v-if="task.approvalNeeded" class="text-amber-600"><ShieldAlert :size="14" /></span>
                </div>
                <span class="text-[10px] font-mono text-slate-400 uppercase">{{ task.id }}</span>
              </NuxtLink>
            </td>
            <td class="px-6 py-4">
              <span class="text-xs font-medium text-slate-600">{{ task.project }}</span>
            </td>
            <td class="px-6 py-4">
              <div class="flex items-center gap-2">
                <div class="w-6 h-6 rounded-full bg-slate-100 flex items-center justify-center text-slate-400 border border-slate-200">
                  <component :is="task.type === 'agent' ? Bot : User" :size="12" />
                </div>
                <span class="text-[10px] font-bold text-slate-500 uppercase">{{ task.assignee }}</span>
              </div>
            </td>
            <td class="px-6 py-4">
              <span class="status-pill" :class="getStatusColor(task.status)">{{ task.status }}</span>
            </td>
            <td class="px-6 py-4">
              <div class="flex items-center gap-3">
                <div class="flex-1 h-1.5 bg-slate-100 rounded-full overflow-hidden min-w-[60px]">
                  <div class="h-full bg-violet-500 rounded-full" :style="{ width: `${task.progress}%` }"></div>
                </div>
                <span class="text-[10px] font-bold text-slate-400">{{ task.progress }}%</span>
              </div>
            </td>
            <td class="px-6 py-4 text-right">
              <button class="p-2 text-slate-300 hover:text-violet-600 transition-colors">
                <MoreHorizontal :size="16" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
