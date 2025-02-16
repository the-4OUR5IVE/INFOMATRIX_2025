#define MOTOR_PIN1 9   // Пин для управления направлением мотора
#define MOTOR_PIN2 10  // Второй пин для направления
#define BUTTON_PIN 2   // Пин для кнопки
#define PHOTO_PIN A0   // Пин для фоторезистора
#define SPEED 85       // Скорость мотора (0-255)

void setup() {
    pinMode(MOTOR_PIN1, OUTPUT);
    pinMode(MOTOR_PIN2, OUTPUT);
    pinMode(BUTTON_PIN, INPUT_PULLUP);
    pinMode(PHOTO_PIN, INPUT);
}

void loop() {
    int lightLevel = analogRead(PHOTO_PIN);  // Читаем уровень освещённости
    bool buttonState = digitalRead(BUTTON_PIN) == LOW; // Кнопка нажата?
    
    if (lightLevel < 500) {  // Если света нет (значение порога можно настроить)
        if (buttonState) {
            analogWrite(MOTOR_PIN1, 0);
            analogWrite(MOTOR_PIN2, SPEED);
        } else {
            analogWrite(MOTOR_PIN1, SPEED);
            analogWrite(MOTOR_PIN2, 0);
        }
    } else {
        analogWrite(MOTOR_PIN1, 0);
        analogWrite(MOTOR_PIN2, 0);  // Останавливаем мотор
    }
}
