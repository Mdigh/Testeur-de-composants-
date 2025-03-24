# Testeur-de-composants-
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// l'adresse I2C de l'écran LCD
LiquidCrystal_I2C lcd(0x27, 16, 2);  

void setup() {
  // Initialisation de l'écran LCD
  lcd.init();
  // Allumer le rétroéclairage
  lcd.backlight();
  
  // Affichage d'un message de démarrage
  lcd.setCursor(0, 0);
  lcd.print("Demarrage du testeur");

  // Effectuer le test du composant dès le démarrage
  detectComponent();
}

void loop() {
  // La boucle loop() est vide 
  // Le test du composant est effectué une seule fois au démarrage
}

// Réinitialiser toutes les broches à LOW ou en entrée
void resetPins() {
  for (int i = 2; i <= 6; i++) {
    pinMode(i, INPUT);  // Met toutes les broches en entrée
    digitalWrite(i, LOW);  // Assure que les sorties sont à LOW
  }
}

// 74HC00 
bool test74HC00() {
  resetPins();
  
  pinMode(2, OUTPUT); 
  pinMode(3, OUTPUT); 
  pinMode(4, INPUT);  

  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != LOW) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, LOW);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, LOW);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != HIGH) return false;

  return true;
}

// 74HC04 
bool test74HC04() {
  resetPins();

  pinMode(5, OUTPUT);
  pinMode(6, INPUT);  

  digitalWrite(5, LOW);
  if (digitalRead(6) != HIGH) return false;

  digitalWrite(5, HIGH);
  if (digitalRead(6) != LOW) return false;

  return true;
}

// 74HC02 
bool test74HC02() {
  resetPins();

  pinMode(8, OUTPUT); 
  pinMode(9, OUTPUT); 
  pinMode(10, INPUT);  

  digitalWrite(8, LOW);
  digitalWrite(9, LOW);
  if (digitalRead(10) != HIGH) return false;

  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
  if (digitalRead(10) != LOW) return false;

  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
  if (digitalRead(10) != LOW) return false;

  digitalWrite(8, LOW);
  digitalWrite(9, HIGH);
  if (digitalRead(10) != LOW) return false;

  return true;
}

// 74HC08 
bool test74HC08() {
  resetPins();

  pinMode(2, OUTPUT); 
  pinMode(3, OUTPUT); 
  pinMode(4, INPUT);  

  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  if (digitalRead(4) != LOW) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, LOW);
  if (digitalRead(4) != LOW) return false;

  digitalWrite(2, LOW);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != LOW) return false;

  return true;
}

// 74HC10 
bool test74HC10() {
  resetPins();

  pinMode(2, OUTPUT); 
  pinMode(3, OUTPUT); 
  pinMode(10, OUTPUT);
  pinMode(9, INPUT);  

  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  digitalWrite(10, LOW);
  if (digitalRead(9) != HIGH) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(10, HIGH);
  if (digitalRead(9) != LOW) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(10, LOW);
  if (digitalRead(9) != HIGH) return false;

  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  digitalWrite(10, HIGH);
  if (digitalRead(9) != HIGH) return false;

  return true;
}

//74HC32 
bool test74HC32() {
  resetPins();

  pinMode(2, OUTPUT); 
  pinMode(3, OUTPUT); 
  pinMode(4, INPUT);   

  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  if (digitalRead(4) != LOW) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, LOW);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, LOW);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != HIGH) return false;

  return true;
}

//74HC86
bool test74HC86() {
  resetPins();

  pinMode(2, OUTPUT); 
  pinMode(3, OUTPUT); 
  pinMode(4, INPUT);  

  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  if (digitalRead(4) != LOW) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, LOW);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, LOW);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != HIGH) return false;

  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  if (digitalRead(4) != LOW) return false;

  return true;
}



// Fonction pour détecter le composant et afficher le résultat sur l'écran LCD
void detectComponent() {
  lcd.clear();
  int componentType = 0;

  // tests pour chaque composant
  if (test74HC00()) {
    componentType = 1;
  } else if (test74HC04()) {
    componentType = 2;
  } else if (test74HC02()) {
    componentType = 3;
  } else if (test74HC08()) {
    componentType = 4;
  } else if (test74HC10()) {
    componentType = 5;
  } else if (test74HC32()) {
    componentType = 6;
  } else if (test74HC86()) {
    componentType = 7;
  } else {
    // Si aucun composant n'est détecté, afficher "pas de circuit"
    componentType = 0;
  }

  // Affichage du résultat sur l'écran LCD
  switch(componentType) {
    case 1:
      lcd.print("74HC00 fonctionelle");
      break;
    case 2:
      lcd.print("74HC04 fonctionelle");
      break;
    case 3:
      lcd.print("74HC02 fonctionelle");
      break;
    case 4:
      lcd.print("74HC08 fonctionelle");
      break;
    case 5:
      lcd.print("74HC10 fonctionelle");
      break;
    case 6:
      lcd.print("74HC32 fonctionelle");
      break;
    case 7:
      lcd.print("74HC86 fonctionelle");
      break;
    default:
      lcd.print("pas de circuit");
      break;
  }

 
  delay(2000);
}
