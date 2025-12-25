<script lang="ts" setup>
import { computed } from 'vue';
import { useRouter } from 'vue-router';

import { WorkbenchHeader } from '@vben/common-ui';
import { IconifyIcon } from '@vben/icons';
import { preferences } from '@vben/preferences';
import { useUserStore } from '@vben/stores';

const userStore = useUserStore();
const router = useRouter();

// 获取当前时间问候语
const greeting = computed(() => {
  const hour = new Date().getHours();
  if (hour < 6) return '凌晨好';
  if (hour < 9) return '早上好';
  if (hour < 12) return '上午好';
  if (hour < 14) return '中午好';
  if (hour < 17) return '下午好';
  if (hour < 19) return '傍晚好';
  if (hour < 22) return '晚上好';
  return '夜深了';
});

// 功能卡片数据
const featureCards = [
  {
    icon: 'ion:grid-outline',
    title: '仪表盘',
    description: '查看数据概览和统计信息',
    route: '/dashboard',
    color: '#1890ff',
  },
  {
    icon: 'ion:layers-outline',
    title: '组件库',
    description: '浏览丰富的UI组件',
    route: '/demos/features/icons',
    color: '#52c41a',
  },
  {
    icon: 'ion:settings-outline',
    title: '系统设置',
    description: '管理系统配置和偏好',
    route: '/demos/features/login-expired',
    color: '#faad14',
  },
  {
    icon: 'ion:bar-chart-outline',
    title: '数据分析',
    description: '查看详细的数据分析报告',
    route: '/analytics',
    color: '#f5222d',
  },
];

function handleCardClick(route: string) {
  router.push(route).catch((error) => {
    console.error('Navigation failed:', error);
  });
}
</script>

<template>
  <div class="welcome-page">
    <div class="p-5">
      <!-- 欢迎头部 -->
      <WorkbenchHeader
        :avatar="userStore.userInfo?.avatar || preferences.app.defaultAvatar"
      >
        <template #title>
          {{ greeting }}, {{ userStore.userInfo?.realName || '用户' }}，欢迎回来！
        </template>
        <template #description>
          今天是 {{ new Date().toLocaleDateString('zh-CN', { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' }) }}
        </template>
      </WorkbenchHeader>

      <!-- 主要内容区域 -->
      <div class="mt-8">
        <!-- 欢迎文字 -->
        <div class="mb-8 text-center">
          <h1 class="mb-4 text-4xl font-bold text-gray-800 dark:text-gray-100">
            欢迎使用管理系统
          </h1>
          <p class="text-lg text-gray-600 dark:text-gray-400">
            开始探索系统的各项功能，让工作更高效
          </p>
        </div>

        <!-- 功能卡片 -->
        <div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-4">
          <div
            v-for="card in featureCards"
            :key="card.title"
            class="group cursor-pointer rounded-lg border border-gray-200 bg-white p-6 shadow-sm transition-all duration-300 hover:shadow-lg dark:border-gray-700 dark:bg-gray-800"
            @click="handleCardClick(card.route)"
          >
            <div
              class="mb-4 inline-flex h-12 w-12 items-center justify-center rounded-lg transition-colors duration-300"
              :style="{ backgroundColor: `${card.color}15`, color: card.color }"
            >
              <IconifyIcon :icon="card.icon" class="text-2xl" />
            </div>
            <h3 class="mb-2 text-lg font-semibold text-gray-800 dark:text-gray-100">
              {{ card.title }}
            </h3>
            <p class="text-sm text-gray-600 dark:text-gray-400">
              {{ card.description }}
            </p>
          </div>
        </div>

        <!-- 底部提示 -->
        <div class="mt-12 text-center">
          <p class="text-gray-500 dark:text-gray-500">
            如有任何问题，请随时联系系统管理员-ok
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.welcome-page {
  min-height: calc(100vh - 64px);
  background: linear-gradient(
    135deg,
    rgba(24, 144, 255, 0.05) 0%,
    rgba(82, 196, 26, 0.05) 100%
  );
}
</style>
