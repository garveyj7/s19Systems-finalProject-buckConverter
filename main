// MAIN CODE FOR BUCK CONVERTER

breaklines = true;
double Kp = 5;
//Proportional constant value

double DutyCycle = 127;
//Sets initial duty cycle value to 50%

const int numReadings = 5;
//Number of values sampled by moving average filter

int readings[numReadings];
// Loads and stores into an array the size of numReadings

int readIndex = 0;
// Sets the index of the current array to zero

int total = 0;
// Sets initial total to zero

int average = 0;
// Sets initial average to zero

int loadvoltage = A0;
//Sets analog pin 0 to load voltage

void setup() {
  Serial.begin(115200); \\Establishes serial communication
  TCCR0B = TCCR0B & B11111000 | B00000010;
  //Sets output frequency to 7912Hz.
  
    for (int thisReading = 0; thisReading < numReadings; thisReading++) {
    readings[thisReading] = 0; 
    // Iterates through the array and sets each value to zero.
    }
}

void loop() {
  
  double LoadVoltage = double(analogRead(A0)) * .005 * 2.12; 
  // Converts ADC value to respective voltage value.
  
  total = total - readings[readIndex];
  // Removes first value in the array and redefines total.
  
  readings[readIndex] = analogRead(A1);
  // Takes reading from the load voltage and adds it to the array.
  
  total = total + readings[readIndex];
  // Advance to the next position in the array and take new total.
  
  readIndex = readIndex + 1;
  // Iterates readindex value by 1.

  if (readIndex >= numReadings) {
    // Checks to see if readindex is equal or larger 
    // to the number of readings.
    
    readIndex = 0;
    // Ff it is, reset back to beginning of the array.
  }
  average = total / numReadings;
  // Calculates average from the total values and number of readings.
  
  double DesiredVoltage = average * .005 * 2.12;
  // Converts the desired voltage ADC value to a voltage value.
  
  double NewError = LoadVoltage - DesiredVoltage;
  // Takes the difference between the load voltage and desired voltage.
  
  double ProportionalError = Kp * NewError;
  // Generates the proportional error by multiplying the previous error
  // and a defined proportional constant value.
  
  double Correction = ProportionalError;
  // Sets the pwm correction value to the
  // calculated proportional error value.
  
  DutyCycle = DutyCycle - Correction;
  //Sets the duty cycle to the difference between the
  //previous duty cycle and the correction value.
  
  if (DutyCycle > 255)
  {
    DutyCycle = 255;
  }
  else if (DutyCycle < 0)
  {
    DutyCycle = 0;
  }
  //Restricts the duty cycle between 0 and 255.
  
  analogWrite(5, int(DutyCycle));
  //Outputs the duty cycle to Digital Pin 5.
  
  Serial.println((int)DutyCycle);
  //Prints duty cycle value to serial monitor.
  delay(5);
  
}
