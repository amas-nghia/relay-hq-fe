<script setup lang="ts">
import { 
  ChevronLeft, 
  ShieldAlert, 
  CheckCircle2, 
  XCircle, 
  RotateCcw,
  Clock,
  User,
  FileText,
  Lock,
  History,
  MessageSquare,
  Send,
  Bot
} from 'lucide-vue-next'

const route = useRoute()
const taskId = route.params.id as string

const activeTab = ref('details')

const task = {
  id: taskId,
  title: 'Deploy Auth Service',
  status: 'waiting-approval',
  priority: 'high',
  assignee: 'agent-backend',
  progress: 60,
  description: 'Deploying the authentication service to production environment. Requires elevated access to production vault secrets.',
  vaultPath: 'vault/shared/tasks/task-007.md',
  workspace: 'Acme Workspace',
  project: 'Auth Project',
  board: 'Auth Board',
  column: 'In Progress',
  tags: ['auth', 'api', 'prod'],
  approval: {
    needed: true,
    requestedBy: 'agent-backend',
    reason: 'Needs production access to write environment variables to the vault.',
    status: 'pending'
  },
  timeline: [
    { status: 'todo', time: 'Oct 24, 10:00 AM', note: 'Task created by Alice PM' },
    { status: 'in-progress', time: 'Oct 24, 10:15 AM', note: 'Assigned to agent-backend' },
    { status: 'waiting-approval', time: 'Oct 24, 11:30 AM', note: 'Agent requested production access' },
  ],
  chat: [
    { sender: 'agent-backend', type: 'agent', message: 'I have completed the staging deployment. Ready for production but I need the vault secrets.', time: '11:25 AM' },
    { sender: 'Alice PM', type: 'human', message: 'Can you confirm if the health checks passed in staging?', time: '11:28 AM' },
    { sender: 'agent-backend', type: 'agent', message: 'Yes, all 12 health checks passed. Logs are clean.', time: '11:30 AM' },
  ]
}
</script>

<template>
  <div class="max-w-5xl mx-auto space-y-8">
    <!-- Back Button -->
    <NuxtLink to="/boards/auth-board" class="inline-flex items-center gap-2 text-xs font-bold text-slate-400 hover:text-violet-600 transition-colors uppercase tracking-widest">
      <ChevronLeft :size="14" />
      Back to Board
    </NuxtLink>

    <!-- Header -->
    <header class="flex flex-col md:flex-row md:items-start justify-between gap-6">
      <div class="space-y-4">
        <div class="flex items-center gap-3">
          <span class="text-[10px] font-mono font-bold px-2 py-1 bg-slate-100 text-slate-500 rounded uppercase">{{ task.id }}</span>
          <div class="flex items-center gap-2">
            <span class="status-pill bg-amber-50 text-amber-700 border-amber-200">{{ task.status }}</span>
            <span class="status-pill bg-violet-50 text-violet-700 border-violet-200">{{ task.priority }}</span>
          </div>
        </div>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">{{ task.title }}</h1>
        <div class="flex flex-wrap items-center gap-6 text-sm text-slate-500">
          <div class="flex items-center gap-2">
            <User :size="16" class="text-slate-400" />
            <span class="font-medium">{{ task.assignee }}</span>
          </div>
          <div class="flex items-center gap-2">
            <Clock :size="16" class="text-slate-400" />
            <span class="font-medium">Updated 12m ago</span>
          </div>
        </div>
      </div>

      <div class="flex items-center gap-3">
        <button class="px-4 py-2 bg-white border border-slate-200 rounded-lg text-xs font-bold text-slate-600 hover:bg-slate-50 transition-colors">
          Edit Task
        </button>
        <button class="px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors shadow-lg shadow-violet-200">
          Reassign
        </button>
      </div>
    </header>

    <!-- Tabs -->
    <div class="flex items-center gap-8 border-b border-slate-200">
      <button 
        v-for="tab in ['details', 'coordination', 'audit']" 
        :key="tab"
        @click="activeTab = tab"
        class="pb-4 text-xs font-bold uppercase tracking-widest transition-all relative"
        :class="activeTab === tab ? 'text-violet-600' : 'text-slate-400 hover:text-slate-600'"
      >
        {{ tab }}
        <div v-if="activeTab === tab" class="absolute bottom-0 left-0 right-0 h-0.5 bg-violet-600"></div>
      </button>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
      <!-- Main Content -->
      <div class="lg:col-span-2 space-y-8">
        
        <!-- Details Tab -->
        <div v-if="activeTab === 'details'" class="space-y-8">
          <section class="glass-card p-8 space-y-6">
            <div class="flex items-center gap-2 text-xs font-bold text-slate-400 uppercase tracking-widest">
              <FileText :size="14" />
              Description
            </div>
            <p class="text-slate-600 leading-relaxed">{{ task.description }}</p>
            
            <div class="pt-6 border-t border-slate-100 grid grid-cols-2 gap-8">
              <div>
                <p class="text-[10px] font-bold text-slate-400 uppercase mb-2">Vault Path</p>
                <p class="text-xs font-mono text-slate-600 bg-slate-50 p-2 rounded border border-slate-100">{{ task.vaultPath }}</p>
              </div>
              <div>
                <p class="text-[10px] font-bold text-slate-400 uppercase mb-2">Tags</p>
                <div class="flex flex-wrap gap-2">
                  <span v-for="tag in task.tags" :key="tag" class="px-2 py-0.5 bg-slate-100 text-slate-600 rounded text-[10px] font-bold uppercase">#{{ tag }}</span>
                </div>
              </div>
            </div>
          </section>
        </div>

        <!-- Coordination Tab -->
        <div v-if="activeTab === 'coordination'" class="glass-card flex flex-col h-[600px]">
          <div class="p-4 border-b border-slate-100 flex items-center justify-between">
            <div class="flex items-center gap-2 text-xs font-bold text-slate-400 uppercase tracking-widest">
              <MessageSquare :size="14" />
              Coordination Chat
            </div>
            <span class="text-[10px] text-slate-400">Context: task-007</span>
          </div>
          
          <div class="flex-1 overflow-y-auto p-6 space-y-6">
            <div v-for="(msg, i) in task.chat" :key="i" class="flex flex-col" :class="msg.type === 'human' ? 'items-end' : 'items-start'">
              <div class="flex items-center gap-2 mb-1">
                <span v-if="msg.type === 'agent'" class="text-[10px] font-bold text-violet-600 uppercase tracking-wider flex items-center gap-1">
                  <Bot :size="10" /> {{ msg.sender }}
                </span>
                <span v-else class="text-[10px] font-bold text-slate-500 uppercase tracking-wider">{{ msg.sender }}</span>
                <span class="text-[9px] text-slate-400">{{ msg.time }}</span>
              </div>
              <div 
                class="max-w-[80%] p-3 rounded-2xl text-sm leading-relaxed shadow-sm"
                :class="msg.type === 'human' ? 'bg-violet-600 text-white rounded-tr-none' : 'bg-slate-100 text-slate-700 rounded-tl-none'"
              >
                {{ msg.message }}
              </div>
            </div>
          </div>

          <div class="p-4 border-t border-slate-100">
            <div class="relative">
              <input type="text" placeholder="Message agent..." class="w-full pl-4 pr-12 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-violet-500/20 focus:border-violet-500 transition-all" />
              <button class="absolute right-2 top-1/2 -translate-y-1/2 p-2 bg-violet-600 text-white rounded-lg hover:bg-violet-700 transition-colors">
                <Send :size="16" />
              </button>
            </div>
          </div>
        </div>

        <!-- Audit Tab -->
        <div v-if="activeTab === 'audit'" class="space-y-6">
          <div class="flex items-center gap-2 text-xs font-bold text-slate-400 uppercase tracking-widest">
            <History :size="14" />
            Audit Trail
          </div>
          <div class="space-y-4">
            <div v-for="(item, index) in task.timeline" :key="index" class="glass-card p-4 flex gap-4">
              <div class="w-10 h-10 rounded-lg bg-slate-50 flex items-center justify-center text-slate-400 shrink-0">
                <History :size="20" />
              </div>
              <div class="flex-1">
                <div class="flex items-center justify-between">
                  <span class="text-xs font-bold text-slate-900 uppercase">{{ item.status }}</span>
                  <span class="text-[10px] text-slate-400">{{ item.time }}</span>
                </div>
                <p class="text-sm text-slate-600 mt-1">{{ item.note }}</p>
                <div class="mt-3 pt-3 border-t border-slate-50 flex items-center gap-4">
                  <div class="flex items-center gap-1.5 text-[9px] font-bold text-slate-400 uppercase">
                    <User :size="10" /> Actor: {{ index === 0 ? 'Alice PM' : 'agent-backend' }}
                  </div>
                  <div class="flex items-center gap-1.5 text-[9px] font-bold text-slate-400 uppercase">
                    <Lock :size="10" /> Signed by Vault
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Sidebar Info -->
      <aside class="space-y-8">
        <!-- Approval Panel -->
        <section v-if="task.approval.needed" class="bg-amber-50 border border-amber-200 rounded-2xl p-6 space-y-6">
          <div class="flex items-center gap-2 text-amber-700">
            <ShieldAlert :size="20" />
            <h2 class="text-sm font-bold uppercase tracking-widest">Approval Pending</h2>
          </div>
          
          <div class="space-y-4">
            <div>
              <p class="text-[10px] font-bold text-amber-600 uppercase mb-1">Requested By</p>
              <p class="text-sm font-bold text-amber-900">{{ task.approval.requestedBy }}</p>
            </div>
            <div>
              <p class="text-[10px] font-bold text-amber-600 uppercase mb-1">Reason</p>
              <p class="text-xs text-amber-800 leading-relaxed">{{ task.approval.reason }}</p>
            </div>
          </div>

          <div class="grid grid-cols-1 gap-2 pt-4">
            <button class="w-full flex items-center justify-center gap-2 px-4 py-2.5 bg-emerald-600 text-white rounded-xl text-xs font-bold hover:bg-emerald-700 transition-colors shadow-lg shadow-emerald-200">
              <CheckCircle2 :size="16" />
              Approve Request
            </button>
            <button class="w-full flex items-center justify-center gap-2 px-4 py-2.5 bg-white border border-red-200 text-red-600 rounded-xl text-xs font-bold hover:bg-red-50 transition-colors">
              <XCircle :size="16" />
              Reject
            </button>
            <button class="w-full flex items-center justify-center gap-2 px-4 py-2.5 bg-white border border-amber-200 text-amber-700 rounded-xl text-xs font-bold hover:bg-amber-100/50 transition-colors">
              <RotateCcw :size="16" />
              Request Changes
            </button>
          </div>
        </section>

        <!-- Metadata Card -->
        <section class="glass-card p-6 space-y-6">
          <div class="space-y-4">
            <div class="flex items-center justify-between">
              <span class="text-[10px] font-bold text-slate-400 uppercase">Workspace</span>
              <span class="text-xs font-bold text-slate-700">{{ task.workspace }}</span>
            </div>
            <div class="flex items-center justify-between">
              <span class="text-[10px] font-bold text-slate-400 uppercase">Project</span>
              <span class="text-xs font-bold text-slate-700">{{ task.project }}</span>
            </div>
            <div class="flex items-center justify-between">
              <span class="text-[10px] font-bold text-slate-400 uppercase">Board</span>
              <span class="text-xs font-bold text-slate-700">{{ task.board }}</span>
            </div>
            <div class="flex items-center justify-between">
              <span class="text-[10px] font-bold text-slate-400 uppercase">Column</span>
              <span class="text-xs font-bold text-slate-700">{{ task.column }}</span>
            </div>
          </div>
          
          <div class="pt-6 border-t border-slate-100">
            <div class="flex items-center justify-between mb-3">
              <span class="text-[10px] font-bold text-slate-400 uppercase">Coordination Lock</span>
              <Lock :size="14" class="text-slate-400" />
            </div>
            <div class="p-3 bg-slate-50 rounded-xl border border-slate-100 flex items-center justify-between">
              <span class="text-xs font-bold text-slate-600">Unlocked</span>
              <div class="w-2 h-2 rounded-full bg-emerald-500"></div>
            </div>
          </div>
        </section>
      </aside>
    </div>
  </div>
</template>
