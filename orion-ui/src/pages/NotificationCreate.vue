<template>
  <p-layout-default class="notification-create">
    <template #header>
      <PageHeadingNotificationCreate />
    </template>
    <NotificationForm action="Create" @submit="submit" @cancel="cancel" />
  </p-layout-default>
</template>

<script lang="ts" setup>
  import { NotificationForm, Notification, PageHeadingNotificationCreate, NotificationCreate } from '@prefecthq/orion-design'
  import { showToast } from '@prefecthq/prefect-design'
  import router, { routes } from '@/router'
  import { notificationsApi } from '@/services/notificationsApi'

  async function submit(notification: Partial<Notification>): Promise<void> {
    try {
      await notificationsApi.createNotification(notification as NotificationCreate)
      router.push(routes.notifications())
    } catch (error) {
      showToast('Error creating notification', 'error')
      console.warn(error)
    }
  }

  function cancel(): void {
    router.push(routes.notifications())
  }
</script>