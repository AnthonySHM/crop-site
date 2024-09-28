<script lang="ts">
  import { onMount } from 'svelte';

  let featuredCrops: any[] = [];
  let siteName = "CropSite";
  let searchQuery = '';

  onMount(async () => {
    try {
      const response = await fetch('http://localhost:3000/api/crops');
      featuredCrops = await response.json();
    } catch (error) {
      console.error('Error fetching crops:', error);
      // Fallback to some default data if the API call fails
      featuredCrops = [
        { name: 'Wheat', image: 'https://example.com/images/wheat.jpg' },
        { name: 'Rice', image: 'https://example.com/images/rice.jpg' },
        { name: 'Potato', image: 'https://example.com/images/potato.jpg' }
      ];
    }
  });

  function handleSearch() {
    // Implement search functionality here
    console.log('Searching for:', searchQuery);
    // You would typically make an API call here to search for crops
  }
</script>

<style>
  .background-image {
    background-image: url('/images/farm-background.jpg');
    background-size: cover;
    background-position: center;
    min-height: 100vh;
    padding: 2rem 0;
  }
  .content-wrapper {
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 1rem;
    padding: 2rem;
  }

  .card-img-top {
    width: 100%;
    height: 200px; /* Set a fixed height */
    object-fit: cover; /* This will crop the image to fit the container */
  }
</style>

<div class="background-image">
  <div class="container content-wrapper">
    <h1 class="display-4 mb-4 text-primary fw-bold text-center text-shadow">Welcome to <span class="text-success">{siteName}</span></h1>

    <form on:submit|preventDefault={handleSearch} class="mb-4">
      <div class="input-group">
        <input 
          type="text" 
          class="form-control" 
          placeholder="Search for crops..." 
          bind:value={searchQuery}
        >
        <button class="btn btn-primary" type="submit">Search</button>
      </div>
    </form>

    <section class="mb-5">
      <h2 class="h3 mb-3">Featured Crops</h2>
      <div class="row row-cols-1 row-cols-md-3 g-4">
        {#each featuredCrops as crop}
          <div class="col">
            <div class="card h-100">
              <div class="card-img-container">
                <img src={crop.image} class="card-img-top" alt={crop.name} />
              </div>
              <div class="card-body">
                <h3 class="card-title h5">{crop.name}</h3>
                <a href="/crops/{crop._id}" class="btn btn-primary">Learn More</a>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </section>
  </div>
</div>
