<script setup>
  import { createAppKit, useAppKit } from '@reown/appkit/vue'

  import { WagmiProvider } from 'wagmi'
  import { arbitrum, mainnet } from '@reown/appkit/networks'
  import { WagmiAdapter } from '@reown/appkit-adapter-wagmi'
  import { reconnect } from '@wagmi/core'

  // 1. Get projectId from https://cloud.reown.com
  const projectId = 'c0805978d723d80b29c63280673fd11d'

  // 2. Create a metadata object - optional
  const metadata = {
    name: 'TestClustr',
    description: 'AppKit Example',
    url: 'https://example.com', // origin must match your domain & subdomain
    icons: ['https://avatars.githubusercontent.com/u/179229932']
  }

  const networks = [mainnet, arbitrum]

  // 3. Create Wagmi Adapter
  const wagmiAdapter = new WagmiAdapter({
    ssr: true,
    projectId,
    networks
  })

  // 4. Create modal
  createAppKit({
    adapters: [wagmiAdapter],
    networks: [mainnet, arbitrum],
    metadata,
    projectId,
    features: {
      analytics: true // Optional - defaults to your Cloud configuration
    }
  })

  // 5. Use modal composable
  const modal = useAppKit()

  onMounted(() => {
    reconnect(wagmiAdapter.wagmiConfig)
  })
</script>

<template>
  <w3m-button label="Wallet connect"/>
</template>
