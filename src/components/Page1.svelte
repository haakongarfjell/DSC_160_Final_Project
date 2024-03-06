<!-- Page2.svelte -->
<script>
    import { onMount } from 'svelte';
    import Katex from 'svelte-katex';
    import 'katex/dist/katex.min.css';
    let showTooltip = false;

    function toggleTooltip() {
        showTooltip = !showTooltip;
    }

    onMount(() => {
        // Load MathJax script when component mounts
        const script = document.createElement('script');
        script.src = 'https://polyfill.io/v3/polyfill.min.js?features=es6';
        script.async = true;
        document.head.appendChild(script);

        const mathjaxScript = document.createElement('script');
        mathjaxScript.id = 'MathJax-script';
        mathjaxScript.async = true;
        mathjaxScript.src = 'https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js';
        document.head.appendChild(mathjaxScript);
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

    .tooltip-show {
        display: block;
    }

    ul {
        list-style-position: inside; /* Align bullet points inside the list */
    }
</style>


<div style="text-align: center; margin-top: 20px; padding: 20px; background-color: #f0f0f0; border-radius: 10px;">
    <h1>Modelling the car</h1>
    <p>To implement a PID controller, one needs a model of a physical system. In this case, we will use a simple model of a car.</p>
    <p>The car is subject to air resistance, friction, and engine power. The differential equation governing the car's motion is derived from Newton's law of motion:</p>
    <Katex displayMode>{"\\sum F = ma"}</Katex>
    <p>A simple motion model for a car can then be expressed as:</p>
    <Katex displayMode>{"u - F_f - F_l = m \\frac{dv}{dt}"}</Katex>
    <p1>Where </p1><Katex>{"u \\,"}</Katex><p1> is the control input (think of this as pressing the gas pedal), </p1> <Katex>{"F_f \\,"}</Katex><p1> is the friction force and </p1> <Katex>{"F_l \\,"}</Katex><p1> is the air resistance force.</p1><br>
    <p1>Inserting standard models for friction and air resistance: </p1> <Katex>{"F_f = \\mu N = \\mu m g"}</Katex> <p1> and </p1> <Katex>{"F_l = k v"}</Katex> <p1> ,where </p1> <Katex>{"\\mu"}</Katex> <p1> and </p1> <Katex>{"k"}</Katex> <p1> are the friction and air resitance coefficients repsectively, we get:</p1><br>
    <Katex displayMode>{"u - \\mu m g - k v = m \\frac{dv}{dt}"}</Katex>
    <Katex displayMode>{"\\frac{dv}{dt} = u - \\mu g - \\frac{k}{m} v"}</Katex>
    <p>We now have a differential equation describing how the velocity changes over time. Explore how the car's mass and and velocity affects the forces below!</p>
</div>  

<div style="position: relative;">
    <!-- Your Svelte template -->
    <svg width="500" height="250">
        <!-- Top -->
        <rect x="70" y="10" width="220" height="130" fill="transparent" rx="150" stroke="crimson" stroke-width="10" />
  
        <!-- Body -->
        <rect x="10" y="70" width="340" height="80" fill="crimson" rx="30" />
        <line x1="425" y1="40" x2="305" y2="40" stroke="black" stroke-width="3" />
        <polygon points="285,40 305,50 305,30" fill="black" />
        <text x="350" y="30" class="small" on:mouseenter={toggleTooltip} on:mouseleave={toggleTooltip}>Air Resistance Force </text>
  
        <line x1="450" y1="105" x2="350" y2="105" stroke="black" stroke-width="3" />
        <polygon points="470,105 450,115 450,95" fill="black" />
        <text x="400" y="130" class="small" on:mouseenter={toggleTooltip} on:mouseleave={toggleTooltip}>Engine Power</text>
  
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
            <text x="0" y="190" class="small" on:mouseenter={toggleTooltip} on:mouseleave={toggleTooltip}>Friction Force</text>
            <div class="tooltip" style="top: 200px; left: 0;">
                <p>This is the friction force (\( F_f \)).</p>
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
