<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import Katex from 'svelte-katex';
    // import { InlineMath } from 'svelte-katex';
    import 'katex/dist/katex.min.css';
  
    let svg;
  
    onMount(() => {
      const width = 1200; // Adjust width as needed
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
      const referenceWidth = 100;
      const referenceHeight = 50;
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
      .attr('y', referenceY)
      .attr('text-anchor', 'middle')
      .attr('dominant-baseline', 'middle')
      .text('Reference');
        
      // Error circle
      const errorX = referenceX + referenceWidth + 200;
      const errorY = height / 2;
      const errorRadius = 30;
      const errorCircle = svg.append('circle')
        .attr('cx', errorX)
        .attr('cy', errorY)
        .attr('r', errorRadius)
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
        .attr('x', errorX)
        .attr('y', errorY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Error');
  
      // P block
      const pX = errorX + errorRadius + 150;
      const pY = height / 2 - 100;
      const pWidth = 100;
      const pHeight = 50;
      const pRect = svg.append('rect')
        .attr('x', pX)
        .attr('y', pY)
        .attr('width', pWidth)
        .attr('height', pHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .on('mouseover', function() {
          d3.select(this).attr('fill', 'lightgreenyellow');
        })
        .on('mouseout', function() {
          d3.select(this).attr('fill', pRect.attr('original-fill'));
        });
      svg.append('text')
        .attr('x', pX + pWidth / 2)
        .attr('y', pY + pHeight / 2)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('P');
  
      // I block
      const iX = errorX + errorRadius + 150;
      const iY = height / 2;
      const iWidth = 100;
      const iHeight = 50;
      const iRect = svg.append('rect')
        .attr('x', iX)
        .attr('y', iY - iHeight / 2)
        .attr('width', iWidth)
        .attr('height', iHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .on('mouseover', function() {
          d3.select(this).attr('fill', 'lightgreenyellow');
        })
        .on('mouseout', function() {
          d3.select(this).attr('fill', iRect.attr('original-fill'));
        });
      svg.append('text')
        .attr('x', iX + iWidth / 2)
        .attr('y', iY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('I');
  
      // D block
      const dX = errorX + errorRadius + 150;
      const dY = height / 2 + 100;
      const dWidth = 100;
      const dHeight = 50;
      const dRect = svg.append('rect')
        .attr('x', dX)
        .attr('y', dY - dHeight / 2 - dHeight/2)
        .attr('width', dWidth)
        .attr('height', dHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .on('mouseover', function() {
          d3.select(this).attr('fill', 'lightgreenyellow');
        })
        .on('mouseout', function() {
          d3.select(this).attr('fill', dRect.attr('original-fill'));
        });
      svg.append('text')
        .attr('x', dX + dWidth / 2)
        .attr('y', dY - dHeight/2)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('D');
      
      // Sum circle
      const sumX = pX + pWidth + 200;
      const sumY = height / 2;
      const sumRadius = 30;
      const sumCircle = svg.append('circle')
        .attr('cx', sumX)
        .attr('cy', sumY)
        .attr('r', sumRadius)
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
        .attr('x', sumX)
        .attr('y', sumY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Sum');
  
      // System block
      const systemX = sumX + sumRadius + 200;
      const systemY = height / 2 ; // Adjusted system Y position
      const systemWidth = 100;
      const systemHeight = 50;
      const systemRect = svg.append('rect')
        .attr('x', systemX)
        .attr('y', systemY - systemHeight / 2)
        .attr('width', systemWidth)
        .attr('height', systemHeight)
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
        .attr('x', systemX + systemWidth / 2)
        .attr('y', systemY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('System');

      const rX = referenceX + (errorX - referenceX) / 2 + 30;
      const rY = referenceY - 10;
      svg.append('text')
        .attr('x', rX)
        .attr('y', rY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('r');

      const uX = sumX + (systemX - sumX) / 2 + 10;
      const uY = sumY - 10;
      svg.append('text')
        .attr('x', uX)
        .attr('y', uY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('u');

      const vX = systemX + systemWidth / 2 + 10;
      const vY = systemY + systemHeight  + 50;
      svg.append('text')
        .attr('x', vX)
        .attr('y', vY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('v');

      // Arrows
      svg.append('line')
        .attr('x1', referenceX + referenceWidth)
        .attr('y1', referenceY)
        .attr('x2', errorX - errorRadius)
        .attr('y2', errorY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
    
      svg.append('line')
        .attr('x1', errorX + errorRadius)
        .attr('y1', errorY)
        .attr('x2', pX)
        .attr('y2', pY + pHeight / 2)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      svg.append('line')
        .attr('x1', errorX + errorRadius)
        .attr('y1', errorY)
        .attr('x2', iX)
        .attr('y2', iY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      svg.append('line')
        .attr('x1', errorX + errorRadius)
        .attr('y1', errorY)
        .attr('x2', dX)
        .attr('y2', dY - dHeight / 2)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      svg.append('line')
        .attr('x1', pX + pWidth)
        .attr('y1', pY + pHeight / 2)
        .attr('x2', sumX - sumRadius)
        .attr('y2', sumY - 8)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      svg.append('line')
        .attr('x1', iX + iWidth)
        .attr('y1', iY)
        .attr('x2', sumX - sumRadius)
        .attr('y2', sumY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
      svg.append('line')
        .attr('x1', dX + dWidth)
        .attr('y1', dY - dHeight / 2)
        .attr('x2', sumX - sumRadius)
        .attr('y2', sumY + 8)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');

      svg.append('line')
        .attr('x1', sumX + sumRadius)
        .attr('y1', sumY)
        .attr('x2', systemX)
        .attr('y2', systemY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');
    
      svg.append('line')
        .attr('x1', systemX+systemWidth/2)
        .attr('y1', systemY+systemHeight/2)
        .attr('x2', systemX+systemWidth/2)
        .attr('y2', systemY+200)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);

      svg.append('line')
        .attr('x1', systemX+systemWidth/2)
        .attr('y1', systemY+200)
        .attr('x2', errorX)
        .attr('y2', systemY+200)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', errorX)
        .attr('y1', errorY + 200)
        .attr('x2', errorX)
        .attr('y2', errorY + errorRadius)
        .attr('stroke', 'black')
        .attr('stroke-width', 2)
        .attr('marker-end', 'url(#arrow)');      
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
        { element: referenceRect, text: "<span style='font-weight: bold;'>Reference</span> represents the desired state or setpoint for the system.<br>In the context of cruise control, it signifies the target speed the vehicle aims to maintain."  },
        { element: errorCircle, text: "<span style='font-weight: bold;'>Error</span> indicates the disparity between the reference value and the actual output of the system.<br>For example, in cruise control, if the desired speed is set to 50 km/h but the speedometer reads 30 km/h, the error would be 20 km/h." },
        { element: pRect, text: "<span style='font-weight: bold;'>P (Proportional Gain)</span> determines the immediate response of the system to changes in error.<br>It produces an output proportional to the current error multiplied by a constant factor, Kp." },
        { element: iRect, text: "<span style='font-weight: bold;'>I (Integral Gain)</span> accounts for accumulated error over time.<br>It integrates the error signal with respect to time, helping to eliminate steady-state error and improve system stability." },
        { element: dRect, text: "<span style='font-weight: bold;'>D (Derivative Gain)</span> anticipates future trends in the error signal by measuring its rate of change. <br>This helps dampen oscillations and improve transient response." },
        { element: sumCircle, text: "<span style='font-weight: bold;'>Sum</span> combines the contributions from the P, I, and D components to form the overall control signal applied to the system." },
        { element: systemRect, text: "<span style='font-weight: bold;'>System</span> represents the dynamics of the physical system being controlled. In the context of this project, it could represent the model of the car and its behavior under different conditions." }
        ];

      
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
