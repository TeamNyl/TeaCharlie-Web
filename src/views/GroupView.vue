<template>
  <div class="team-view">
    <h1 class="title">TeamNil</h1>
    <p class="subtitle">茶屿开发团队</p>
    <h2>介绍</h2>
    <div class="team-description">
      <p>由一群热爱技术的人们</p>
    </div>
    <h2>成员</h2>
    <div class="team-container">
      <GroupMemberCard
        v-for="member in members"
        :key="member.Name"
        :member="member"
        @showBigCard="showBigCard"
      />
    </div>
    <div v-if="selectedMember" class="big-card-container" @click.self="selectedMember = null">
      <GroupMemberCardBig :member="selectedMember" @close="selectedMember = null" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import GroupMemberCard from '@/components/GroupMemberCard.vue'
import GroupMemberCardBig from '@/components/GroupMemberCardBig.vue'

interface Member {
  Name: string
  Title: string
  Avatar: string
  Bio: string
  Link?: string
  Desc: string
}

const members = ref<Member[]>([])
const selectedMember = ref<Member | null>(null)

const showBigCard = (member: Member) => {
  selectedMember.value = member
}

onMounted(async () => {
  try {
    const response = await fetch('/group-member.json')
    members.value = await response.json()
  } catch (error) {
    console.error('Failed to load team members:', error)
  }
})
</script>

<style scoped>
.big-card-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 2rem;
}
</style>

<style scoped>
.team-view {
  min-height: 100vh;
  background-color: #c1eae6;
  padding: 2rem 4rem;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
}

.team-view .title {
  font-size: 3rem;
  color: #7c471d;
  margin-bottom: 0.5rem;
}

.team-view h2 {
  /* font-size: 2rem;
  color: #7c471d;
  margin-bottom: 0.5rem; */
  width: 100%;
  font-size: 2rem;
  color: #7c471d;
  border-bottom: 0.3rem ridge #7c471d;
  padding-bottom: 0.5rem;
  margin-bottom: 1rem;
}

.team-view .subtitle {
  font-size: 1rem;
  color: #876345;
  margin-bottom: 1rem;
}

.team-description {
  font-size: 1.2rem;
  color: #7c471d;
  margin-bottom: 2rem;
  max-width: 800px;
  text-align: center;
  line-height: 1.6;
}

.team-container {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
  padding: 1rem;
  padding-bottom: 4rem;
}
</style>
