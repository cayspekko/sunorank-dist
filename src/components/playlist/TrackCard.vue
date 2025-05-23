<template>
  <n-card 
    class="track-card"
    :bordered="false"
  >
    <n-grid :x-gap="12" :cols="12">
      <n-gi :span="1">
        <div class="rank-number">{{ track.rank }}</div>
      </n-gi>
      <n-gi :span="3">
        <div class="track-image">
          <img v-if="track.imageUrl" :src="track.imageUrl" alt="Track cover" />
          <div v-else class="no-image">
            <n-icon><music-icon /></n-icon>
          </div>
        </div>
      </n-gi>
      <n-gi :span="6">
        <TrackInfo :track="track">
          <template #rating-info>
            <div v-if="track.voteCount !== undefined" class="rating-info">
              <template v-if="rankingMethod === 'star'">
                <n-rate 
                  readonly 
                  :value="track.averageRating || 0" 
                  :allow-half="true"
                />
                <span>({{ track.averageRating?.toFixed(1) || '0.0' }}) - {{ track.voteCount }} votes</span>
              </template>
              <template v-else-if="rankingMethod === 'updown'">
                <span>Score: {{ track.averageRating?.toFixed(0) || '0' }} ({{ track.voteCount }} votes)</span>
              </template>
              <template v-else-if="rankingMethod === 'ranked'">
                <span>Score: {{ track.averageRating?.toFixed(0) || '0' }}</span>
                <div class="rank-distribution">
                  <n-tag :color="{ color: '#D4AF37', textColor: '#000' }" size="small" round>
                    <template #icon><n-icon><trophy /></n-icon></template>
                    {{ track.votesByRank?.[1] || 0 }}
                  </n-tag>
                  <n-tag :color="{ color: '#A9A9B0', textColor: '#000' }" size="small" round>
                    <template #icon><n-icon><trophy /></n-icon></template>
                    {{ track.votesByRank?.[2] || 0 }}
                  </n-tag>
                  <n-tag :color="{ color: '#A97142', textColor: '#000' }" size="small" round>
                    <template #icon><n-icon><trophy /></n-icon></template>
                    {{ track.votesByRank?.[3] || 0 }}
                  </n-tag>
                </div>
              </template>
              <template v-else>
                <span>{{ track.voteCount }} favorites</span>
              </template>
            </div>
          </template>
        </TrackInfo>
      </n-gi>
      <n-gi :span="2">
        <div class="track-actions">
          <slot name="actions"></slot>
        </div>
      </n-gi>
    </n-grid>
  </n-card>
</template>

<script setup lang="ts">
import { MusicalNoteOutline as MusicIcon } from '@vicons/ionicons5'
import { TrophyOutline as Trophy, MedalOutline as Medal, PodiumOutline as Podium } from '@vicons/ionicons5'
import { TrackWithRanking, RankingMethod } from '../../types/playlist'
import TrackInfo from './TrackInfo.vue'

defineProps<{
  track: TrackWithRanking,
  rankingMethod: RankingMethod
}>()
</script>

<style scoped>
.track-card {
  transition: all 0.2s ease;
}

.track-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.track-image {
  width: 80px;
  height: 80px;
  border-radius: 4px;
  overflow: hidden;
}

.track-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.no-image {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--n-card-color);
  border: 1px solid var(--n-border-color);
}

/* Ensure proper vertical alignment with the Track Info component */
.n-grid {
  align-items: flex-start;
}

.track-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.artist-container {
  display: flex;
  align-items: center;
  margin-bottom: 6px;
}

.artist-avatar {
  margin-right: 8px;
}

.artist-name {
  margin: 0;
}

.track-tags {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.6);
  margin: 2px 0 8px 0;
}

.title-container {
  margin: 0 0 4px 0;
}

.track-title {
  font-size: 1.1rem;
  font-weight: bold;
}

.track-info p {
  margin: 0 0 8px 0;
  color: var(--n-text-color-2);
}

.rating-info {
  display: flex;
  align-items: center;
  gap: 8px;
}

.rank-distribution {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: 8px;
}

.track-actions {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  height: 100%;
}

.rank-number {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--n-text-color-3);
}
</style>
