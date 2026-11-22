(Very likely to be moved to control.md later.) 
# Safety Interlocks
Below are the corresponding system responses that occur when the appropriate Critical I/O limit is reached or becomes unresponsive for >10 seconds.

### Control

| Critical I/O | Critical I/O Meaning |System Response |
| ------------ | ------------- | ------------- |
| TBD | Emergency stop is activated | Immediately cut the power to the motor and cutterhead by breaking the hardwired safety relay loop, and apply power-off brakes to cutterhead through a Safe Torque Off function. Reset then required. |
| TBD | Motor exceeds appropriate temperature level | Cease cutterhead operation, trip motor, send alert and require a manual reset/acknowledgement before system can be reused.  |
| TBD | Drill RPM exceeds appropriate amount | Stop cutter. |
| TBD | Current levels are outside of set range or current tracking has been unresponsive beyond allotted time | trip motor, send alert and require a manual reset/acknowledgement before system can be reused.  |
| TBD | Travel limit exceeded | Prevent movement in particular direction, send alert. |
| TBD | Main power lost | Enter Lockout state until system is manually reset.  |
| TBD | Machine inclination exceeds limit | Pause movement in particular direction, send alert.  |
| TBD | Communication loss | Emergency stop triggers to prevent blind drilling  |


# Manual Fallbacks Definition 

Manual fallback occurs when a sensor experiences a fault or invalid data. Feedback data validity can be detected through either monitoring sensor current, setting appropriate value range limits, or a communication watchdog. If the relevant sensor feedback fails, certain systems, such as the main drive or the thrust system can remain operational under a controlled manual open-loop mode. In this mode, a user/operator will be able to send limited control commands to these systems without automatic feedback regulation. 

Entering this mode will require deliberate action from the user rather than a spontaneous switch to the mode when feedback failure occurs. Systems that cannot remain operational after feedback failure because of the safety risks that may occur will follow their respective safety response protocol. 

If accurate feedback returns, the user will be able to return to the standard automated mode. This switch must be deliberate, and requires acknowledgement from the user before resumption. 

Systems suitable for this mode remain subject to change at this time.

# Fault Recovery Logic Definition

The system transitions to an E-STOPPED state after power loss. To do this, power is cut off from machine, and cannot be returned until two inputs are given.

### Input 1: Safety reset
Reset once the power is returned. Occurs after user releases the E-Stop button, where the machine remains in a LOCKOUT state. After reset, motors remain turned off for now.

### Input 2: Reenable system/ Safe Idle
The system is initialized by the user through the GUI. Once remaining interlocks are satisfied and sensor feedback returns, system can be manually returned into a READY state.

### Recovering Sensor States
During recovery after power loss, sensors will need to recover and reflect the state of the current machine. This means restoring sensor data prior to the loss of power alone is not sufficient for some sensors. This is dependent on sensor choice, and as of right now the current chosen sensors will be able to collect new data without any issue or need for recalibration. This is subject to change.

The following sensors will collect new data upon restart.
+Hall effect sensors
+(Absolute) magnetic linear encoder
+VFD telemetry (if available)
+PT100/PT1000 RTD
+Mechanical safety-rated limit switch/inductive proximity sensor
Industrial 2-axis inclinometer

