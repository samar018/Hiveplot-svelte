<script>
  import { onMount } from 'svelte';
  import GenomeScatterPlot from './GeneDataPlot.svelte';
  import HivePlot from './HivePlot.svelte';

  let genesData;
  let downloadProgress = 0;
  let genomeData = [];

  // Function to handle the download button click
  const handleDownload = async () => {
    // Your download logic...
  };

  // Function to fetch the data
  const fetchData = async () => {
    try {
      const response = await fetch('https://vda-lab.github.io/assets/genes.json');
      genesData = await response.json();

      // Transform the data into genomeData required for the scatter plot
      genomeData = genesData.map((gene) => ({
        x: gene.gene_start,
        y: gene.gene_strand === 'forward' ? 1 : -1,
        label: gene.ngn,
        type: gene.axes,
      }));
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  // Fetch the data when the component mounts
  onMount(fetchData);
</script>

<main>
  <h1>Gene Data Display</h1>
  {#if genesData}
    <!-- <pre>{JSON.stringify(genesData, null, 2)}</pre> -->
    <HivePlot data={genesData} />
    <button on:click={handleDownload}>Download Data</button>
    <div class="progress-bar">
      <div class="progress" style="width: {downloadProgress}%"></div>
    </div>
  {:else}
    <p>Loading gene data...</p>
  {/if}
</main>

<style>
  /* Add styling for the component here */
  .progress-bar {
    background-color: #f2f2f2;
    border: 1px solid #ddd;
    border-radius: 4px;
    height: 20px;
    margin-top: 10px;
    overflow: hidden;
  }

  .progress {
    background-color: #4CAF50;
    height: 100%;
    transition: width 0.2s ease-in;
  }
</style>
