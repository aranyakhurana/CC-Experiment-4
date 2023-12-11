/*
 * MIDIUSB_test.ino
 *
 * Created: 4/6/2015 10:47:08 AM
 * Reference: Gurbrinder Grewal
 * Modified by Arduino LLC (2015)
 * 
 * Modified by Aranya Khurana (2023)
 * Replaced delay with millis for time for responsiveness
 * millis reference found at https://forum.arduino.cc/t/using-millis-for-timing-a-beginners-guide/483573
 * used ChatGPT for millis code confirmation
 *
 */

#include "MIDIUSB.h"

#define DEBOUNCE 100  // delay for ensuring that the button presses do not overlap

int sensor_1_Pin = A0;  // input pin for analog sensor 1
int sensor_2_Pin = A1;  // input pin for analog sensor 2
int sensor_3_Pin = A2;  // input pin for analog sensor 3
int sensor_4_Pin = A3;  // input pin for analog sensor 4
int sensor_5_Pin = A4;  // input pin for analog sensor 5
int sensor_6_Pin = A5;  // input pin for analog sensor 6
int sensor_7_Pin = A6;  // input pin for analog sensor 7
int sensor_8_Pin = A7;  // input pin for analog sensor 8


// these thresholds below are based on testing and different on purpose to account for variation:

int sensor_1_Threshold = 300;  // threshold value for sensor 1
int sensor_2_Threshold = 450;  // threshold value for sensor 2
int sensor_3_Threshold = 450;  // threshold value for sensor 3
int sensor_4_Threshold = 450;  // threshold value for sensor 4
int sensor_5_Threshold = 600;  // threshold value for sensor 5
int sensor_6_Threshold = 500;  // threshold value for sensor 6
int sensor_7_Threshold = 500;  // threshold value for sensor 7
int sensor_8_Threshold = 550;  // threshold value for sensor 8


// Variables that will change

int sensor_1_Value = 0;  // variable to hold sensor 1 value
int sensor_2_Value = 0;  // variable to hold sensor 2 value
int sensor_3_Value = 0;  // variable to hold sensor 3 value
int sensor_4_Value = 0;  // variable to hold sensor 4 value
int sensor_5_Value = 0;  // variable to hold sensor 5 value
int sensor_6_Value = 0;  // variable to hold sensor 6 value
int sensor_7_Value = 0;  // variable to hold sensor 7 value
int sensor_8_Value = 0;  // variable to hold sensor 8 value

int note_1_Velocity;  // variable to store note 1 velocity
int note_2_Velocity;  // variable to store note 2 velocity
int note_3_Velocity;  // variable to store note 3 velocity
int note_4_Velocity;  // variable to store note 4 velocity
int note_5_Velocity;  // variable to store note 5 velocity
int note_6_Velocity;  // variable to store note 6 velocity
int note_7_Velocity;  // variable to store note 7 velocity
int note_8_Velocity;  // variable to store note 8 velocity

// Generally, you should use "unsigned long" for variables that hold time
// The value will quickly become too large for an int to store
unsigned long lastButtonPressTime_1 = 0;  // will store last time Sensor 1 was was updated
unsigned long lastButtonPressTime_2 = 0;  // will store last time Sensor 2 was was updated
unsigned long lastButtonPressTime_3 = 0;  // will store last time Sensor 3 was was updated
unsigned long lastButtonPressTime_4 = 0;  // will store last time Sensor 4 was was updated
unsigned long lastButtonPressTime_5 = 0;  // will store last time Sensor 5 was was updated
unsigned long lastButtonPressTime_6 = 0;  // will store last time Sensor 6 was was updated
unsigned long lastButtonPressTime_7 = 0;  // will store last time Sensor 7 was was updated
unsigned long lastButtonPressTime_8 = 0;  // will store last time Sensor 8 was was updated


// MIDI Basics:
// First parameter is the event type (0x09 = note on, 0x08 = note off).
// Second parameter is note-on/note-off, combined with the channel.
// Channel can be anything between 0-15. Typically reported to the user as 1-16.
// Third parameter is the note number (48 = middle C).
// Fourth parameter is the velocity (64 = normal, 127 = fastest).

void noteOn(byte channel, byte pitch, byte velocity) {
  midiEventPacket_t noteOn = { 0x09, 0x90 | channel, pitch, velocity };
  MidiUSB.sendMIDI(noteOn);
}

void noteOff(byte channel, byte pitch, byte velocity) {
  midiEventPacket_t noteOff = { 0x08, 0x80 | channel, pitch, velocity };
  MidiUSB.sendMIDI(noteOff);
}

// First parameter is the event type (0x0B = control change).
// Second parameter is the event type, combined with the channel.
// Third parameter is the control number number (0-119).
// Fourth parameter is the control value (0-127).

void controlChange(byte channel, byte control, byte value) {
  midiEventPacket_t event = { 0x0B, 0xB0 | channel, control, value };
  MidiUSB.sendMIDI(event);
}

void setup() {
  Serial.begin(9600);
}

void loop() {

  // read the values from the sensors:
  sensor_1_Value = analogRead(sensor_1_Pin);
  sensor_2_Value = analogRead(sensor_2_Pin);
  sensor_3_Value = analogRead(sensor_3_Pin);
  sensor_4_Value = analogRead(sensor_4_Pin);
  sensor_5_Value = analogRead(sensor_5_Pin);
  sensor_6_Value = analogRead(sensor_6_Pin);
  sensor_7_Value = analogRead(sensor_7_Pin);
  sensor_8_Value = analogRead(sensor_8_Pin);

  // map note velocities to sensor values
  note_1_Velocity = map(sensor_1_Value, sensor_1_Threshold, 1023, 127, 64);
  note_2_Velocity = map(sensor_2_Value, sensor_2_Threshold, 1023, 64, 127);
  note_3_Velocity = map(sensor_3_Value, sensor_3_Threshold, 1023, 64, 127);
  note_4_Velocity = map(sensor_4_Value, sensor_4_Threshold, 1023, 64, 127);
  note_5_Velocity = map(sensor_5_Value, 100, 1023, 127, 64);
  note_6_Velocity = map(sensor_6_Value, sensor_6_Threshold, 1023, 64, 127);
  note_7_Velocity = map(sensor_7_Value, sensor_7_Threshold, 1023, 64, 127);
  note_8_Velocity = map(sensor_8_Value, sensor_8_Threshold, 1023, 64, 127);


  // // sensorValues printing for debugging; comment in/out as needed
  //   Serial.print(sensor_1_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_2_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_3_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_4_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_5_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_6_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_7_Value);
  //   Serial.print("\t\t");
  //   Serial.print(sensor_8_Value);
  //   Serial.println("");

  // // noteVelocities printing for debugging; comment in/out as needed
  // Serial.print(note_1_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_2_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_3_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_4_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_5_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_6_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_7_Velocity);
  // Serial.print("\t\t");
  // Serial.print(note_8_Velocity);
  // Serial.println("");




  // The sensors are programmed to work as follows:
  // Sensor 1: MIDI CC (needs to be mapped in Ableton Live using MIDI Learn)
  // Sensor 2: MIDI Notes
  // Sensor 3: MIDI Notes
  // Sensor 4: MIDI Notes
  // Sensor 5: MIDI CC (needs to be mapped in Ableton Live using MIDI Learn)
  // Sensor 6: MIDI Notes
  // Sensor 7: MIDI Notes
  // Sensor 8: MIDI Notes




  // Code for Sensor 1:
  // This will send MIDI CC messages

  if (sensor_1_Value >= sensor_1_Threshold) {
    if (millis() - lastButtonPressTime_1 >= DEBOUNCE) {
      controlChange(0, 11, note_1_Velocity);  // Channel 0, controller 11, value = velocity
      Serial.print("Note_1 CC");
      Serial.print(note_1_Velocity);
      Serial.print("\t");
      MidiUSB.flush();
      lastButtonPressTime_1 = millis();      // Update the last button press time
    }
  }

  else if (sensor_1_Value < sensor_1_Threshold) {
    if (millis() - lastButtonPressTime_1 >= DEBOUNCE) {
      controlChange(0, 11, 0);               // Channel 0, controller 11, value = 0
      Serial.print("Note_1 CC OFF");
      Serial.print("\t");
      MidiUSB.flush();
      lastButtonPressTime_1 = millis();      // Update the last button press time
    }
  }


  // Code for Sensor 2:
  // This will keep sending a note/chord while it is depressed and stop sending when released

  if (sensor_2_Value >= sensor_2_Threshold) {
    if (millis() - lastButtonPressTime_2 >= DEBOUNCE) {
      Serial.print("Note_2 ON");
      Serial.print("\t");
      noteOn(0, 60, note_2_Velocity);        // Channel 0,   note C4,    velocity = amount of pressure applied
      // noteOn(0, 62, note_2_Velocity);     // Channel 0,   note D4,    velocity = amount of pressure applied
      noteOn(0, 63, note_2_Velocity);        // Channel 0,   note D#4,   velocity = amount of pressure applied
      // noteOn(0, 65, note_2_Velocity);     // Channel 0,   note F4,    velocity = amount of pressure applied
      // noteOn(0, 67, note_2_Velocity);     // Channel 0,   note G4,    velocity = amount of pressure applied
      // noteOn(0, 68, note_2_Velocity);     // Channel 0,   note G#4,   velocity = amount of pressure applied
      MidiUSB.flush();
      lastButtonPressTime_2 = millis();      // Update the last button press time
    }
  }

  else if (sensor_2_Value < sensor_2_Threshold) {
    if (millis() - lastButtonPressTime_2 >= DEBOUNCE) {
      Serial.print("Note_2 OFF");
      Serial.print("\t");
      noteOn(0, 60, 0);                      // Channel 0,   note C4,    velocity = 0
      // noteOn(0, 62, 0);                   // Channel 0,   note D4,    velocity = 0
      noteOn(0, 63, 0);                      // Channel 0,   note D#4,   velocity = 0
      // noteOn(0, 65, 0);                   // Channel 0,   note F4,    velocity = 0
      // noteOn(0, 67, 0);                   // Channel 0,   note G4,    velocity = 0
      // noteOn(0, 68, 0);                   // Channel 0,   note G#4,   velocity = 0
      MidiUSB.flush();                       // Clear MIDI notes
      lastButtonPressTime_2 = millis();      // Update the last button press time
    }
  }



  if (sensor_3_Value >= sensor_3_Threshold) {
    if (millis() - lastButtonPressTime_3 >= DEBOUNCE) {
      Serial.print("Note_3 ON");
      Serial.print("\t");
      // noteOn(0, 60, note_3_Velocity);     // Channel 0,   note C4,    velocity = amount of pressure applied
      noteOn(0, 62, note_3_Velocity);        // Channel 0,   note D4,    velocity = amount of pressure applied
      // noteOn(0, 63, note_3_Velocity);     // Channel 0,   note D#4,   velocity = amount of pressure applied
      noteOn(0, 65, note_3_Velocity);        // Channel 0,   note F4,    velocity = amount of pressure applied
      // noteOn(0, 67, note_3_Velocity);     // Channel 0,   note G4,    velocity = amount of pressure applied
      // noteOn(0, 68, note_3_Velocity);     // Channel 0,   note G#4,   velocity = amount of pressure applied
      MidiUSB.flush();
      lastButtonPressTime_3 = millis();      // Update the last button press time
    }
  }

  else if (sensor_3_Value < sensor_3_Threshold) {
    if (millis() - lastButtonPressTime_3 >= DEBOUNCE) {
      Serial.print("Note_3 OFF");
      Serial.print("\t");
      // noteOn(0, 60, 0);                   // Channel 0,   note C4,    velocity = 0
      noteOn(0, 62, 0);                      // Channel 0,   note D4,    velocity = 0
      // noteOn(0, 63, 0);                   // Channel 0,   note D#4,   velocity = 0
      noteOn(0, 65, 0);                      // Channel 0,   note F4,    velocity = 0
      // noteOn(0, 67, 0);                   // Channel 0,   note G4,    velocity = 0
      // noteOn(0, 68, 0);                   // Channel 0,   note G#4,   velocity = 0
      MidiUSB.flush();
      lastButtonPressTime_3 = millis();      // Update the last button press time
    }
  }



  if (sensor_4_Value >= sensor_4_Threshold) {
    if (millis() - lastButtonPressTime_4 >= DEBOUNCE) {
      Serial.print("Note_4 ON");
      Serial.print("\t");
      // noteOn(0, 60, note_4_Velocity);     // Channel 0,   note C4,    velocity = amount of pressure applied
      // noteOn(0, 62, note_4_Velocity);     // Channel 0,   note D4,    velocity = amount of pressure applied
      noteOn(0, 63, note_4_Velocity);        // Channel 0,   note D#4,   velocity = amount of pressure applied
      // noteOn(0, 65, note_4_Velocity);     // Channel 0,   note F4,    velocity = amount of pressure applied
      noteOn(0, 67, note_4_Velocity);        // Channel 0,   note G4,    velocity = amount of pressure applied
      // noteOn(0, 68, note_4_Velocity);     // Channel 0,   note G#4,   velocity = amount of pressure applied
      MidiUSB.flush();
      lastButtonPressTime_4 = millis();      // Update the last button press time
    }
  }

  else if (sensor_4_Value < sensor_4_Threshold) {
    if (millis() - lastButtonPressTime_4 >= DEBOUNCE) {
      Serial.print("Note_4 OFF");
      Serial.print("\t");
      // noteOn(0, 60, 0);                   // Channel 0,   note C4,    velocity = 0
      // noteOn(0, 62, 0);                   // Channel 0,   note D4,    velocity = 0
      noteOn(0, 63, 0);                      // Channel 0,   note D#4,   velocity = 0
      // noteOn(0, 65, 0);                   // Channel 0,   note F4,    velocity = 0
      noteOn(0, 67, 0);                      // Channel 0,   note G4,    velocity = 0
      // noteOn(0, 68, 0);                   // Channel 0,   note G#4,   velocity = 0
      MidiUSB.flush();
      lastButtonPressTime_4 = millis();      // Update the last button press time
    }
  }


  if (sensor_5_Value >= sensor_5_Threshold) {
    if (millis() - lastButtonPressTime_5 >= DEBOUNCE) {
      controlChange(0, 12, note_5_Velocity);  // Channel 0, controller 12, value = velocity
      Serial.print("Note_5 CC");
      Serial.print(note_5_Velocity);
      Serial.print("\t");
      MidiUSB.flush();
      lastButtonPressTime_5 = millis();       // Update the last button press time
    }
  }

  else if (sensor_5_Value < sensor_5_Threshold) {
    if (millis() - lastButtonPressTime_5 >= DEBOUNCE) {
      controlChange(0, 12, 0);               // Channel 0, controller 12, value = 0
      Serial.print("Note_5 CC OFF");
      Serial.print("\t");
      MidiUSB.flush();
      lastButtonPressTime_5 = millis();      // Update the last button press time
    }
  }


  if (sensor_6_Value >= sensor_6_Threshold) {
    if (millis() - lastButtonPressTime_6 >= DEBOUNCE) {
      Serial.print("Note_6 ON");
      Serial.print("\t");
      noteOn(0, 60, note_6_Velocity);        // Channel 0,   note C4,    velocity = amount of pressure applied
      // noteOn(0, 62, note_6_Velocity);     // Channel 0,   note D4,    velocity = amount of pressure applied
      // noteOn(0, 63, note_6_Velocity);     // Channel 0,   note D#4,   velocity = amount of pressure applied
      noteOn(0, 65, note_6_Velocity);        // Channel 0,   note F4,    velocity = amount of pressure applied
      // noteOn(0, 67, note_6_Velocity);     // Channel 0,   note G4,    velocity = amount of pressure applied
      // noteOn(0, 68, note_6_Velocity);     // Channel 0,   note G#4,   velocity = amount of pressure applied
      MidiUSB.flush();
      lastButtonPressTime_6 = millis();      // Update the last button press time
    }
  }

  else if (sensor_6_Value < sensor_6_Threshold) {
    if (millis() - lastButtonPressTime_6 >= DEBOUNCE) {
      Serial.print("Note_6 OFF");
      Serial.print("\t");
      noteOn(0, 60, 0);                      // Channel 0,   note C4,    velocity = 0
      // noteOn(0, 62, 0);                   // Channel 0,   note D4,    velocity = 0
      // noteOn(0, 63, 0);                   // Channel 0,   note D#4,   velocity = 0
      noteOn(0, 65, 0);                      // Channel 0,   note F4,    velocity = 0
      // noteOn(0, 67, 0);                   // Channel 0,   note G4,    velocity = 0
      // noteOn(0, 68, 0);                   // Channel 0,   note G#4,   velocity = 0
      MidiUSB.flush();
      lastButtonPressTime_6 = millis();      // Update the last button press time
    }
  }


  if (sensor_7_Value >= sensor_7_Threshold) {
    if (millis() - lastButtonPressTime_7 >= DEBOUNCE) {
      Serial.print("Note_7 ON");
      Serial.print("\t");
      // noteOn(0, 60, note_7_Velocity);     // Channel 0,   note C4,    velocity = amount of pressure applied
      noteOn(0, 62, note_7_Velocity);        // Channel 0,   note D4,    velocity = amount of pressure applied
      // noteOn(0, 63, note_7_Velocity);     // Channel 0,   note D#4,   velocity = amount of pressure applied
      // noteOn(0, 65, note_7_Velocity);     // Channel 0,   note F4,    velocity = amount of pressure applied
      noteOn(0, 67, note_7_Velocity);        // Channel 0,   note G4,    velocity = amount of pressure applied
      // noteOn(0, 68, note_7_Velocity);     // Channel 0,   note G#4,   velocity = amount of pressure applied
      MidiUSB.flush();
      lastButtonPressTime_7 = millis();      // Update the last button press time
    }
  }

  else if (sensor_7_Value < sensor_7_Threshold) {
    if (millis() - lastButtonPressTime_7 >= DEBOUNCE) {
      Serial.print("Note_7 OFF");
      Serial.print("\t");
      // noteOn(0, 60, 0);                   // Channel 0,   note C4,    velocity = 0
      noteOn(0, 62, 0);                      // Channel 0,   note D4,    velocity = 0
      // noteOn(0, 63, 0);                   // Channel 0,   note D#4,   velocity = 0
      // noteOn(0, 65, 0);                   // Channel 0,   note F4,    velocity = 0
      noteOn(0, 67, 0);                      // Channel 0,   note G4,    velocity = 0
      // noteOn(0, 68, 0);                   // Channel 0,   note G#4,   velocity = 0
      MidiUSB.flush();
      lastButtonPressTime_7 = millis();      // Update the last button press time
    }
  }


  if (sensor_8_Value >= sensor_8_Threshold) {
    if (millis() - lastButtonPressTime_8 >= DEBOUNCE) {
      Serial.print("Note_8 ON");
      Serial.print("\t");
      // noteOn(0, 60, note_8_Velocity);     // Channel 0,   note C4,    velocity = amount of pressure applied
      // noteOn(0, 62, note_8_Velocity);     // Channel 0,   note D4,    velocity = amount of pressure applied
      noteOn(0, 63, note_8_Velocity);        // Channel 0,   note D#4,   velocity = amount of pressure applied
      // noteOn(0, 65, note_8_Velocity);     // Channel 0,   note F4,    velocity = amount of pressure applied
      // noteOn(0, 67, note_8_Velocity);     // Channel 0,   note G4,    velocity = amount of pressure applied
      noteOn(0, 68, note_8_Velocity);        // Channel 0,   note G#4,   velocity = amount of pressure applied
      MidiUSB.flush();
      lastButtonPressTime_8 = millis();      // Update the last button press time
      Serial.println("");
    }
  }

  else if (sensor_8_Value < sensor_8_Threshold) {
    if (millis() - lastButtonPressTime_8 >= DEBOUNCE) {
      Serial.print("Note_8 OFF");
      Serial.print("\t");
      // noteOn(0, 60, 0);                   // Channel 0,   note C4,    velocity = 0
      // noteOn(0, 62, 0);                   // Channel 0,   note D4,    velocity = 0
      noteOn(0, 63, 0);                      // Channel 0,   note D#4,   velocity = 0
      // noteOn(0, 65, 0);                   // Channel 0,   note F4,    velocity = 0
      // noteOn(0, 67, 0);                   // Channel 0,   note G4,    velocity = 0
      noteOn(0, 68, 0);                      // Channel 0,   note G#4,   velocity = 0
      MidiUSB.flush();
      lastButtonPressTime_8 = millis();      // Update the last button press time
      Serial.println("");
    }
  }

  delay(DEBOUNCE / 5);                       // for stability
}
