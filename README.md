#define BUTTON_PIN A0
#define NUM_LEDS 12

int ledPins[NUM_LEDS] = {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
int buttonState = 0;
int lastButtonState = 0;
int patternIndex = 0;
bool debounce = false;

void setup() {
    pinMode(BUTTON_PIN, INPUT_PULLUP);
    for (int i = 0; i < NUM_LEDS; i++) {
        pinMode(ledPins[i], OUTPUT);
    }
}

void loop() {
    buttonState = digitalRead(BUTTON_PIN);
    if (buttonState == LOW && lastButtonState == HIGH) {
        delay(50);
        patternIndex = (patternIndex + 1) % 5;
    }
    lastButtonState = buttonState;

    switch (patternIndex) {
        case 0: patternBlinking(); break;
        case 1: patternSequence(); break;
        case 2: patternChaserSlow(); break;
        case 3: patternChaserFast(); break;
        case 4: patternSnake(); break;
        case 5: patternHalfOnOff(); break;
    }
}

void patternBlinking() {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < NUM_LEDS; j++) digitalWrite(ledPins[j], HIGH);
        delay(300);
        for (int j = 0; j < NUM_LEDS; j++) digitalWrite(ledPins[j], LOW);
        delay(300);
    }
}

void patternSequence() {
    for (int i = 0; i < NUM_LEDS; i++) {
        digitalWrite(ledPins[i], HIGH);
        delay(100);
        digitalWrite(ledPins[i], LOW);
    }
}

void patternChaserSlow() {
    for (int i = 0; i < NUM_LEDS; i++) {
        digitalWrite(ledPins[i], HIGH);
        if (i > 0) digitalWrite(ledPins[i - 1], LOW);
        delay(700);
    }
    digitalWrite(ledPins[NUM_LEDS - 1], LOW);
}

void patternChaserFast() {
    for (int i = 0; i < NUM_LEDS; i++) {
        digitalWrite(ledPins[i], HIGH);
        if (i > 0) digitalWrite(ledPins[i - 1], LOW);
        delay(100);
    }
    digitalWrite(ledPins[NUM_LEDS - 1], LOW);
}

void patternSnake() {
    for (int i = 0; i < NUM_LEDS; i++) {
        digitalWrite(ledPins[i], HIGH);
        delay(100);
    }
    for (int i = NUM_LEDS - 1; i >= 0; i--) {
        digitalWrite(ledPins[i], LOW);
        delay(100);
    }
}

void patternHalfOnOff() {
    for (int i = 0; i < NUM_LEDS / 2; i++) {
        digitalWrite(ledPins[2,3,4,5,6,7], HIGH);
    }
    delay(500);
  
    for (int i = 0; i < NUM_LEDS / 2; i++) {
        digitalWrite(ledPins[i], LOW);
    }
    for (int i = NUM_LEDS / 2; i < NUM_LEDS; i++) {
        digitalWrite(ledPins[i], HIGH);
    }
    delay(500);
    for (int i = NUM_LEDS / 2; i < NUM_LEDS; i++) {
        digitalWrite(ledPins[i], LOW);
    }
}
