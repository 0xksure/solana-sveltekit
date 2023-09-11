<script lang="ts">
  import { WalletMultiButton } from '@svelte-on-solana/wallet-adapter-ui';
  import { walletStore } from '@svelte-on-solana/wallet-adapter-core';
  import { Connection, VersionedTransaction } from '@solana/web3.js';

  let transaction: string;

  const executeTransaction = async (txbs64: string) => {
    const rpcUrl = process.env.RPC_URL ?? 'https://api.mainnet-beta.solana.com';
    if (!rpcUrl) throw new Error('RPC_URL is not defined');
    const connection = new Connection(rpcUrl, 'confirmed');
    const bufTx = Buffer.from(txbs64, 'base64');
    const vtx = VersionedTransaction.deserialize(bufTx);
    const txid = await connection.simulateTransaction(vtx);
    console.log('txid: ', txid);
  };
</script>

<div class="wrapper-app">
  <div class="title">
    <h1>Solana Transaction executor</h1>
    <p>Transaction (base64)</p>
    <input
      type="text"
      bind:value={transaction}
      placeholder="base64 transaction"
      style="width: 400px; margin: 0 auto; display: block; padding: 10px; border-radius: 5px; border: none; margin-bottom: 20px;"
    />
    <button
      on:click={() => executeTransaction(transaction)}
      style="width: 400px; margin: 0 auto; display: block; padding: 10px; border-radius: 5px; border: none; margin-bottom: 20px;"
    >
      Execute
    </button>
  </div>

  <div class="address">
    <WalletMultiButton />
  </div>

  {#if $walletStore?.connected}
    <p class="warning">You are connected to DevNet!</p>
  {:else}
    <p class="warning">You are not connected...</p>
  {/if}
</div>

<style>
  :global(body) {
    padding: 100px;
    margin: 0;
    background-color: #333333;
  }
  .wrapper-app {
    height: 100vh;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS',
      sans-serif;
  }
  .title {
    text-align: center;
    color: white;
    font-size: 20px;
    margin-bottom: 40px;
  }

  a {
    color: #676796;
  }

  .address {
    position: absolute;
    right: 30px;
    top: 30px;
    border-radius: 5px;
    padding: 10px;
  }
</style>
