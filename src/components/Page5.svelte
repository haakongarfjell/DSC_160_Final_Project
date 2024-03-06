<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    
    // Define simulation parameters
    let initial_velocity = 100;
    let setpoint_velocity = 50;
    let control_on_time = 50;
    let Kd = 0.01;
    let Kp = 0.1;
    let Ki = 1;
    
    // Define PID controller function
    function PID_controller(K, current_velocity, setpoint_velocity, errors, currentTime, controlOnTime) {
        const [Kp, Ki, Kd] = K;

        const error = setpoint_velocity - current_velocity;
        let D = 0; // Initialize derivative term to 0 initially

        // Only calculate the derivative term if the current time is after the control on time
        if (currentTime > controlOnTime + 1.0) {
            D = -12*Kd * (error - errors[errors.length - 1]);
        }

        const P = Kp * error;
        const I = Ki * errors.reduce((acc, curr) => acc + curr, 0);

        const control_signal = P + I + D;
        return [control_signal, error];
    }

    // Define the differential equation of the system
    function dv_dt(current_velocity, control_signal) {
        const mass = 100;  // Mass of car in kg
        const friction_coefficient = 20;  // Friction coefficient in Ns/m
        const acceleration = (control_signal - friction_coefficient * current_velocity) / mass;
        return acceleration;
    }
    
    // Euler method for numerical integration
    function simulate(initial_velocity, setpoint_velocity, control_on_time, K) {
        const initial_time = 0;
        const final_time = 100;
        const dt = 0.1;  // time step
        const time = [];
        const velocity = [];
        const errors = [0];
        let control_signal = 0;
        let current_velocity = initial_velocity;
        let error; // Declare error variable here
    
        // Euler integration
        for (let t = initial_time; t <= final_time; t += dt) {
          time.push(t);
          if (t > control_on_time) {
            [control_signal, error] = PID_controller(K, current_velocity, setpoint_velocity, errors, t, control_on_time);
            errors.push(error);
          }
          velocity.push(current_velocity);
          const acceleration = dv_dt(current_velocity, control_signal);
          current_velocity += acceleration * dt;
        }
        return [velocity, time];
    }
    
    // Function to plot the simulation data using D3
    function plotData() {
      const margin = { top: 50, right: 100, bottom: 70, left: 70 };
      const width = 800 - margin.left - margin.right;
      const height = 500 - margin.top - margin.bottom;
    
      const svg = d3.select('#plot')
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform', `translate(${margin.left},${margin.top})`);
    
      const x = d3.scaleLinear().domain([0, 100]).range([0, width]);
      const y = d3.scaleLinear().domain([0, d3.max(simulationData[0])]).nice().range([height, 0]);
    
      const line = d3.line()
        .x((d, i) => x(simulationData[1][i]))
        .y(d => y(d));
    
      // Append the title
      svg.append("text")
        .attr("x", width / 2)
        .attr("y", -20)
        .attr("text-anchor", "middle")
        .style("font-size", "18px")
        .style("font-weight", "bold")
        .style("font-family", "Arial, sans-serif")
        .style("fill", "black")
        .style("text-transform", "capitalize")
        .text("Cruise Control by Using PID controller");
    
      // Append the line for simulation data
      svg.append('path')
        .datum(simulationData[0])
        .attr('fill', 'none')
        .attr('stroke', 'steelblue')
        .attr('stroke-width', 3) // Adjust line size
        .attr('d', line);
    
      // Append the green horizontal dotted line for setpoint_velocity
      svg.append('line')
        .attr('x1', 0)
        .attr('x2', width)
        .attr('y1', y(setpoint_velocity))
        .attr('y2', y(setpoint_velocity))
        .attr('stroke', 'green')
        .attr('stroke-width', 2)
        .attr('stroke-dasharray', '5,5');
    
      // Append the vertical red dotted line for control_on_time
      svg.append('line')
        .attr('x1', x(control_on_time))
        .attr('x2', x(control_on_time))
        .attr('y1', 0)
        .attr('y2', height)
        .attr('stroke', 'red')
        .attr('stroke-width', 2)
        .attr('stroke-dasharray', '5,5');
    
      // Define legends data
      const legends = [
        { label: "Car Speed", color: "steelblue" , dasharray: "1,0"},
        { label: "Setpoint Velocity", color: "green", dasharray: "5,5" },
        { label: "Cruise Control ON", color: "red", dasharray: "5,5" }
      ];
    
      // Append legends
      svg.selectAll(".legend")
        .data(legends)
        .enter()
        .append("text")
        .attr("x", width - 125)
        .attr("y", (d, i) => 25 + i * 20)
        .attr("text-anchor", "start")
        .text(d => d.label)
        .attr("alignment-baseline", "middle")
        .attr("font-size", "16px")
        .attr("fill", d => d.color);
    
      // Append lines for legends
      svg.selectAll(".legend-line")
        .data(legends)
        .enter()
        .append("line")
        .attr("x1", width - 150 + 2)
        .attr("y1", (d, i) => 24 + i * 20)
        .attr("x2", width - 130)
        .attr("y2", (d, i) => 24 + i * 20)
        .attr("stroke", d => d.color)
        .attr("stroke-width", 2)
        .attr("stroke-dasharray", d => d.dasharray || "1,0");
    
      // Append X axis
      svg.append("g")
        .attr("transform", "translate(0," + height + ")")
        .call(d3.axisBottom(x))
        .append("text")
        .attr("x", width / 2)
        .attr("y", 20)
        .attr("dy", "1em")
        .attr("text-anchor", "middle")
        .text("Time (s)")
        .style("fill", "black")
        .style("font-size", "16px"); // Adjust axis label size
        
      // Append Y axis
      svg.append("g")
        .call(d3.axisLeft(y))
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("y", -margin.left + 10)
        .attr("x", -height / 2)
        .attr("dy", "1em")
        .attr("text-anchor", "middle")
        .text("Velocity (m/s)")
        .style("fill", "black")
        .style("font-size", "16px"); // Adjust axis label size
    
      // Add gridlines
      svg.append("g")
        .attr("class", "grid")
        .attr("transform", "translate(0," + height + ")")
        .call(d3.axisBottom(x).tickSize(-height).tickFormat(""))
        .selectAll(".tick")
        .classed("minor", d => d % 10 !== 0)
        .selectAll("line")
        .style("stroke-opacity", 0.2);
    
      svg.append("g")
        .attr("class", "grid")
        .call(d3.axisLeft(y).tickSize(-width).tickFormat(""))
        .selectAll(".tick")
        .classed("minor", d => d % 10 !== 0)
        .selectAll("line")
        .style("stroke-opacity", 0.2);
    }
   
    // Function to update simulation data with new Ki value
    function updateParameters() {
        simulationData = simulate(initial_velocity, setpoint_velocity, control_on_time, [Kp, Ki, Kd]);
        d3.select('#plot').select('svg').remove();
        plotData();
    }

    function updateKp(event) {
        Kp = +event.target.value; 
        updateParameters();
    }

    function updateKi(event) {
        Ki = +event.target.value; 
        updateParameters();
    }

    function updateKd(event) {
        Kd = +event.target.value; 
        updateParameters();
    }
    
    // Define simulation data
    let simulationData = [[], []];
    onMount(() => {
        simulationData = simulate(initial_velocity, setpoint_velocity, control_on_time, [Kp, Ki, Kd]);
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

<div id="plot">
    <!-- Plot will be rendered here -->
</div>

<!-- Sliders for adjusting parameters -->
<div style="text-align: center; margin-top: 20px;">
    <label for="Ki">Ki:</label>
    <input type="range" id="Ki" name="Ki" min="0" max="5" step="0.1" bind:value={Ki} on:input={updateParameters}>
    <span>{Ki}</span>

    <label for="Kp" style="margin-left: 20px;">Kp:</label>
    <input type="range" id="Kp" name="Kp" min="0" max="50" step="1" bind:value={Kp} on:input={updateParameters}>
    <span>{Kp}</span>

    <label for="Kd" style="margin-left: 20px;">Kd:</label>
    <input type="range" id="Kd" name="Kd" min="0" max="50" step="1" bind:value={Kd} on:input={updateParameters}>
    <span>{Kd}</span>
</div>

<div style="text-align: center; margin-top: 20px;">
    <label for="initial_velocity">Initial Velocity:</label>
    <input type="range" id="initial_velocity" name="initial_velocity" min="0" max="200" step="1" bind:value={initial_velocity} on:input={updateParameters}>
    <span>{initial_velocity}</span>

    <label for="setpoint_velocity" style="margin-left: 20px;">Set Velocity:</label>
    <input type="range" id="setpoint_velocity" name="setpoint_velocity" min="0" max="100" step="1" bind:value={setpoint_velocity} on:input={updateParameters}>
    <span>{setpoint_velocity}</span>

    <label for="control_on_time" style="margin-left: 20px;">Cruise Control On:</label>
    <input type="range" id="control_on_time" name="control_on_time" min="10" max="90" step="1" bind:value={control_on_time} on:input={updateParameters}>
    <span>{control_on_time}</span>
</div>



<div style="text-align: center; margin-top: 20px; margin-left: 500px; margin-right: 500px;  padding: 20px; background-color: #f0f0f0; border: 2px solid #000; border-radius: 10px;">
  <h1>The complete PID-Controller</h1>
    <p>Lets experiment with how the PID-controller affects the cruise control model by including the D-term. This gives us a little more flexibility in order to reduce oscillations. Adjust the proportional (Kp), integral (Ki), and derivative (Kd) terms, along with the setpoint velocity and initial velocity when cruise control is activated.</p>
    <p>Test your control engineer abilities by making the system critically dampened for various conditions of initial velocities and setpoint values! </p>
</div>

<p></p><br>
<p></p><br>
