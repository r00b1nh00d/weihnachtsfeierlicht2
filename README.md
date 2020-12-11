# WeihnachtsfeierLicht2
## ~avatar avatar @unplugged
![PartyStern](https://github.com/r00b1nh00d/weihnachtsfeierlicht/blob/master/PartyStern3.gif?raw=true)

##  ~ @unplugged Variante 2
Die zweite Variante, wie sie auch in der Anfangsanimation zu sehen war ist etwas aufwendiger zu programmieren. Hier wird automatisch eine Liste von Messwerten erstellt und daraus ein Maximalwert gebildet. In abhängigkeit zur Lautstärke werden dann dauerhaft vom ersten und vom letzten Pixel aus bis zur Mitte die Pixel angesteuert.


## Schritt 1 
Beginne mit einer Liste, welche ``||basic:beim Start||`` erstellt und durch eine ``||loops:Schleife||`` mit Messwerten gefüllt wird.
```blocks

let maximum = 0
let aktueller_Messwert = 0
let messreihe: number[] = []
let strip = neopixel.create(DigitalPin.P1, 49, NeoPixelMode.RGB_RGB)
for (let index = 0; index < 48; index++) {
    messreihe.push(input.soundLevel())
}
```

## Schritt 2
Zum Beginn der ``||basic:dauerhaft||``- Schleife soll ein aktueller Messwert am Ende der Liste hinzugefügt werden und der erste, also der älteste Messwert entfernt werden.
Zudem soll ein Maximalwert aus dieser Liste herausgefunden werden.

```blocks
let maximum = 0
let aktueller_Messwert = 0
let messreihe: number[] = []
let strip = neopixel.create(DigitalPin.P1, 49, NeoPixelMode.RGB_RGB)
for (let index = 0; index < 48; index++) {
    messreihe.push(input.soundLevel())
}
basic.forever(function () {
    aktueller_Messwert = Math.round(input.soundLevel())
    messreihe.push(aktueller_Messwert)
    messreihe.shift()
    maximum = 6
    for (let Wert of messreihe) {
        maximum = Math.max(maximum, Wert)
    }

``` 
## Schritt 3
Für die Animation soll erstmal der Strip ``||neopixel.strip:ausgeschaltet||`` werden. <br>
Anschließend muss in je einer ``||loops:index-Schleife||`` die Farbe an der Index Position, bzw. von der anderen Seite gesehen am maximalen Pixel minus des Indexes eingestellt werden. Du kannst hier eine einfarbige Farbe nehmen oder mit dem ``||neopixel.strip:HSL Farbwert||`` die Pixel in unterschiedlichen Farben leuchten lassen. <br>
**Zum einstellen der HSL-Einfärbung kannst du für die Sättigung eine 100 eintragen, für die Helligkeit eine 20 und den Farbwert beispielsweise mit der Formel 60 +(Index x 60) berechnen lassen**



```blocks
let maximum = 0
let aktueller_Messwert = 0
let messreihe: number[] = []
let strip = neopixel.create(DigitalPin.P1, 49, NeoPixelMode.RGB_RGB)
for (let index = 0; index < 48; index++) {
    messreihe.push(input.soundLevel())
}
basic.forever(function () {
    aktueller_Messwert = Math.round(input.soundLevel())
    messreihe.push(aktueller_Messwert)
    messreihe.shift()
    maximum = 6
    for (let Wert of messreihe) {
        maximum = Math.max(maximum, Wert)
    }
    strip.clear()
    for (let Index = 0; Index <= Math.map(aktueller_Messwert, 0, maximum, 0, 24); Index++) {
        strip.setPixelColor(Index, neopixel.hsl(60 + Index * 60, 100, 20))
    }
    for (let Index2 = 0; Index2 <= Math.map(aktueller_Messwert, 0, maximum, 0, 24); Index2++) {
        strip.setPixelColor(48 - Index2, neopixel.hsl(60 + Index2 * 60, 100, 20))
    }
    strip.show()
    basic.pause(10)
})

``` 

