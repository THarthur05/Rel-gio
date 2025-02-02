# Relógio com Arduino Uno, RTC DS1307 e LCD 16x2 (Sem I2C)  

![Arduino Uno](https://upload.wikimedia.org/wikipedia/commons/3/38/Arduino_Uno_-_R3.jpg)  

## Descrição  
Este projeto consiste em um relógio digital utilizando um **Arduino Uno**, um **módulo RTC DS1307** e um **display LCD 16x2 sem interface I2C**. O objetivo é exibir a hora e a data em um LCD, mantendo a precisão do tempo mesmo quando o Arduino for desligado, graças ao RTC.  

🔹 **Funcionalidades:**  
- Exibe **hora, minutos e segundos** no LCD  
- Mantém a contagem do tempo mesmo sem alimentação  
- Permite ajustes manuais da data e hora  
- Ideal para projetos de automação e controle de tempo  

## 📑 Sumário  
- [ Pré-requisitos](#-pré-requisitos) 
- [ Instalação](#-instalação)  
- [ Conexões](#-conexões)  
- [ Código-fonte](#-código-fonte)  

## Pré-requisitos  
Antes de iniciar, certifique-se de ter os seguintes componentes e ferramentas:  

### **Hardware:**  
- Arduino Uno  
- Módulo RTC DS1307  
- Display LCD 16x2 (sem I2C)  
- Potenciômetro de 10KΩ (para controle de contraste do LCD)  
- Resistores (220Ω para backlight do LCD)  
- Fios jumper e protoboard  

### **Software:**  
- **Arduino IDE** (versão mais recente) → [Download](https://www.arduino.cc/en/software)  
- Biblioteca **RTClib** (para comunicação com o RTC)  
- Biblioteca **LiquidCrystal** (para o LCD)  

## Instalação  
1️⃣ Instale o **Arduino IDE** se ainda não tiver.  
2️⃣ Adicione a biblioteca **RTClib**:  
   - No Arduino IDE, vá em **Sketch → Incluir Biblioteca → Gerenciar Bibliotecas**  
   - Pesquise por **RTClib** e instale  
3️⃣ Adicione a biblioteca **LiquidCrystal**:  
   - Já vem instalada por padrão no Arduino IDE  
4️⃣ Baixe o código-fonte do projeto e carregue no Arduino.  

5️⃣ Conecte os componentes conforme a tabela abaixo e faça o upload do código no Arduino. 

## 🔌 Conexões  

### **RTC DS1307 → Arduino**  
| Pino RTC DS1307 | Pino Arduino Uno |
|----------------|----------------|
| VCC            | 5V             |
| GND            | GND            |
| SDA            | A4             |
| SCL            | A5             |

### **LCD 16x2 → Arduino**  
| Pino LCD | Pino Arduino Uno |
|----------|-----------------|
| VSS      | GND             |
| VDD      | 5V              |
| V0       | Meio do potenciômetro |
| RS       | 7               |
| RW       | GND             |
| E        | 8               |
| D4       | 9               |
| D5       | 10              |
| D6       | 11              |
| D7       | 12              |
| A (LED+) | 5V (com resistor de 220Ω) |
| K (LED-) | GND             |

### **Potenciômetro 10KΩ → LCD**  
| Pino Potenciômetro | Conexão |
|------------------|----------|
| 1 (Esquerda)    | 5V       |
| 2 (Meio)        | V0 do LCD |
| 3 (Direita)     | GND      |

## -código-fonte

#include <Wire.h>
#include <RTClib.h>
#include <LiquidCrystal.h>

// Definição dos pinos do LCD
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
RTC_DS1307 rtc;

void setup() {
  lcd.begin(16, 2);
  Wire.begin();

  if (!rtc.begin()) {
    lcd.print("Erro no RTC");
    while (1);
  }

  if (!rtc.isrunning()) {
    lcd.print("Ajustando RTC...");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }
}

void loop() {
  DateTime now = rtc.now();

  lcd.setCursor(0, 0);
  lcd.print("Hora: ");
  lcd.print(now.hour(), DEC);
  lcd.print(":");
  lcd.print(now.minute(), DEC);
  lcd.print(":");
  lcd.print(now.second(), DEC);

  lcd.setCursor(0, 1);
  lcd.print("Data: ");
  lcd.print(now.day(), DEC);
  lcd.print("/");
  lcd.print(now.month(), DEC);
  lcd.print("/");
  lcd.print(now.year(), DEC);

  delay(1000);

```sh
git clone https://github.com/seu-usuario/relogio-arduino-rtc-ds1307.git
