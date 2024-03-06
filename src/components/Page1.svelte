<!-- Page2.svelte -->
<script>
    import { onMount } from 'svelte';
    import Katex from 'svelte-katex';
    import 'katex/dist/katex.min.css';
    import * as d3 from 'd3';

    let svgWidth = 500;
    let svgHeight = 200;
    let carSpeed = 100;
    let carMass = 50;

    // Define variables for car dimensions and positions
    let carTopX = 70;
    let carTopY = 10;
    let carTopWidth = 220;
    let carTopHeight = 130;
    let carBodyX = 10;
    let carBodyY = 70;
    let carBodyWidth = 340;
    let carBodyHeight = 80;
    let leftLineX = carTopX + carTopWidth / 2;
    let rightLineX = leftLineX + 70;
    let leftBumperX = 0;
    let rightBumperX = carBodyWidth - 40;
    let leftWheelX = 90;
    let rightWheelX = 270;
    let wheelY = 140;
    let goldLightX = carBodyWidth;
    let goldLightY = carTopY + carTopHeight / 2 + 10;
    let orangeLightX = carBodyX - 25;
    let orangeLightY = carTopY + carTopHeight / 2;
    let groundY = 180;
    let groundLineX2 = carBodyWidth;
    let polygonX = carTopX + carTopWidth / 2 + 10;

    function plotData() {
        const svg = d3.select('svg')
            .attr('width', svgWidth)
            .attr('height', svgHeight);

        // Remove old plot before drawing new one
        svg.selectAll('*').remove();

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

        // Car top
        svg.append('rect')
            .attr('x', carTopX)
            .attr('y', carTopY)
            .attr('width', carTopWidth)
            .attr('height', carTopHeight)
            .attr('fill', 'transparent')
            .attr('rx', carTopHeight / 2)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Car body
        svg.append('rect')
            .attr('x', carBodyX)
            .attr('y', carBodyY)
            .attr('width', carBodyWidth)
            .attr('height', carBodyHeight)
            .attr('fill', 'crimson')
            .attr('rx', 30);

        // Left line
        svg.append('line')
            .attr('x1', leftLineX)
            .attr('y1', carTopY)
            .attr('x2', leftLineX)
            .attr('y2', carBodyY)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Right line
        svg.append('line')
            .attr('x1', rightLineX)
            .attr('y1', carTopY + 5)
            .attr('x2', rightLineX)
            .attr('y2', carBodyY)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Right line
        svg.append('line')
            .attr('x1', leftLineX-70)
            .attr('y1', carTopY + 5)
            .attr('x2', leftLineX-70)
            .attr('y2', carBodyY)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Left bumper
        svg.append('rect')
            .attr('x', leftBumperX)
            .attr('y', carBodyY + 40)
            .attr('width', 40)
            .attr('height', 20)
            .attr('fill', '#999')
            .attr('rx', 10);

        // Right bumper
        svg.append('rect')
            .attr('x', rightBumperX + 25)
            .attr('y', carBodyY + 40)
            .attr('width', 40)
            .attr('height', 20)
            .attr('fill', '#999')
            .attr('rx', 10);

        // Left wheel
        svg.append('g')
            .attr('transform', `translate(${leftWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '40px')
            .attr('fill', '#222')
            .attr('stroke', 'white')
            .attr('stroke-width', 7);

        // Right wheel
        svg.append('g')
            .attr('transform', `translate(${rightWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '40px')
            .attr('fill', '#222')
            .attr('stroke', 'white')
            .attr('stroke-width', 7);

        // Left wheel
        svg.append('g')
            .attr('transform', `translate(${leftWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '15px')
            .attr('fill', '#555');

        // Left wheel
        svg.append('g')
            .attr('transform', `translate(${rightWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '15px')
            .attr('fill', '#555');

        // Headlights
        svg.append('g')
            .attr('transform', `translate(${goldLightX}, ${goldLightY})`)
            .append('circle')
            .attr('r', '15px')
            .attr('fill', 'gold');


        // Friction force line with arrow
        svg.append('line')
            .attr('x1', rightWheelX)
            .attr('y1', groundY)
            .attr('x2', rightWheelX - carMass)
            .attr('y2', groundY)
            .attr('stroke', 'black')
            .attr('stroke-width', 3)
            .attr('marker-end', 'url(#arrow)');

        // Text under the arrow
        svg.append('text')
            .attr('x', rightWheelX)
            .attr('y', groundY + 20)
            .text('Ground Friction Force')
            .style('text-anchor', 'middle')
            .style('font-size', '14px')
            .style('fill', 'black')
            .style('font-weight', 'bold');

         // Engine force line with arrow
        svg.append('line')
            .attr('x1', rightBumperX+50)
            .attr('y1', carBodyY + 50)
            .attr('x2', rightBumperX + 130)
            .attr('y2', carBodyY + 50)
            .attr('stroke', 'black')
            .attr('stroke-width', 3)
            .attr('marker-end', 'url(#arrow)');

        // Text under the arrow
        svg.append('text')
            .attr('x', rightBumperX+100)
            .attr('y', carBodyY + 80)
            .text('Engine Force')
            .style('text-anchor', 'middle')
            .style('font-size', '14px')
            .style('fill', 'black')
            .style('font-weight', 'bold');   

        // Air Recistiance force line with arrow
        svg.append('line')
            .attr('x1', rightBumperX + carSpeed)
            .attr('y1', carBodyY-10)
            .attr('x2', rightBumperX)
            .attr('y2', carBodyY - 10) 
            .attr('stroke', 'black')
            .attr('stroke-width', 3)
            .attr('marker-end', 'url(#arrow)');

        // Text under the arrow
        svg.append('text')
            .attr('x', rightBumperX + 60)
            .attr('y', carBodyY - 30)
            .text('Air Resistance  Force')
            .style('text-anchor', 'middle')
            .style('font-size', '14px')
            .style('fill', 'black')
            .style('font-weight', 'bold');  
    }

    function updateCarMass(event) {
        carMass = +event.target.value;
        plotData();
    }
    function updateCarSpeed(event) {
        carSpeed = +event.target.value;
        plotData();
    }

    onMount(() => {
        plotData();
    });
</script>


<style>
    /* Slider Styles */
    input[type="range"] {
      -webkit-appearance: none;
      appearance: none;
      width: 200px;
      height: 10px;
      background: #d3d3d3;
      outline: none;
      opacity: 0.7;
      -webkit-transition: .2s;
      transition: opacity .2s;
      border-radius: 5px;
      margin-bottom: 10px;
    }
  
    input[type="range"]:hover {
      opacity: 1;
    }
  
    input[type="range"]::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 20px;
      height: 20px;
      background: #4CAF50;
      cursor: pointer;
      border-radius: 50%;
    }
  
    input[type="range"]::-moz-range-thumb {
      width: 20px;
      height: 20px;
      background: #4CAF50;
      cursor: pointer;
      border-radius: 50%;
    }
  
    /* Label Styles */
    label {
      font-family: Arial, sans-serif;
      font-size: 16px;
      color: #333;
    }
  
      #plot {
      margin: auto; /* Center the plot */
      width: 800px; /* Adjust width */
      height: 500px; /* Adjust height */
      }
  </style>


<div style="text-align: center; margin-top: 20px; margin-left: 500px; margin-right: 500px;  padding: 20px; background-color: #f0f0f0; border: 2px solid #000; border-radius: 10px;">
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

<div style="text-align: center; margin-top: 0px; padding: 20px;"> 
    <svg></svg>
</div>
<div style="text-align: center; margin-top: 0px;">
    <label for="carMass">Car Mass:</label>
    <input type="range" id="carMass" name="carMass" min="10" max="100" bind:value={carMass} on:input={updateCarMass}>
    <span>{carMass}</span>
    <br>
    <label for="carSpeed">Car Speed:</label>
    <input type="range" id="carSpeed" name="carSpeed" min="10" max="100" bind:value={carSpeed} on:input={updateCarSpeed}>
    <span>{carSpeed}</span>
</div>
<div style="text-align: center; margin-top: 20px; padding: 20px; background-color: #00000; border-radius: 10px;">
</div>