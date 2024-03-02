<script>
    // You can add logic specific to Page1 here
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
</style>
    
<div style="text-align: center;">
    <div>
        <h1>How does a PID controller work?</h1>
        <p1>The proportional-integral-derivative controller (PID controller) is a widely used control system for industrial processes.</p1><br>
        <p1>It is a feedback control system that continuously calculates an error value as the difference between a desired setpoint and a measured process variable.</p1><br>
        <p1>The controller attempts to minimize the error by adjusting the process control inputs.</p1><br>
        <p1>The PID controller is composed of three terms: the proportional term, the integral term, and the derivative term.</p1><br>
        <p1>The proportional term is proportional to the current error value and is used to provide an immediate response to the error.</p1><br>
        <p1>The integral term is proportional to the accumulated error over time and is used to eliminate any residual error that remains after the proportional term has been applied.</p1><br>
        <p1>The derivative term is proportional to the rate of change of the error and is used to anticipate future error trends and provide a damping effect on the system.</p1><br>
        <p1>This visualization aims to demonstrate the effects of how a PID controller can be implemented as a simple cruise control system</p1><br>
        <p1>Click on the buttons below to navigate through the visualization</p1><br><br><br>
        
        <p1>Writeup</p1><br>
        <p1>This is a prototype, future steps will describe the dynamic system, P-controller, PI-controller, PD-controller, and then PID-controller as well as demonstrating its robustness to noise</p1><br>
        <p1>So far we have thought about how we can structure a fun and engaging visual explanation of how one of our favorite controllers from control theory works</p1><br>
        <p1>We landed on creating an interactive example of cruise control on a car as PID is widely used for this, and it is an understandable entry level example for non-control system engineers</p1><br>
        <p1>The most challening part will be to create an interactive visualisation of the car model (page1). Also describing the robostness to noise can be somewhat challenging to visualize</p1><br>

    </div><br>
    <img src = "https://www.thorlabs.com/images/TabImages/PID2.jpg" alt = "PID controller" width = "800" height = "300">
</div>
