<script lang="ts">
  import '../app.css';
  import { onMount } from 'svelte';
  import { WalletProvider } from '@svelte-on-solana/wallet-adapter-ui';

  const localStorageKey = 'walletAdapter';

  let wallets;

  onMount(async () => {
    const { PhantomWalletAdapter, SolflareWalletAdapter, TorusWalletAdapter } =
      await import('@solana/wallet-adapter-wallets');

    const walletsMap = [
      new PhantomWalletAdapter(),
      new SolflareWalletAdapter(),
      new TorusWalletAdapter(),
    ];

    wallets = walletsMap;
  });
</script>

<WalletProvider {localStorageKey} {wallets} autoConnect />
<div>
  <slot />
</div>
