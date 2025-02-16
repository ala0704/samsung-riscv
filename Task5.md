# OVERVIEW #

 This project is built using an ultrasonic sensor for the purpose of measurement of distance from the sensor and controls a buzzer based on object proximity with respect to the ultrasonic sensor. The TRIG_PIN sends a ultrasonic pulse, while the ECHO_PIN receives the wave and the reflection time to calculate distance. If an object is within 10 cm range, the buzzer starts buzzing continuously; otherwise, it remains OFF. The use of pulseIn() ensures fast and accurate measurements, while a 1ms timeout prevents delays, making the system highly responsive to the environment its present in.
This system is useful for object detection, obstacle avoidance, and safety applications. It can be implemented in navigation of robotics by obstacle detection, smart parking systems, automated doors, near live power supplies to drive animals away and security alarms to detect nearby objects and trigger actions accordingly. The fast response ensures quick decision-making, making it ideal for critical applications and triggering safety measures. The project runs on the CH32V003F4U6 microcontroller, which features a 32-bit RISC-V core based on the RV32EC instruction set, ensuring efficient performance and low power consumption.

# COMPONENTS REQUIRED TO BUILD SMART ULTRASONIC PROXIMITY DETECTOR WITH BUZZER ALERT: #
* VSD SQUADRON MINI (CH32V003F4U6 chip with 32-bit RISC-V core based on RV32EC instruction set)

* HC-SRO4 ultrasonic sensor

* Bread Board

* Jumper Wires

*  Buzzer 

The SMART ULTRASONIC PROXIMITY DETECTOR WITH BUZZER ALERT offers precise distance measurement and quick response using essential components. The VSD SQUADRON MINI, driven by the CH32V003F4U6 microcontroller (32-bit RISC-V core, RV32EC instruction set), efficiently processes sensor data and manages outputs. The HC-SR04 ultrasonic sensor detects object proximity by measuring sound wave time delay. A breadboard allows solderless circuit assembly, and jumper wires ensure seamless connections. This creates a fast, reliable proximity detection system suitable for automation and safety applications.



# PIN CONNECTIONS #
HC-SRO4 sensor | CH32V003x Pin
---------------|---------------
VCC            |   3.3V
Trigger        |   PD2
Echo           |   PD5
Ground         |  Ground

Buzzer         |  CH32V003x Pin
---------------|--------
Positive Lead  |  PD6
Negtive Lead   |  Ground

# UNDERSTANDING THE CODE #
The code utilizes an ultrasonic sensor to measure the distance to an object and controls a buzzer based on the measured distance. In the `setup()` function, the required pins are initialized: `TRIG_PIN` (PD2) for triggering the ultrasonic pulse, `ECHO_PIN` (PD5) for receiving the echo, and `BUZZER_PIN` (PD6) for controlling the buzzer. Within the `loop()` function, the `TRIG_PIN` sends a pulse to the ultrasonic sensor, which travels until it encounters an object and reflects back. The `ECHO_PIN` receives this reflected pulse, and the time it takes for the pulse to return is measured.

This time is then used to calculate the distance between the sensor and the object using the formula: distance = (speed of sound * time) / 2. If the distance is below 10 cm, the `BUZZER_PIN` is set to HIGH, activating the buzzer to alert the presence of the object. If the distance exceeds 10 cm, the buzzer is turned off by setting the `BUZZER_PIN` to LOW.

The process of measuring the distance and controlling the buzzer is repeated every 500 milliseconds, allowing continuous monitoring of the proximity of objects. This setup can be employed in various applications such as obstacle detection systems, distance measurement, or proximity sensing, providing real-time feedback to users based on the closeness of nearby objects. The system is efficient for alerting when objects come within a predefined range, enhancing automation or safety in specific scenarios.

# PROGRAM CODE #


    #define TRIG_PIN PD2  
    #define ECHO_PIN PD5  
    #define BUZZER_PIN  PD6  

    void setup() {
        pinMode(TRIG_PIN, OUTPUT);
        pinMode(ECHO_PIN, INPUT);
        pinMode(BUZZER_PIN, OUTPUT);
    }
    
    void loop() {
        uint16_t duration;
        int distance;

     //Send trigger pulse
    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);

     //Wait for Echo pin (with timeout)
    unsigned long startTime = millis();
    while (digitalRead(ECHO_PIN) == LOW) {
        if (millis() - startTime > 100) return; //  Exit if timeout
    }

     //  Measure Echo duration
      duration = 0;
      while (digitalRead(ECHO_PIN) == HIGH) {
          duration++;
          delayMicroseconds(1);
    }

    // Convert to distance (integer math)
    distance = (duration * 34)  /2000;

    // BUZZER logic
    if (distance > 0 && distance <= 10) {
        digitalWrite(BUZZER_PIN, HIGH);
    } else {
        digitalWrite(BUZZER_PIN, LOW);
    }

    delay(500);
    }



