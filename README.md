# LI-Practica-wowki
## Animacion del clima
### Codigos
#### Main
```
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C
import time

# Inicializa I2C y la pantalla OLED
i2c = I2C(0, scl=Pin(1), sda=Pin(0))
oled = SSD1306_I2C(128, 64, i2c)

def mostrar_clima(nombre, dibujo_func):
    oled.fill(0)
    dibujo_func()
    oled.text(nombre, 0, 54)
    oled.show()

# --------- Íconos ---------

def icono_sol():
    # Sol cuadrado con rayos
    oled.fill_rect(60, 20, 8, 8, 1)  # Centro
    # Rayos
    oled.line(64, 10, 64, 18, 1)
    oled.line(64, 28, 64, 36, 1)
    oled.line(54, 24, 59, 24, 1)
    oled.line(69, 24, 74, 24, 1)
    oled.pixel(58, 18, 1)
    oled.pixel(70, 18, 1)
    oled.pixel(58, 30, 1)
    oled.pixel(70, 30, 1)

def icono_nublado():
    oled.fill_rect(50, 24, 28, 12, 1)  # Nube principal
    oled.fill_rect(56, 20, 20, 6, 1)   # Parte superior

def icono_lluvia():
    icono_nublado()
    for x in range(55, 80, 6):
        oled.line(x, 36, x-1, 44, 1)  # Gotas diagonales

def icono_tormenta():
    icono_nublado()
    # Rayo
    oled.line(62, 36, 58, 44, 1)
    oled.line(58, 44, 66, 44, 1)
    oled.line(66, 44, 62, 52, 1)

def icono_nieve():
    icono_nublado()
    for x in range(54, 78, 8):
        oled.text("*", x, 40, 1)

# --------- Lista de climas ---------
climas = [
    ("Soleado", icono_sol),
    ("Nublado", icono_nublado),
    ("Lluvia", icono_lluvia),
    ("Tormenta", icono_tormenta),
    ("Nieve", icono_nieve)
]

# --------- Bucle principal ---------
while True:
    for nombre, dibujo in climas:
        mostrar_clima(nombre, dibujo)
        time.sleep(2)
```

#### OLED
```
# ssd1306.py
import framebuf

class SSD1306:
    def __init__(self, width, height, external_vcc):
        self.width = width
        self.height = height
        self.external_vcc = external_vcc
        self.pages = self.height // 8
        self.buffer = bytearray(self.pages * self.width)
        self.framebuf = framebuf.FrameBuffer(self.buffer, self.width, self.height, framebuf.MONO_VLSB)
        self.poweron()
        self.init_display()

    def init_display(self):
        for cmd in (
            0xae, 0xa4, 0xd5, 0xf0, 0xa8, self.height - 1, 0xd3, 0x00, 0x40,
            0x8d, 0x14, 0x20, 0x00, 0xa1, 0xc8, 0xda, 0x12, 0x81, 0xcf,
            0xd9, 0xf1, 0xdb, 0x40, 0xa6, 0xaf):
            self.write_cmd(cmd)
        self.fill(0)
        self.show()

    def poweron(self):
        pass

    def write_cmd(self, cmd):
        raise NotImplementedError

    def show(self):
        raise NotImplementedError

    def fill(self, col):
        self.framebuf.fill(col)

    def pixel(self, x, y, col):
        self.framebuf.pixel(x, y, col)

    def scroll(self, dx, dy):
        self.framebuf.scroll(dx, dy)

    def text(self, string, x, y, col=1):
        self.framebuf.text(string, x, y, col)

    def rect(self, x, y, w, h, col):
        self.framebuf.rect(x, y, w, h, col)

    def fill_rect(self, x, y, w, h, col):
        self.framebuf.fill_rect(x, y, w, h, col)

    def line(self, x1, y1, x2, y2, col):
        self.framebuf.line(x1, y1, x2, y2, col)

    def blit(self, fbuf, x, y):
        self.framebuf.blit(fbuf, x, y)

class SSD1306_I2C(SSD1306):
    def __init__(self, width, height, i2c, addr=0x3c, external_vcc=False):
        self.i2c = i2c
        self.addr = addr
        self.temp = bytearray(2)
        super().__init__(width, height, external_vcc)

    def write_cmd(self, cmd):
        self.temp[0] = 0x80
        self.temp[1] = cmd
        self.i2c.writeto(self.addr, self.temp)

    def show(self):
        for page in range(0, self.height // 8):
            self.i2c.writeto(self.addr, bytearray([0x00, 0xb0 | page, 0x02, 0x10]))
            self.i2c.writeto(self.addr, bytearray([0x40]) + self.buffer[page * self.width:(page + 1) * self.width])
```

### Resultado

## API GPT
### Codigo
```

```

### Resultado
