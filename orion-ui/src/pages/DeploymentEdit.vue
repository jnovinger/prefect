<template>
  <p-layout-default class="deployment-edit">
    <template #header>
      <PageHeadingDeploymentEdit v-if="deployment" :deployment="deployment" />
    </template>

    <DeploymentForm v-if="deployment" :deployment="deployment" @submit="submit" @cancel="cancel" />
  </p-layout-default>
</template>

<script lang="ts" setup>
  import { PageHeadingDeploymentEdit, DeploymentForm, DeploymentUpdate } from '@prefecthq/orion-design'
  import { showToast } from '@prefecthq/prefect-design'
  import { useSubscription, useRouteParam } from '@prefecthq/vue-compositions'
  import { computed } from 'vue'
  import router from '@/router'
  import { deploymentsApi } from '@/services/deploymentsApi'

  const deploymentId = useRouteParam('id')
  const subscriptionOptions = {
    interval: 300000,
  }

  const deploymentSubscription = useSubscription(deploymentsApi.getDeployment, [deploymentId.value], subscriptionOptions)
  const deployment = computed(() => deploymentSubscription.response)

  async function submit(deployment: DeploymentUpdate): Promise<void> {
    try {
      await deploymentsApi.updateDeployment(deploymentId.value, deployment)
      showToast('Deployment updated', 'success')
      router.back()
    } catch (error) {
      showToast('Error updating deployment', 'error')
      console.warn(error)
    }
  }

  function cancel(): void {
    router.back()
  }
</script>

