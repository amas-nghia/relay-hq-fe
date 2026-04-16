<script setup lang="ts">
import { 
  MoreHorizontal, 
  Plus, 
  User, 
  Clock, 
  ShieldAlert,
  ChevronRight,
  Filter,
  Search
} from 'lucide-vue-next'

const viewMode = ref('kanban')

const columns = [
  { id: 'todo', title: 'Todo', count: 3 },
  { id: 'in-progress', title: 'In Progress', count: 4 },
  { id: 'review', title: 'Review', count: 3 },
  { id: 'done', title: 'Done', count: 2 },
]

const tasks = [
  { id: 'task-001', title: 'Implement Password Reset API', status: 'todo', priority: 'high', assignee: 'agent-dev', progress: 0, project: 'Auth Project' },
  { id: 'task-005', title: 'Deploy Auth Service', status: 'in-progress', priority: 'high', assignee: 'agent-dev', progress: 60, stale: false, project: 'Auth Project' },
  { id: 'task-006', title: 'Database Schema Update', status: 'in-progress', priority: 'critical', assignee: 'agent-db', progress: 20, stale: true, project: 'Infra Project' },
  { id: 'task-007', title: 'Security Audit Logs', status: 'in-progress', priority: 'medium', assignee: 'human-alice', progress: 45, project: 'Auth Project' },
  { id: 'task-009', title: 'PR Review: Auth Module', status: 'review', priority: 'medium', assignee: 'human-alice', progress: 90, project: 'Auth Project' },
  { id: 'task-012', title: 'Delete Legacy Data', status: 'review', priority: 'critical', assignee: 'agent-db', progress: 0, approvalNeeded: true, project: 'Infra Project' },
]

const plans = [
  { 
    id: 'plan-001', 
    title: 'Authentication Infrastructure', 
    children: [
      { id: 'task-001', title: 'Implement Password Reset API', status: 'todo' },
      { id: 'task-005', title: 'Deploy Auth Service', status: 'in-progress' },
      { id: 'task-009', title: 'PR Review: Auth Module', status: 'review' },
    ]
  },
  {
    id: 'plan-002',
    title: 'Database & Infrastructure',
    children: [
      { id: 'task-006', title: 'Database Schema Update', status: 'in-progress' },
      { id: 'task-012', title: 'Delete Legacy Data', status: 'review' },
    ]
  }
]

const getTasksByColumn = (colId: string) => tasks.filter(t => {
  if (colId === 'review' && t.approvalNeeded) return true
  return t.status === colId
})

const getPriorityColor = (priority: string) => {
  switch (priority) {
    case 'critical': return 'border-l-red-600'
    case 'high': return 'border-l-amber-500'
    case 'medium': return 'border-l-blue-500'
    default: return 'border-l-slate-300'
  }
}
</script>

<template>
  <div class="space-y-8">
    <!-- Board Header -->
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <div class="flex items-center gap-2 text-[10px] font-bold text-slate-400 uppercase tracking-widest">
          <span>Projects</span>
          <ChevronRight :size="10" />
          <span class="text-violet-600">Auth Project</span>
        </div>
        <h1 class="text-3xl font-bold tracking-tight text-slate-950">Auth Board</h1>
        <p class="text-slate-500 text-sm">Managing authentication and authorization workflow gates.</p>
      </div>
      
      <div class="flex items-center gap-3">
        <!-- View Toggle -->
        <div class="bg-slate-100 p-1 rounded-lg flex items-center gap-1 mr-4">
          <button 
            @click="viewMode = 'kanban'"
            class="px-3 py-1.5 text-[10px] font-bold uppercase tracking-wider rounded-md transition-all"
            :class="viewMode === 'kanban' ? 'bg-white text-violet-600 shadow-sm' : 'text-slate-500 hover:text-slate-700'"
          >
            Kanban
          </button>
          <button 
            @click="viewMode = 'plan'"
            class="px-3 py-1.5 text-[10px] font-bold uppercase tracking-wider rounded-md transition-all"
            :class="viewMode === 'plan' ? 'bg-white text-violet-600 shadow-sm' : 'text-slate-500 hover:text-slate-700'"
          >
            Plan View
          </button>
        </div>

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

    <!-- Kanban Grid -->
    <div v-if="viewMode === 'kanban'" class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 gap-6 items-start">
      <div v-for="col in columns" :key="col.id" class="space-y-4">
        <div class="flex items-center justify-between px-2">
          <div class="flex items-center gap-2">
            <h2 class="text-xs font-bold uppercase tracking-widest text-slate-500">{{ col.title }}</h2>
            <span class="bg-slate-200 text-slate-600 text-[10px] font-bold px-1.5 py-0.5 rounded-full">{{ getTasksByColumn(col.id).length }}</span>
          </div>
          <button class="text-slate-400 hover:text-slate-600">
            <MoreHorizontal :size="16" />
          </button>
        </div>

        <div class="space-y-3 min-h-[500px]">
          <NuxtLink 
            v-for="task in getTasksByColumn(col.id)" 
            :key="task.id"
            :to="`/tasks/${task.id}`"
            class="glass-card p-4 block border-l-4 group hover:translate-y-[-2px] transition-all"
            :class="[getPriorityColor(task.priority)]"
          >
            <div class="space-y-3">
              <div class="flex items-center justify-between">
                <span class="text-[9px] font-bold text-violet-600 bg-violet-50 px-1.5 py-0.5 rounded uppercase tracking-wider">{{ task.project }}</span>
                <span class="text-[10px] font-mono text-slate-400 shrink-0">{{ task.id }}</span>
              </div>
              
              <h3 class="text-sm font-bold text-slate-900 leading-tight group-hover:text-violet-600 transition-colors">{{ task.title }}</h3>

              <div v-if="task.stale || task.approvalNeeded" class="flex flex-wrap gap-2">
                <div v-if="task.stale" class="flex items-center gap-1.5 px-2 py-0.5 bg-red-50 text-red-600 rounded text-[10px] font-bold border border-red-100">
                  <Clock :size="10" />
                  STALE
                </div>
                <div v-if="task.approvalNeeded" class="flex items-center gap-1.5 px-2 py-0.5 bg-amber-50 text-amber-600 rounded text-[10px] font-bold border border-amber-100">
                  <ShieldAlert :size="10" />
                  APPROVAL REQUIRED
                </div>
              </div>

              <div class="flex items-center justify-between pt-2">
                <div class="flex items-center gap-2">
                  <div class="w-6 h-6 rounded-full bg-slate-100 flex items-center justify-center text-slate-400 border border-slate-200">
                    <User :size="12" />
                  </div>
                  <span class="text-[10px] font-bold text-slate-500 uppercase">{{ task.assignee }}</span>
                </div>
                <div class="text-right">
                  <div class="text-[10px] font-bold text-slate-400 uppercase mb-1">{{ task.progress }}%</div>
                  <div class="w-16 h-1 bg-slate-100 rounded-full overflow-hidden">
                    <div class="h-full bg-violet-500 rounded-full" :style="{ width: `${task.progress}%` }"></div>
                  </div>
                </div>
              </div>
            </div>
          </NuxtLink>
        </div>
      </div>
    </div>

    <!-- Plan View -->
    <div v-else class="space-y-6">
      <div v-for="plan in plans" :key="plan.id" class="glass-card overflow-hidden">
        <div class="p-4 bg-slate-50/50 border-b border-slate-100 flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 rounded-lg bg-violet-100 text-violet-600 flex items-center justify-center">
              <FolderKanban :size="18" />
            </div>
            <h2 class="text-sm font-bold text-slate-900">{{ plan.title }}</h2>
          </div>
          <span class="text-[10px] font-mono text-slate-400 uppercase">{{ plan.id }}</span>
        </div>
        <div class="divide-y divide-slate-50">
          <NuxtLink 
            v-for="task in plan.children" 
            :key="task.id"
            :to="`/tasks/${task.id}`"
            class="p-4 flex items-center justify-between hover:bg-slate-50 transition-colors group"
          >
            <div class="flex items-center gap-4">
              <div class="w-2 h-2 rounded-full" :class="task.status === 'done' ? 'bg-emerald-500' : task.status === 'in-progress' ? 'bg-blue-500' : 'bg-slate-300'"></div>
              <span class="text-sm font-medium text-slate-700 group-hover:text-violet-600 transition-colors">{{ task.title }}</span>
            </div>
            <div class="flex items-center gap-4">
              <span class="status-pill bg-slate-50 text-slate-500 border-slate-200">{{ task.status }}</span>
              <ChevronRight :size="14" class="text-slate-300 group-hover:text-violet-400" />
            </div>
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>
