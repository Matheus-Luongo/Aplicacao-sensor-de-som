# Aplicacao-de-sensore-de-som
Aplicação de sensores junto a micro controladores para coleta de dados em tempo real.
#include <SoftwareSerial.h>
#include <LiquidCrystal_I2C.h>

//Declarando o display LCD
LiquidCrystal_I2C lcd(0x20, 16, 2);

const int sensor = 9; // Pino digital para sensor
const int led_alarme = 8; // Pino digital para LED de alarme
const int sensor_A0 = A5; // Pino analógico para sensor de umidade

int sensorAnalogValue = 0; // Valor lido do sensor de umidade

bool activePin(int pinNumber) { // declara active como booleano
  return digitalRead(pinNumber); // retorna o valor booleano para o pino
}

void setup() {
  Serial.begin(9600); // inicia o display
  lcd.init();
  lcd.backlight();
  lcd.leftToRight();
  lcd.setCursor(3, 0);
  lcd.print("Monitor de"); // escreve monitor de som no display
  lcd.setCursor(7, 7);
  lcd.print("som");
  delay(4000);
  pinMode(sensor, INPUT); // declara o pino 9 como entrada
  pinMode(led_alarme, OUTPUT); // declara o pino 8 como saida
  pinMode(sensor_A0, INPUT);// declara o pino analogico A5 como entrada
}

void loop() {
  sensorAnalogValue = analogRead(sensor_A0); // Armazena o valor coletado pelo sensor na variavel

  if (activePin(sensor)) { // se a variavel receber sinal positivo
    digitalWrite(led_alarme, HIGH); // liga o led de alarme
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Db: "); // escreve um valor decibéis que o sensor coletou
    lcd.print(sensorAnalogValue);
    delay(3000);
    lcd.clear();
  } else { // se a variavel receber sinal negativo
    lcd.clear(); // limpa o display
    digitalWrite(led_alarme, LOW); // apaga o led
  }
}
