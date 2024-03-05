<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    let svg;
  
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

      const width = 1000; // Adjust width as needed
      const height = 500; // Adjust height as needed
  
      // Create SVG element
      svg = d3.select('svg')
        .attr('width', width)
        .attr('height', height);

     // Define arrow marker
     svg.append('defs').append('marker')
        .attr('id', 'arrow')
        .attr('viewBox', '0 0 10 10')
        .attr('refX', 8)
        .attr('refY', 5)
        .attr('markerWidth', 6)
        .attr('markerHeight', 6)
        .attr('orient', 'auto')
        .append('path')
        .attr('d', 'M 0 0 L 10 5 L 0 10 z')
        .attr('fill', 'black');
  
      // Reference block
      const referenceX = 50;
      const referenceY = height / 2;
      const referenceWidth = 150;
      const referenceHeight = 80;
      const referenceRect = svg.append('rect')
        .attr('x', referenceX)
        .attr('y', referenceY - referenceHeight / 2)
        .attr('width', referenceWidth)
        .attr('height', referenceHeight)
        .attr('fill', 'lightblue')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .on('mouseover', function() {
          d3.select(this).attr('fill', 'lightcyan');
        })
        .on('mouseout', function() {
          d3.select(this).attr('fill', 'lightblue');
        });
      svg.append('text')
        .attr('x', referenceX + referenceWidth / 2)
        .attr('y', referenceY - 10)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Reference for')
      svg.append('text')
        .attr('x', referenceX + referenceWidth / 2)
        .attr('y', referenceY + 10)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Cruise Control');
  
      // BLACK BOX
      const blackBoxX = referenceX + referenceWidth + 200;
      const blackBoxY = height / 2;
      const blackBoxWidth = 150;
      const blackBoxHeight = 80;
      const blackBoxRect = svg.append('rect')
        .attr('x', blackBoxX)
        .attr('y', blackBoxY - blackBoxHeight / 2)
        .attr('width', blackBoxWidth)
        .attr('height', blackBoxHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .on('mouseover', function() {
          d3.select(this).attr('fill', 'lightgreenyellow');
        })
        .on('mouseout', function() {
          d3.select(this).attr('fill', blackBoxRect.attr('original-fill'));
        });
      svg.append('text')
        .attr('x', blackBoxX + blackBoxWidth / 2)
        .attr('y', blackBoxY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('BLACK BOX');
  
      // Car going desired speed
      const carX = blackBoxX + blackBoxWidth + 200;
      const carY = height / 2;
      const carWidth = 150;
      const carHeight = 80;
      const carRect = svg.append('rect')
        .attr('x', carX)
        .attr('y', carY - carHeight / 2)
        .attr('width', carWidth)
        .attr('height', carHeight)
        .attr('fill', 'lightcoral')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .on('mouseover', function() {
          d3.select(this).attr('fill', 'lightcoral');
        })
        .on('mouseout', function() {
          d3.select(this).attr('fill', 'lightcoral');
        });
      svg.append('text')
        .attr('x', carX + carWidth / 2)
        .attr('y', carY - 10)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Car going desired')
      svg.append('text')
        .attr('x', carX + carWidth / 2)
        .attr('y', carY + 10)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('speed');
      
      // Tooltip
      const tooltip = d3.select("body").append("div")
        .style("opacity", 0)
        .style("position", "absolute")
        .style("background-color", "lightgray")
        .style("border", "1px solid black")
        .style("border-radius", "5px")
        .style("padding", "10px")
        .style("pointer-events", "none");
      
        const blocks = [
        { element: referenceRect, text: "<span style='font-weight: bold;'>Reference</span> represents the desired state or setpoint for the system.<br>In the context of cruise control, it signifies the target speed the vehicle aims to maintain." },
        { element: blackBoxRect, text: "<span style='font-weight: bold;'>BLACK BOX</span> is the controller itself, processing the error signal and generating control actions based on PID parameters." },
        { element: carRect, text: "<span style='font-weight: bold;'>Car going desired speed</span> represents the system output, indicating whether the car is achieving the desired speed set by the reference." }
        ];

      // Arrows
      const arrowPathRefToBox = `M${referenceX + referenceWidth},${referenceY} L${blackBoxX},${blackBoxY}`;
      const arrowPathBoxToCar = `M${blackBoxX + blackBoxWidth},${blackBoxY} L${carX},${carY}`;
      svg.append('path')
        .attr('d', arrowPathRefToBox)
        .attr('fill', 'none')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      svg.append('path')
        .attr('d', arrowPathBoxToCar)
        .attr('fill', 'none')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      
      blocks.forEach(block => {
        // Store original fill color
        block.element.attr('original-fill', block.element.attr('fill'));
        
        block.element.on("mouseover", function() {
          block.element.attr('fill', 'lightcyan');
          const { x, y, width, height } = block.element.node().getBoundingClientRect();
          const centerX = x + width / 2;
          const centerY = y + height / 2;
          const tooltipX = centerX - tooltip.node().offsetWidth / 2;
          const tooltipY = y + height + 20;
          
          tooltip.transition()
            .duration(200)
            .style("opacity", 1)
            .style("left", tooltipX + "px")
            .style("top", tooltipY + "px");
            
          tooltip.html('<div>' + block.text + '</div>');
        })
        .on("mouseout", function() {
          // Reset to original fill color
          block.element.attr('fill', block.element.attr('original-fill'));
          tooltip.transition()
            .duration(500)
            .style("opacity", 0);
        });
      });
    });
</script>

<style>
    body {  
        user-select: none; /* Disable text selection */
    }
    .highlight:hover {
        font-weight: bold;
        color: blue;
    }
</style>

<div style="text-align: center; margin-top: 20px; padding: 20px; background-color: #f0f0f0; border-radius: 10px;">
    <h1>The PID controller as a Block Diagram</h1>
    <p>This is a block diagram representation of how the PID controller works. Move the mouse over the blocks to explore.</p>
</div>  

<!-- SVG container -->
<div style="text-align: center;"> 
    <svg></svg>
</div>
