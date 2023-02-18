# 8. Final mit LED
Erstelle eine neue Datei und füge den Code ein:
```
import board
import keypad
import time
import digitalio
 
# LED setup Red.
led1 = digitalio.DigitalInOut(board.GP17)
led1.direction = digitalio.Direction.OUTPUT
led1.value = True

# LED setup Green.
led2 = digitalio.DigitalInOut(board.GP18)
led2.direction = digitalio.Direction.OUTPUT

#Tupel mit genutzten Pins
rowPins = (board.GP10, board.GP11, board.GP12, board.GP13)
columnPins = (board.GP14, board.GP15, board.GP16)

#keypad Matrix wird erstellt
keypadMatrix = keypad.KeyMatrix(rowPins, columnPins)

#Tastenzuweisung
tastenMap = { 0: "1", 1: "2", 2: "3", 3: "4", 4: "5", 5: "6", 6: "7", 7: "8",
              8: "9", 9: "*", 10: "0", 11: "#" }

#Signalpin der mit dem Relais verbunden ist
relais = digitalio.DigitalInOut(board.GP0)
relais.direction = digitalio.Direction.OUTPUT

#LED auf dem Pico, wird aktiviert um zu sehen, dass das Programm läuft
led = digitalio.DigitalInOut(board.GP25)
led.direction = digitalio.Direction.OUTPUT

#Variablen speichern
seq = []
wrongs = 0

#Dauerschleife
while True:
    led.value = True
    event = keypadMatrix.events.get()
    if event:
        if event.pressed:
            print(tastenMap[event.key_number])
            if(tastenMap[event.key_number] == "#"):
                if(seq == ['Deine_1_Zahl', 'Deine_2_Zahl', 'Deine_3_Zahl', 'Deine_4_Zahl', 'Deine_5_Zahl', 'Deine_6_Zahl']):
                   relais.value = True
                   led1.value = False
                   led2.value = True
                   wrongs = 0
                elif(seq == ['#', 'Deine_1_Zahl', 'Deine_2_Zahl', 'Deine_3_Zahl', 'Deine_4_Zahl', 'Deine_5_Zahl', 'Deine_6_Zahl']):
                   relais.value = True
                   led1.value = False
                   led2.value = True
                   wrongs = 0
                elif(seq == ['#']):
                    time.sleep(0.5)
                else:
                   led1.value = False
                   time.sleep(1)
                   led1.value = True
                   wrongalt = wrongs
                   wrongadd = 1
                   wrongs = wrongalt + wrongadd
                   print("Wrongs: " + str(wrongs))
                   if(wrongs == 3):
                       for x in range(60):
                           led1.value = False
                           time.sleep(0.5)
                           led1.value = True
                           time.sleep(0.5)
                       wrongs = 0
                time.sleep(1)
                seq = []
            elif(tastenMap[event.key_number] == "*"):
                 relais.value = False
                 led1.value = True
                 led2.value = False
                 time.sleep(1)
                 seq = []
            else:
                seq.append(tastenMap[event.key_number])
                time.sleep(0.5)
                print(seq)
```
Füge deinen Code bei den Feldern wo steht Deine_1-6_Zahl (Zeile 41 und 44 in Thonny)<br>
Speicher nun den Python Code mit dem namen **main.py**. (Mit diesem namen, wird der Code automatisch beim einstecken ausgeführt.)<br>
Du kannst nun den USB Stecker aus deinem PC rausstecken und beim 12v Netzteil in den USB Stecker reinstecken.<br>
Jetzt kannst du deine Truhe entsperren...<br>
... _ _ _ _ _ _ #<br>
... gebe deinen 6 stelligen Code ein und drücke die "#".<br>
...drücke "Stern" um wieder zu verschließen<br>
Gibst du den Code 3 mal Falsch ein, ist die Truhe für 60 Sekunden gesperrt!<br>
Noch Fragen? **moderatorps@gmail.com**<br>
## Viel Spaß!
