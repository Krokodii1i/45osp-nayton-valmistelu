# Tehtävän eteneminen
ÄÄNI AKTIVOIDUT VALOT

let noise = 0
let light2 = 0
led.enable(false)    #Kytkee ledin päälle / pois
let strip = neopixel.create(DigitalPin.P1, 1, NeoPixelMode.RGB)    #Ohjaa Neopixel stirppiä
basic.forever(function () {
    light2 = smarthome.ReadLightIntensity(AnalogPin.P3)    #TODO: get light intensity(0~100%)
    if (light2 < 50) {    
        noise = smarthome.ReadNoise(AnalogPin.P2)
        if (noise > 70) {
            strip.showColor(neopixel.colors(NeoPixelColors.White))    #Jos ääni isompi kuin 70 laita valo päälle
            basic.pause(10000)    #Kun ääni loppunut odota 10sec ja sammuta valo
            strip.showColor(neopixel.colors(NeoPixelColors.Black))
        }
    }
})

SMART FAN

let temp = 0
OLED.init(64, 128)
basic.forever(function () {
    temp = smarthome.ReadTemperature(TMP36Type.TMP36_temperature_C, AnalogPin.P2)    #Lue temperature arvossa C
    OLED.clear()
    OLED.writeString("Temperature:")    #Kirjoita näytölle " Temperature: "
    OLED.writeNum(temp)    #Kirjoita lämpötila " Temperature: " perään
    if (temp > 29) {
        basic.showLeds(`
            . # . # .
            # . # . #
            # . . . #
            . # . # .
            . . # . .
            `)    #Jos lämpötila yli 29C näytä led kuvio
        music.startMelody(music.builtInMelody(Melodies.BaDing), MelodyOptions.Once)
        pins.digitalWritePin(DigitalPin.P1, 1)
        basic.pause(5000)    #tauko 5sec
        pins.digitalWritePin(DigitalPin.P1, 0)
        basic.pause(500)    #tauko 0,5 sec
    } else {
        pins.digitalWritePin(DigitalPin.P1, 0)
    }
})


AUTO WINDOWS

let noise = 0
pins.servoWritePin(AnalogPin.P1, 0)
basic.forever(function () {
    noise = smarthome.ReadNoise(AnalogPin.P10)
    if (noise > 50) {
        pins.servoWritePin(AnalogPin.P1, 0)
        basic.pause(2000)
    } else {
        pins.servoWritePin(AnalogPin.P1, 100)
        basic.pause(2000)
    }
})


AUTOMAATTINEN VAATEKAAPPI

pins.setPull(DigitalPin.P2, PinPullMode.PullUp)
let door = false
let strip = neopixel.create(DigitalPin.P1, 1, NeoPixelMode.RGB)
basic.forever(function () {
    if (pins.digitalReadPin(DigitalPin.P2) == 0) {
        door = !(door)
        if (door == true) {
            strip.showColor(neopixel.colors(NeoPixelColors.White))
            pins.servoWritePin(AnalogPin.P7, 0)
        } else {
            pins.servoWritePin(AnalogPin.P7, 180)
            strip.showColor(neopixel.colors(NeoPixelColors.Black))
        }
    }
})
+
