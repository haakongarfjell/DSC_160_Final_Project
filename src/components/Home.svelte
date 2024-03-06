<script>
    import Katex from 'svelte-katex';
    import 'katex/dist/katex.min.css';
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    let svg;

    onMount(() => {
        const width = 1000; // Adjust width as needed
        const height = 150; // Adjust height as needed
  
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
            .text('Setpoint reference/')
        svg.append('text')
            .attr('x', referenceX + referenceWidth / 2)
            .attr('y', referenceY + 10)
            .attr('text-anchor', 'middle')
            .attr('dominant-baseline', 'middle')
            .text('Target velocity');
    
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
            .text('Resulting velocity')
        svg.append('text')
            .attr('x', carX + carWidth / 2)
            .attr('y', carY + 10)
            .attr('text-anchor', 'middle')
            .attr('dominant-baseline', 'middle')
            .text('of the car');
        
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
            { element: blackBoxRect, text: "<span style='font-weight: bold;'>BLACK BOX</span> this is where the magic happens, and the controller is applied to the dynamical system of the car in order to produce an input that controls the car to the desired velocity." },
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
    @font-face {
        font-family: 'Gelasio';
        font-style: normal;
        font-weight: 400;
        src: local('Gelasio Regular'), local('Gelasio-Regular'), url(https://fonts.gstatic.com/s/gelasio/v1/cIf9MaFfvUQxTTqS9C6hYQ.woff2) format('woff2');
        unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    
    div {
        font-family: Gelasio
    }

    body {  
        user-select: none; /* Disable text selection */
    }
    .highlight:hover {
        font-weight: bold;
        color: blue;
    }

    .term-box {
        border: 2px solid black;
        padding: 5px;
        border-radius: 10px;
        background-color: #f0f0f0;
        width: 2500px; /* Adjust width as needed */
        margin: 20px; /* Adjust margin as needed */
    }
    .term-box-container {
        display: flex;
        padding: 0px;
        justify-content: space-between;
        width: 1200px;
    }
    .centered {
        display: flex;
        justify-content: center; /* Center align the content */
        margin: 10px, 0px;
    }
</style>
    
<div style="text-align: center;">
    <div>
        <h1>What is the PID-controller?</h1>
        <p1>The proportional-integral-derivative controller (PID controller) is a widely used control system for industrial processes.</p1><br>
        <p1>It is a feedback control system that continuously calculates an error value as the difference between a desired setpoint and a measured process variable.</p1><br>
        <p1>This article will provide an explanation of how we can implement a PID-controller as a cruise control system for a car in order to make the car automatically drive at a desired velocity.</p1><br>
        <div style="text-align: center;"> 
            <svg></svg>
        </div>
        <p1>Given a desired setpoint reference:</p1><Katex>{"r \\,"}</Katex><p1> and a measured velocity</p1><Katex>{"v \\,"}</Katex><p1>, the controller aims to adjust the control input</p1><Katex>{"u \\,"}</Katex><p1> so that the error </p1><Katex>{"e = r - v \\,"}</Katex><p1>approaches 0 in a both fast and controlled manner.<br>
        <p1>The PID controller is composed of three terms: the proportional term, the integral term, and the derivative term.</p1><br>
        <div class="centered">
            <div class="term-box-container">
                <div class="term-box">
                    <h2>Proportional term</h2>
                    <p1>The proportional term of a PID-controller aims to correct the error between the reference and output, by adding a constant scale factor to the input</p1><br><br>
                    <Katex displayMode>{"u = K_p e"}</Katex>
                </div>
                <div class="term-box">
                    <h2>Integral term</h2>
                    <p1>The integral term is proportional to the accumulated error over time and is used to eliminate any residual error that remains after the proportional term has been applied.</p1><br>
                    <Katex displayMode>{"u = K_i \\int_0^t e(\\tau) \\, d\\tau"}</Katex>
                </div>
                <div class="term-box">
                    <h2>Derivative term</h2>
                    <p1>The derivative term is proportional to the rate of change of the error and is used to anticipate future error trends and provide a damping effect on the system.</p1><br>
                    <Katex displayMode>{"u = K_d \\frac{de}{dt}"}</Katex>
                </div>
            </div>    
        </div>  
        <div class="centered">
            <div class="term-box-container">
                <div class="term-box">
                    <h2>The PID-controller</h2>
                    <p1>By combining the three terms we create a PID-controller where the parameters can be tuned to achieve the desired behavior of our system</p1><br>
                    <Katex displayMode>{"u = K_p e + K_i \\int_0^t e(\\tau) \\, d\\tau + K_d \\frac{de}{dt}"}</Katex>
                </div>
            </div>
        </div>
        <p1>Explore how we model the dynamical system of a car on the next page, or jump to the parts you find most interesting!</p1><br>
        <p> </p><br>
        <p> </p><br>

        <!-- <p1>Writeup</p1><br>
        <p1>This is a prototype, future steps will describe the dynamic system, P-controller, PI-controller, PD-controller, and then PID-controller as well as demonstrating its robustness to noise</p1><br>
        <p1>So far we have thought about how we can structure a fun and engaging visual explanation of how one of our favorite controllers from control theory works.</p1><br>
        <p1>We landed on creating an interactive example of cruise control on a car as PID is widely used for this, and it is an understandable entry level example for non-control system engineers</p1><br>
        <p1>So far we have made a working prototype where a user can interact with the PID controller by adjusting gains to the controller.</p1><br>
        <p1>Also a model of the car is shown and a set up for the rest of the project with connecting pages through buttons.</p1><br>
        <p1>The most challening part will be to create an interactive visualisation of the car model (page1). Also describing the robostness to noise can be somewhat challenging to visualize</p1><br>
        <p1> This will be done by interacing with the equations with the mouse. Also describing the robostness to noise can be somewhat challenging to visualize. We are thinking of letting the user adding a hill to the simulations to make it realistic. </p1> -->

    </div>
    <!-- <img src = "https://www.thorlabs.com/images/TabImages/PID2.jpg" alt = "PID controller" width = "800" height = "300"> -->
</div>
