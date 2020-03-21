---
layout: post
title:  "Katakana Quiz"
date:   2020-02-16 11:16:10 -0500
categories: project
permalink: /projects/Katakana_Quiz/

---
# (Work in progress)


I am currently taking a intro Japanese class at Miami Dade College. In this class we must learn part of the [Japanese writing system](https://en.wikipedia.org/wiki/Japanese_writing_system).

The writing system is divided into 3 parts.

Hiragana, Katakana, and Kanji.

Let's focus on katakana for now. [Katakana](https://en.wikipedia.org/wiki/Katakana) is one of Japanese's syllabaries. It is used for loan words from other languages; for onomatopoeia; and for scientific and technical terms.



##Parts List
  * Arduino UNO
  * 10k Potentiometer
  * Push button switch
  * 20x4 LCD
  * Hitachi HD44780 LCD controller
  
---

Looking at the datasheet for the [Hitachi HD44780 LCD controller](https://cdn-shop.adafruit.com/datasheets/HD44780.pdf) I learned that the character generator ROM has Japanese katakana symbols. So I hooked up a quick test circuit and wrote a bit of code just so I could see the katakana characters.


![kana4](/img/katakanaquiz4.JPG)

```c
#include <LiquidCrystal.h>

int aval = 0;
int row = 0;
int col = 0;
char c = '0';

const int apin = A0;
const int button = 10;
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

void setup() {
  pinMode(button, INPUT);
  lcd.begin(20, 4);
  lcd.display();
  Serial.begin(9600);
}
void loop() {
  lcd.setCursor(col,row);
  aval = analogRead(apin);
  c = (aval % 64) + B10100000;
  lcd.print(c);
  Serial.print('\n');
  Serial.print(col);
  Serial.print('_');
  Serial.print(aval);
  Serial.print('_');
  Serial.println(aval, BIN);
  if (digitalRead(button) == HIGH){
    col++;
    if(col == 20){
      row = 1;
      col = 0;  
    }
  }
   delay(200);
}
```
