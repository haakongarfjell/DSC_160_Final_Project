<!-- Page2.svelte -->
<script>
  import { onMount } from 'svelte';
  import katex from 'katex';
  let showTooltip = false;

  function toggleTooltip() {
      showTooltip = !showTooltip;
  }

  onMount(() => {
      katex.render("F_f = k \\cdot v", document.getElementById('friction-force-equation'), {
          throwOnError: false
      });
  });
</script>

<style>
  svg {
      display: block;
      margin: 75px auto 0;
  }

  .tooltip {
      position: absolute;
      background-color: #0b9619;
      color: white;
      padding: 5px;
      border-radius: 5px;
      z-index: 1;
      display: none;
  }
</style>

<div>
  <h1>The car model</h1>
  <p>For simplicity, we assume that the friction F_f force is proportional to the velocity v of the car:</p><br>
  <div id="friction-force-equation"></div>
  <p>We also assume that the car is driving in a single gear with a constant propulsion force u</p>

  <!-- Your Svelte template -->
  <svg width="500" height="250">
      <!-- Top -->
      <rect x="70" y="10" width="220" height="130" fill="transparent" rx="150" stroke="crimson" stroke-width="10" />

      <!-- Body -->
      <rect x="10" y="70" width="340" height="80" fill="crimson" rx="30" />
      <line x1="425" y1="40" x2="305" y2="40" stroke="black" stroke-width="3" />
      <polygon points="285,40 305,50 305,30" fill="black" />
      <text x="350" y="30" class="small">F_l</text>

      <line x1="450" y1="105" x2="350" y2="105" stroke="black" stroke-width="3" />
      <polygon points="470,105 450,115 450,95" fill="black" />
      <text x="400" y="100" class="small">u</text>

      <g>
          <!-- Left line -->
          <line x1="145" y1="10" x2="145" y2="80" stroke="crimson" stroke-width="10" />

          <!-- Right line -->
          <line x1="215" y1="10" x2="215" y2="80" stroke="crimson" stroke-width="10" />
      </g>

      <g>
          <!-- Left bumper -->
          <rect x="0" y="110" width="40" height="20" fill="#999" rx="10" />

          <!-- Right bumper -->
          <rect x="325" y="110" width="40" height="20" fill="#999" rx="10" />
      </g>

      <!-- Left wheel -->
      <g>
          <circle r="40px" fill="#222" stroke="white" stroke-width="7" cx="90" cy="140" />
          <circle r="15px" fill="#555" cx="90" cy="140" />

          <!-- Arrow -->
          <path d="M 20 140 L 90 140" stroke="black" stroke-width="3" />
          <!-- arrow pointing at windshield -->
          <polygon points="0,140 20,150 20,130" fill="black" />
          <text x="0" y="175" class="small">F_f</text>
          <div role="button" tabindex="0" on:mouseenter={toggleTooltip} on:mouseleave={toggleTooltip}>
              {#if showTooltip}
              <!-- Update the tooltip to show some text -->
              <div class="tooltip tooltip-show">
                  <p>This is the friction force (F_f).</p>
              </div>
              {/if}
          </div>
      </g>

      <!-- Right wheel -->
      <g>
          <circle r="40px" fill="#222" stroke="white" stroke-width="7" cx="270" cy="140" />
          <circle r="15px" fill="#555" cx="270" cy="140" />
      </g>

      <g>
          <!-- Gold light -->
          <circle r="15px" fill="gold" cx="340" cy="90" />

          <!-- Orange light -->
          <circle r="10px" fill="orange" cx="15" cy="90" />
      </g>
  </svg>
</div>
