# P22_ECU_Telemetry
ECU and Telemetry System for Prom Racing's P22

# Introduction
During my 3rd year of studies I joined PROM Racing. After my first year with the team where I came to understand how an electric formula car is made I finally had the chance to to design one myself. 

As an electrical and computer engineer with a focus on hardware systems and electronics the idea of designing and assembling a motorsports grade ECU was a very exciting project, and when the time came for me to finally start working on the project I was...very(you have no idea!)excited. 

# Conceptual Phase

Desingning an electric vehicle and especially a race car is no simple task, and even though the design of PROM's previous car was well thought, it was still the team's first ever electric vehicle. Therefore, there was a good reference but there was still a long way to go!

Since the previous vehicle was based on a distributed model its ECU was extremely simple, and for me this was unacceptable!
My desire was to build an extremely robust and powerful ECU which would be the center of the car. 
And thus started a long proccess of carefully exmaning all of my options and fine tuning the architecture of the vehicle's electronics

# The Final Concept

After months of planning and preperation I had finally come up with the final concept of the vehicle's architecture and consequently the ECUs main functionalities.
Instead of a distributed model a more centralized one would be used. At the center of it all we would have the vehicles ECU.. A mixture of analog(sensor)signals and digital signals would be sampled by the ECU and three different CAN Buses would be utilized in order for it to communicate with the other systems of the vehicle.
