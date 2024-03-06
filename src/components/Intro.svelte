<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let svgWidth = 1800; //3500
    let svgHeight = 500;
    let carSpeed = 8000;
    let textCarSpeed = 0;
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

        // Car body
        const car = svg.append('g')
            .attr('id', 'car');

        car.append('rect')
            .attr('x', carTopX)
            .attr('y', carTopY)
            .attr('width', carTopWidth)
            .attr('height', carTopHeight)
            .attr('fill', 'transparent')
            .attr('rx', carTopHeight / 2)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Car body
        car.append('rect')
            .attr('x', carBodyX)
            .attr('y', carBodyY)
            .attr('width', carBodyWidth)
            .attr('height', carBodyHeight)
            .attr('fill', 'crimson')
            .attr('rx', 30);


        // Left line
        car.append('line')
            .attr('x1', leftLineX)
            .attr('y1', carTopY)
            .attr('x2', leftLineX)
            .attr('y2', carBodyY)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Right line
        car.append('line')
            .attr('x1', rightLineX)
            .attr('y1', carTopY + 5)
            .attr('x2', rightLineX)
            .attr('y2', carBodyY)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);

        // Right line
        car.append('line')
            .attr('x1', leftLineX-70)
            .attr('y1', carTopY + 5)
            .attr('x2', leftLineX-70)
            .attr('y2', carBodyY)
            .attr('stroke', 'crimson')
            .attr('stroke-width', 10);    

         // Left bumper
         car.append('rect')
            .attr('x', leftBumperX)
            .attr('y', carBodyY + 40)
            .attr('width', 40)
            .attr('height', 20)
            .attr('fill', '#999')
            .attr('rx', 10);

        // Right bumper
        car.append('rect')
            .attr('x', rightBumperX + 25)
            .attr('y', carBodyY + 40)
            .attr('width', 40)
            .attr('height', 20)
            .attr('fill', '#999')
            .attr('rx', 10);

        // Left wheel
        car.append('g')
            .attr('transform', `translate(${leftWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '40px')
            .attr('fill', '#222')
            .attr('stroke', 'white')
            .attr('stroke-width', 7);

        // Right wheel
        car.append('g')
            .attr('transform', `translate(${rightWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '40px')
            .attr('fill', '#222')
            .attr('stroke', 'white')
            .attr('stroke-width', 7);

        // Left wheel
        car.append('g')
            .attr('transform', `translate(${leftWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '15px')
            .attr('fill', '#555');

        // Left wheel
        car.append('g')
            .attr('transform', `translate(${rightWheelX}, ${wheelY})`)
            .append('circle')
            .attr('r', '15px')
            .attr('fill', '#555');

        // Headlights
        car.append('g')
            .attr('transform', `translate(${goldLightX}, ${goldLightY})`)
            .append('circle')
            .attr('r', '15px')
            .attr('fill', 'gold');    

        // Road
        car.append('line')
            .attr('x1',-svgWidth)
            .attr('y1', groundY)
            .attr('x2', groundLineX2 + svgWidth)
            .attr('y2', groundY)
            .attr('stroke', 'black')
            .attr('stroke-width', 10);

            // Set speed text
        svg.append('text')
            .attr('x', 0)
            .attr('y', 240)
            .attr('fill', 'black')
            .text('Set speed: 100')
            .attr('font-size', 60);

        moveCar(car);
    }

    function moveCar(car) {

        car.transition()
            .duration(carSpeed)
            .attr('transform', 'translate(' + (svgWidth) + ',0)')
            .on('end', function() {
                d3.select(this).attr('transform', 'translate(-' + carBodyWidth + ',0)');
                moveCar(car);
            });
    }

    onMount(() => {
        plotData();
    });
</script>


<div style="text-align: center; margin-top: 200px; margin-left: 500px; margin-right: 500px;  padding: 20px; background-color: #f0f0f0; border: 2px solid #000; border-radius: 10px;">
    <h1>What happends inside a cruise control system?</h1>
    <p>An interactive visualization of the PID-controller</p>
</div>

<div style="text-align: center; margin-top: 100px; padding: 0px;">
    <svg></svg>
</div>
<!-- SVG container -->
