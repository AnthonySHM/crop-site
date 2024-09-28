<script lang="ts">
  import { page } from '$app/stores';
  import { onMount } from 'svelte';

  let crop: any = null;
  let activeTab = 'overview';

  onMount(async () => {
    const cropId = $page.params.id;
    try {
      const response = await fetch(`http://localhost:3000/api/crops/${cropId}`);
      if (!response.ok) {
        throw new Error('Crop not found');
      }
      crop = await response.json();
    } catch (error) {
      console.error('Error fetching crop details:', error);
      // Fallback to some default data if the API call fails
      crop = {
        id: cropId,
        name: 'Crop Not Found',
        image: 'https://example.com/images/default-crop.jpg',
        overview: 'Sorry, we couldn\'t find information for this crop.',
        planting: 'N/A',
        care: 'N/A',
        harvest: 'N/A',
        economics: 'N/A',
        comments: []
      };
    }
  });

  function setActiveTab(tab: string) {
    activeTab = tab;
  }
</script>

<div class="container mt-4">
  {#if crop}
    <h1 class="mb-4">{crop.name}</h1>
    <div class="row">
      <div class="col-md-4">
        <img src={crop.image} alt={crop.name} class="img-fluid mb-3" />
      </div>
      <div class="col-md-8">
        <ul class="nav nav-tabs mb-3">
          <li class="nav-item">
            <button class="nav-link" class:active={activeTab === 'overview'} on:click={() => setActiveTab('overview')}>Overview</button>
          </li>
          <li class="nav-item">
            <button class="nav-link" class:active={activeTab === 'planting'} on:click={() => setActiveTab('planting')}>Planting</button>
          </li>
          <li class="nav-item">
            <button class="nav-link" class:active={activeTab === 'care'} on:click={() => setActiveTab('care')}>Care</button>
          </li>
          <li class="nav-item">
            <button class="nav-link" class:active={activeTab === 'harvest'} on:click={() => setActiveTab('harvest')}>Harvest</button>
          </li>
          <li class="nav-item">
            <button class="nav-link" class:active={activeTab === 'economics'} on:click={() => setActiveTab('economics')}>Economics</button>
          </li>
          <li class="nav-item">
            <button class="nav-link" class:active={activeTab === 'comments'} on:click={() => setActiveTab('comments')}>Comments</button>
          </li>
        </ul>

        <div class="tab-content">
          {#if activeTab === 'overview'}
            <p>{crop.overview}</p>
          {:else if activeTab === 'planting'}
            <p>{crop.planting}</p>
          {:else if activeTab === 'care'}
            <p>{crop.care}</p>
          {:else if activeTab === 'harvest'}
            <p>{crop.harvest}</p>
          {:else if activeTab === 'economics'}
            <p>{crop.economics}</p>
          {:else if activeTab === 'comments'}
            <p>Comments section (to be implemented)</p>
          {/if}
        </div>
      </div>
    </div>
  {:else}
    <p>Loading crop details...</p>
  {/if}
</div>
