# Tehtävän eteneminen
ÄÄNI AKTIVOIDUT VALOT

let noise = 0
let light2 = 0
led.enable(false)
let strip = neopixel.create(DigitalPin.P1, 1, NeoPixelMode.RGB)
basic.forever(function () {
    light2 = smarthome.ReadLightIntensity(AnalogPin.P3)
    if (light2 < 50) {
        noise = smarthome.ReadNoise(AnalogPin.P2)
        if (noise > 70) {
            strip.showColor(neopixel.colors(NeoPixelColors.White))
            basic.pause(10000)
            strip.showColor(neopixel.colors(NeoPixelColors.Black))
        }
    }
})

SMART FAN

let temp = 0
OLED.init(128, 64)
basic.forever(function () {
    temp = smarthome.ReadTemperature(TMP36Type.TMP36_temperature_C, AnalogPin.P1)
    OLED.clear()
    OLED.writeString("temperature")
    OLED.writeNum(temp)
    if (temp > 30) {
        basic.showLeds(`
            # # . # #
            # . # . #
            # . . . #
            . # # # .
            . . # . .
            `)
        pins.digitalWritePin(DigitalPin.P1, 1)
        basic.pause(5000)
        pins.digitalWritePin(DigitalPin.P1, 0)
        basic.pause(500)
    } else {
        pins.digitalWritePin(DigitalPin.P1, 0)
    }
})

AUTO WINDOWS

let noise = 0
pins.servoWritePin(AnalogPin.P1, 0)
basic.forever(function () {
    noise = smarthome.ReadNoise(AnalogPin.P10)
    if (noise > 30) {
        pins.servoWritePin(AnalogPin.P1, 0)
        basic.pause(2000)
    } else {
        pins.servoWritePin(AnalogPin.P1, 100)
        basic.pause(500)
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
