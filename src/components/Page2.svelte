<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    let svg;
  
    onMount(() => {
      const width = 800; // Adjust width as needed
      const height = 400; // Adjust height as needed
  
      // Create SVG element
      svg = d3.select('svg')
        .attr('width', width)
        .attr('height', height);
  
      // Reference block
      const referenceX = 50;
      const referenceY = height / 2;
      const referenceWidth = 100;
      const referenceHeight = 50;
      svg.append('rect')
        .attr('x', referenceX)
        .attr('y', referenceY - referenceHeight / 2)
        .attr('width', referenceWidth)
        .attr('height', referenceHeight)
        .attr('fill', 'lightblue')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', referenceX + referenceWidth / 2)
        .attr('y', referenceY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Reference');
  
      // Error block
      const errorX = referenceX + referenceWidth;
      const errorY = height / 2;
      const errorWidth = 100;
      const errorHeight = 50;
      svg.append('rect')
        .attr('x', errorX)
        .attr('y', errorY - errorHeight / 2)
        .attr('width', errorWidth)
        .attr('height', errorHeight)
        .attr('fill', 'lightblue')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', errorX + errorWidth / 2)
        .attr('y', errorY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Error');
  
      // P block
      const pX = errorX + errorWidth + 50;
      const pY = errorY - 50;
      const pWidth = 100;
      const pHeight = 50;
      svg.append('rect')
        .attr('x', pX)
        .attr('y', pY)
        .attr('width', pWidth)
        .attr('height', pHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', pX + pWidth / 2)
        .attr('y', pY + pHeight / 2)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('P');
  
      // I block
      const iX = errorX + errorWidth + 50;
      const iY = errorY;
      const iWidth = 100;
      const iHeight = 50;
      svg.append('rect')
        .attr('x', iX)
        .attr('y', iY - iHeight / 2)
        .attr('width', iWidth)
        .attr('height', iHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', iX + iWidth / 2)
        .attr('y', iY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('I');
  
      // D block
      const dX = errorX + errorWidth + 50;
      const dY = errorY + 50;
      const dWidth = 100;
      const dHeight = 50;
      svg.append('rect')
        .attr('x', dX)
        .attr('y', dY - dHeight / 2)
        .attr('width', dWidth)
        .attr('height', dHeight)
        .attr('fill', 'lightgreen')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', dX + dWidth / 2)
        .attr('y', dY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('D');
      
      // Sum block
      const sumX = pX + pWidth + 100;
      const sumY = height / 2;
      const sumWidth = 100;
      const sumHeight = 50;
      svg.append('rect')
        .attr('x', sumX)
        .attr('y', sumY - sumHeight / 2)
        .attr('width', sumWidth)
        .attr('height', sumHeight)
        .attr('fill', 'lightblue')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', sumX + sumWidth / 2)
        .attr('y', sumY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('Sum');
  
      // System block
      const systemX = sumX + sumWidth + 100;
      const systemY = height / 2;
      const systemWidth = 100;
      const systemHeight = 50;
      svg.append('rect')
        .attr('x', systemX)
        .attr('y', systemY - systemHeight / 2)
        .attr('width', systemWidth)
        .attr('height', systemHeight)
        .attr('fill', 'lightblue')
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('text')
        .attr('x', systemX + systemWidth / 2)
        .attr('y', systemY)
        .attr('text-anchor', 'middle')
        .attr('dominant-baseline', 'middle')
        .text('System');
      
      // Arrows
      svg.append('line')
        .attr('x1', referenceX + referenceWidth)
        .attr('y1', referenceY)
        .attr('x2', errorX)
        .attr('y2', errorY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', errorX + errorWidth)
        .attr('y1', errorY)
        .attr('x2', pX)
        .attr('y2', pY + pHeight / 2)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', errorX + errorWidth)
        .attr('y1', errorY)
        .attr('x2', iX)
        .attr('y2', iY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', errorX + errorWidth)
        .attr('y1', errorY)
        .attr('x2', dX)
        .attr('y2', dY - dHeight / 2)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', pX + pWidth)
        .attr('y1', pY + pHeight / 2)
        .attr('x2', sumX)
        .attr('y2', sumY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', iX + iWidth)
        .attr('y1', iY)
        .attr('x2', sumX)
        .attr('y2', sumY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', dX + dWidth)
        .attr('y1', dY - dHeight / 2)
        .attr('x2', sumX)
        .attr('y2', sumY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', sumX + sumWidth)
        .attr('y1', sumY)
        .attr('x2', systemX)
        .attr('y2', systemY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
      svg.append('line')
        .attr('x1', systemX)
        .attr('y1', systemY)
        .attr('x2', errorX)
        .attr('y2', errorY)
        .attr('stroke', 'black')
        .attr('stroke-width', 2);
    });
</script>

<div style="text-align: center; margin-top: 20px; padding: 20px; background-color: #f0f0f0; border-radius: 10px;">
    <h1>The PID controller as a Block Diagram</h1>
    <p>This is a block diagram representation of how the PID controller works. Move the mouse over the blocks to explore.</p>
</div>  

<!-- SVG container -->
<div style="text-align: center;"> 
    <svg></svg>
</div>
