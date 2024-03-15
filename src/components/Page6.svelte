<script> 
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import factory from './images/working-factory.png'
    import plane from './images/plane.png'
    import pcb from './images/pcb-board.png'

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
            { element: blackBoxRect, text: "<span style='font-weight: bold;'>BLACK BOX</span> PID controller" },
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
    .image-container {
        margin-bottom: 20px; /* Add margin between image containers */
    }
    .image-container > div {
        display: inline-block; /* Display divs containing image and description next to each other */
        vertical-align: top; /* Align divs to the top */
    }
    .scaled-image {
        max-width: 100%; /* Ensure the image doesn't exceed its container's width */
        height: auto; /* Maintain the aspect ratio of the image */
        width: 100px;
        height: 100px;
        margin-left: 30px;
        margin-right: 30px;
    }
    /* Optionally, you can add margin to the bottom of each image */
    .image-container > div:last-child {
        margin-right: 0; /* Remove the right margin from the last image container */
    }
</style>

<div style="text-align: center;">
    <div>
        <h1>Takeaways</h1>
        <div class="centered">
            <div class="term-box-container">
                <div class="term-box">
                    <h2>What was the question?</h2>
                    <p1>To remind our self of what the initial question was, we return to the black box model. We ask our self: given a desired velocity, how can we ensure the car achieves this velocity?</p1>
                    <div style="text-align: center;"> 
                        <svg></svg>
                    </div>
                    <p1>We know now that this can be achieved with a PID controller, which raises the question of what other systems the can PID controller be used for?</p1><br>
                    <h2>Applications</h2>
                    <p>As demonstrated, the PID controller is surprisingly simple. An important takeaway is that the PID controller is applicable to all dynamical systems as long as you can apply an input and measure the output.
                        In this example, we use a simple model to simulate the car dynamics, but in practice you would use a sensor to measure the velocity of the car and apply the input produced by the controller.
                    </p>
                    <p>
                        The idea of calculating an input based on a measurement is not limited to our example, but can be applied to any system where you want a desired output! Some other examples of where the PID controller is widely used are:
                    </p>
                    <div class="image-container">
                        <div>
                            <img src={factory} alt="Factory" class="scaled-image">
                            <p>  Industrial processes  </p>
                        </div>
                        <div>
                            <img src={plane} alt="Plane" class="scaled-image">
                            <p>  Aircraft systems  </p>
                        </div>

                        <div>
                            <img src={pcb} alt="Pcb" class="scaled-image">
                            <p>  Electronics  </p>
                        </div>
                    </div>
                    <h2>Why do you need control engineers?</h2>
                    <p>If the PID controller can be applied to all dynamical systems, why not save the cost of the control engineer and do it yourself?</p>
                    <p>The most important thing you should find as you play around with the tuning parameters is that it can be hard to find the perfect set of parameters.
                        In fact there is no one-size-fits-all solution. For every system, the parameters will be different, but with more knowledge and experience there is no limit to what you can control. 
                    </p>
                </div>
            </div>
        </div>
    </div>
</div>

<a href="https://www.flaticon.com/free-icons/factory" title="factory icons">Factory icons created by Freepik - Flaticon</a><br>
<a href="https://www.flaticon.com/free-icons/plane" title="plane icons">Plane icons created by Freepik - Flaticon</a><br>
<a href="https://www.flaticon.com/free-icons/pcb-board" title="pcb board icons">Pcb board icons created by Design Circle - Flaticon</a><br>
<br><br>

