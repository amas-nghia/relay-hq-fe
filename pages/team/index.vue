<script setup lang="ts">
import { 
  Users, 
  Search, 
  Plus, 
  Mail, 
  Shield, 
  MoreVertical,
  ChevronRight,
  Clock
} from 'lucide-vue-next'

const team = [
  { id: 'user-001', name: 'Alice PM', role: 'Project Manager', email: 'alice@acme.com', status: 'active', projects: 3, lastActive: '2m ago', avatar: 'A' },
  { id: 'user-002', name: 'Bob Architect', role: 'System Architect', email: 'bob@acme.com', status: 'active', projects: 2, lastActive: '1h ago', avatar: 'B' },
  { id: 'user-003', name: 'Charlie Dev', role: 'Senior Developer', email: 'charlie@acme.com', status: 'away', projects: 1, lastActive: '4h ago', avatar: 'C' },
  { id: 'user-004', name: 'Diana Ops', role: 'Site Reliability', email: 'diana@acme.com', status: 'offline', projects: 2, lastActive: '1d ago', avatar: 'D' },
]
</script>

<template>
  <div class="space-y-8">
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-6">
      <div class="space-y-2">
        <p class="text-[10px] uppercase tracking-[0.2em] text-violet-600 font-bold">Management</p>
        <h1 class="text-4xl font-bold tracking-tight text-slate-950">Team Directory</h1>
        <p class="text-slate-500 text-sm">Manage human team members and their access levels within the workspace.</p>
      </div>
      
      <div class="flex items-center gap-3">
        <button class="flex items-center gap-2 px-4 py-2 bg-violet-600 text-white rounded-lg text-xs font-bold hover:bg-violet-700 transition-colors shadow-lg shadow-violet-200">
          <Plus :size="16" />
          Invite Member
        </button>
      </div>
    </header>

    <div class="glass-card overflow-hidden">
      <table class="w-full text-left border-collapse">
        <thead>
          <tr class="border-b border-slate-100 bg-slate-50/50">
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Member</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Role</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Projects</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Status</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest">Last Active</th>
            <th class="px-6 py-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest text-right">Actions</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-slate-50">
          <tr v-for="member in team" :key="member.id" class="group hover:bg-slate-50/50 transition-colors">
            <td class="px-6 py-4">
              <div class="flex items-center gap-4">
                <div class="w-10 h-10 rounded-full bg-violet-100 text-violet-600 flex items-center justify-center font-bold text-sm">
                  {{ member.avatar }}
                </div>
                <div>
                  <h3 class="text-sm font-bold text-slate-900">{{ member.name }}</h3>
                  <div class="flex items-center gap-1.5 text-[10px] text-slate-400">
                    <Mail :size="10" />
                    {{ member.email }}
                  </div>
                </div>
              </div>
            </td>
            <td class="px-6 py-4">
              <div class="flex items-center gap-2">
                <Shield :size="14" class="text-slate-400" />
                <span class="text-xs font-medium text-slate-600">{{ member.role }}</span>
              </div>
            </td>
            <td class="px-6 py-4">
              <span class="text-xs font-bold text-slate-700">{{ member.projects }} Projects</span>
            </td>
            <td class="px-6 py-4">
              <div class="flex items-center gap-2">
                <div class="w-2 h-2 rounded-full" :class="member.status === 'active' ? 'bg-emerald-500' : member.status === 'away' ? 'bg-amber-500' : 'bg-slate-300'"></div>
                <span class="text-[10px] font-bold text-slate-500 uppercase tracking-wider">{{ member.status }}</span>
              </div>
            </td>
            <td class="px-6 py-4">
              <div class="flex items-center gap-1.5 text-xs text-slate-500">
                <Clock :size="12" />
                {{ member.lastActive }}
              </div>
            </td>
            <td class="px-6 py-4 text-right">
              <button class="p-2 text-slate-300 hover:text-slate-600 transition-colors">
                <MoreVertical :size="16" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
