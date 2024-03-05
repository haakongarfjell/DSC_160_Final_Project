<script>
    import Katex from 'svelte-katex';
    import 'katex/dist/katex.min.css';

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
        <p1>Given a desired setpoint reference:</p1><Katex>{"r \\,"}</Katex><p1> and a measured output</p1><Katex>{"y \\,"}</Katex><p1>, the controller aims to adjust the control input</p1><Katex>{"u \\,"}</Katex><p1> so that the error </p1><Katex>{"e = r - y \\,"}</Katex><p1>approaches 0 in a both fast and controlled manner.<br>
        <p1>The PID controller is composed of three terms: the proportional term, the integral term, and the derivative term.</p1><br>
        <h2>Proportional term</h2>
        <p1>The proportional term of a PID-controller aims to correct the error between the reference and output, by adding a constant scale factor to the input</p1><br>
        <Katex displayMode>{"u = K_p e"}</Katex>
        <h2>Integral term</h2>
        <p1>The integral term is proportional to the accumulated error over time and is used to eliminate any residual error that remains after the proportional term has been applied.</p1><br>
        <Katex displayMode>{"u = K_i \\int_0^t e(\\tau) \\, d\\tau"}</Katex>
        <h2>Derivative term</h2>
        <p1>The derivative term is proportional to the rate of change of the error and is used to anticipate future error trends and provide a damping effect on the system.</p1><br>
        <Katex displayMode>{"u = K_d \\frac{de}{dt}"}</Katex>
        <h2>Combining the terms</h2>
        <p1>By combining the three terms we create a PID-controller where the parameters can be tuned to achieve the desired behavior of our system</p1><br>
        <Katex displayMode>{"u = K_p e + K_i \\int_0^t e(\\tau) \\, d\\tau + K_d \\frac{de}{dt}"}</Katex>
        <p1>Explore how the controller can be applied to a dynamical system by clicking the arrows, or jump to the parts you find most interesting!</p1><br>

        <!-- <p1>Writeup</p1><br>
        <p1>This is a prototype, future steps will describe the dynamic system, P-controller, PI-controller, PD-controller, and then PID-controller as well as demonstrating its robustness to noise</p1><br>
        <p1>So far we have thought about how we can structure a fun and engaging visual explanation of how one of our favorite controllers from control theory works.</p1><br>
        <p1>We landed on creating an interactive example of cruise control on a car as PID is widely used for this, and it is an understandable entry level example for non-control system engineers</p1><br>
        <p1>So far we have made a working prototype where a user can interact with the PID controller by adjusting gains to the controller.</p1><br>
        <p1>Also a model of the car is shown and a set up for the rest of the project with connecting pages through buttons.</p1><br>
        <p1>The most challening part will be to create an interactive visualisation of the car model (page1). Also describing the robostness to noise can be somewhat challenging to visualize</p1><br>
        <p1> This will be done by interacing with the equations with the mouse. Also describing the robostness to noise can be somewhat challenging to visualize. We are thinking of letting the user adding a hill to the simulations to make it realistic. </p1> -->

    </div><br>
    <!-- <img src = "https://www.thorlabs.com/images/TabImages/PID2.jpg" alt = "PID controller" width = "800" height = "300"> -->
</div>
