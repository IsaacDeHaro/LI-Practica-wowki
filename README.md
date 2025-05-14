# LI-Practica-wowki
## Animacion del clima
### Codigos
#### Main
```
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C
import time
import sys

# Inicializar pantalla OLED
i2c = I2C(0, scl=Pin(1), sda=Pin(0))
oled = SSD1306_I2C(128, 64, i2c)

# Base de datos de países y climas
base_datos_paises = {
    "Peru": "Nublado",
    "Argentina": "Soleado",
    "Chile": "Lluvia",
    "Colombia": "Tormenta",
    "Brasil": "Nieve",
    "Mexico": "Soleado",
    "Canada": "Lluvia",
    "Estados Unidos": "Tormenta",
    "Francia": "Nublado",
    "Espana": "Soleado",
    "Alemania": "Nublado",
    "Italia": "Soleado",
    "Reino Unido": "Lluvia",
    "Australia": "Soleado",
    "Japon": "Tormenta",
    "China": "Nublado",
    "India": "Soleado",
    "Sudafrica": "Lluvia",
    "Rusia": "Nieve",
    "Egipto": "Soleado",
    "Nigeria": "Tormenta",
    "Vietnam": "Lluvia",
    "Tailandia": "Soleado",
    "Indonesia": "Nublado",
    "Corea del Sur": "Lluvia",
    "Pakistan": "Tormenta",
    "Bangladesh": "Soleado",
    "Filipinas": "Nieve",
    "Arabia Saudita": "Soleado",
    "Irak": "Lluvia",
    "Turquia": "Nublado",
    "Israel": "Soleado",
    "Malasia": "Lluvia",
    "Singapur": "Soleado",
    "Nueva Zelanda": "Nublado",
    "Uruguay": "Soleado",
    "Paraguay": "Tormenta",
    "Afganistan": "Soleado", "Albania": "Lluvia", "Andorra": "Soleado", "Angola": "Tormenta",
    "Antigua y Barbuda": "Nieve", "Argelia": "Lluvia", "Armenia": "Tormenta",
    "Austria": "Nublado", "Azerbaiyan": "Lluvia", "Bahamas": "Soleado",
    "Barbados": "Tormenta", "Barein": "Lluvia", "Belgica": "Nublado", "Belice": "Soleado", "Benin": "Lluvia", 
    "Bhutan": "Tormenta", "Bielorrusia": "Lluvia", "Birmania": "Soleado", "Bolivia": "Nieve", "Bosnia y Herzegovina": "Nublado",
    "Botsuana": "Soleado", "Brunei": "Lluvia", "Bulgaria": "Tormenta", "Burkina Faso": "Soleado",
    "Burundi": "Soleado", "Butan": "Lluvia", "Cabo Verde": "Tormenta", "Camboya": "Soleado", "Camerun": "Soleado",
    "Catar": "Nublado", "Chad": "Tormenta", "Chipre": "Soleado",
    "Comoras": "Soleado", "Congo": "Soleado", "Corea del Norte": "Nieve",
    "Costa Rica": "Soleado", "Croacia": "Soleado", "Cuba": "Tormenta",
    "Curazao": "Nieve", "Dinamarca": "Nublado", "Dominica": "Lluvia", "Ecuador": "Lluvia",
    "El Salvador": "Tormenta", "Emiratos Arabes Unidos": "Soleado", "Eritrea": "Lluvia",
    "Eslovaquia": "Soleado", "Eslovenia": "Lluvia", "Estonia": "Lluvia",
    "Esuatini": "Tormenta", "Etiopia": "Soleado", "Fiyi": "Nublado",
    "Finlandia": "Nublado", "Gabon": "Lluvia", "Gambia": "Tormenta",
    "Georgia": "Soleado", "Ghana": "Lluvia", "Granada": "Soleado",
    "Guatemala": "Tormenta", "Guinea": "Nublado", "Guyana": "Soleado",
    "Haiti": "Tormenta", "Honduras": "Nublado", "Hungria": "Lluvia",
    "Iran": "Soleado", "Irlanda": "Lluvia", "Islandia": "Nieve",
    "Islas Marshall": "Tormenta", "Islas Salomon": "Lluvia", "Islas Feroe": "Nublado",
    "Jamaica": "Soleado", "Jordania": "Nublado", "Kazajistan": "Tormenta",
    "Kenia": "Soleado", "Kirguistan": "Nublado", "Kiribati": "Soleado",
    "Kuwait": "Lluvia", "Laos": "Nublado", "Lesoto": "Tormenta",
    "Letonia": "Soleado", "Libano": "Tormenta", "Liberia": "Nieve",
    "Libia": "Soleado", "Liechtenstein": "Tormenta", "Lituania": "Lluvia",
    "Luxemburgo": "Soleado", "Madagascar": "Tormenta", "Malaui": "Tormenta",
    "Maldivas": "Nublado", "Mali": "Tormenta", "Malta": "Soleado",
    "Marruecos": "Nublado", "Mauricio": "Tormenta", "Mauritania": "Soleado",
    "Micronesia": "Lluvia", "Moldavia": "Nieve", "Monaco": "Soleado",
    "Mongolia": "Nieve", "Mozambique": "Lluvia", "Namibia": "Nublado",
    "Nauru": "Lluvia", "Nepal": "Nublado", "Nicaragua": "Lluvia",
    "Niger": "Soleado", "Noruega": "Soleado", "Oman": "Lluvia",
    "Palaos": "Lluvia", "Panama": "Soleado", "Papua Nueva Guinea": "Tormenta",
    "Polonia": "Lluvia", "Portugal": "Soleado", "Ruanda": "Lluvia",
    "Rumania": "Nublado", "Samoa": "Lluvia", "San Cristobal y Nieves": "Nublado",
    "San Marino": "Soleado", "Santa Lucia": "Tormenta", "Santo Tome y Principe": "Soleado",
    "Senegal": "Tormenta", "Serbia": "Soleado", "Seychelles": "Soleado",
    "Sierra Leona": "Tormenta", "Siria": "Tormenta", "Somalia": "Tormenta",
    "Sri Lanka": "Soleado", "Suazilandia": "Soleado", "Sudan": "Lluvia",
    "Sudan del Sur": "Tormenta", "Suecia": "Lluvia", "Suiza": "Nublado",
    "Surinam": "Soleado", "Tayikistan": "Nublado", "Timor Oriental": "Soleado",
    "Togo": "Tormenta", "Tonga": "Soleado", "Trinidad y Tobago": "Tormenta",
    "Tunez": "Soleado", "Turkmenistan": "Tormenta", "Tuvalu": "Nublado",
    "Ucrania": "Tormenta", "Uganda": "Soleado", "Uzbekistan": "Lluvia",
    "Vanuatu": "Soleado", "Vaticano": "Soleado", "Venezuela": "Tormenta",
    "Yemen": "Soleado", "Yibuti": "Tormenta", "Zambia": "Soleado",
    "Zimbabue": "Nublado"
}



# Función para capitalizar manualmente la primera letra de cada palabra
def capitalize_string(input_string):
    words = input_string.split()  # Divide la entrada en palabras
    capitalized_words = [word[0].upper() + word[1:].lower() if word else '' for word in words]  # Capitaliza la primera letra de cada palabra
    return ' '.join(capitalized_words)  # Une las palabras de nuevo en una cadena

# -------- Íconos --------
def icono_sol():
    oled.fill_rect(60, 20, 8, 8, 1)
    oled.line(64, 10, 64, 18, 1)
    oled.line(64, 28, 64, 36, 1)
    oled.line(54, 24, 59, 24, 1)
    oled.line(69, 24, 74, 24, 1)
    oled.pixel(58, 18, 1)
    oled.pixel(70, 18, 1)
    oled.pixel(58, 30, 1)
    oled.pixel(70, 30, 1)

def icono_nublado():
    oled.fill_rect(50, 24, 28, 12, 1)
    oled.fill_rect(56, 20, 20, 6, 1)

def icono_lluvia():
    icono_nublado()
    for x in range(55, 80, 6):
        oled.line(x, 36, x-1, 44, 1)

def icono_tormenta():
    icono_nublado()
    oled.line(62, 36, 58, 44, 1)
    oled.line(58, 44, 66, 44, 1)
    oled.line(66, 44, 62, 52, 1)

def icono_nieve():
    icono_nublado()
    for x in range(54, 78, 8):
        oled.text("*", x, 40, 1)

# Asociación clima → ícono
iconos_clima = {
    "Soleado": icono_sol,
    "Nublado": icono_nublado,
    "Lluvia": icono_lluvia,
    "Tormenta": icono_tormenta,
    "Nieve": icono_nieve
}

# Mostrar clima en pantalla
def mostrar_pais_clima(pais, clima):
    oled.fill(0)
    iconos_clima[clima]()
    oled.text(pais, 0, 0)
    oled.text(clima, 0, 54)
    oled.show()

# Mostrar error
def mostrar_error(pais):
    oled.fill(0)
    oled.text("Pais no", 10, 20)
    oled.text("encontrado:", 0, 32)
    oled.text(pais, 0, 48)
    oled.show()

# Bucle principal de entrada serial
while True:
    sys.stdout.write("Ingresa un país: ")
    entrada = sys.stdin.readline().strip()  # Lee la entrada sin title()
    entrada = capitalize_string(entrada)  # Capitaliza la entrada correctamente

    if entrada in base_datos_paises:
        clima = base_datos_paises[entrada]
        mostrar_pais_clima(entrada, clima)
    else:
        mostrar_error(entrada)

    time.sleep(3)
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
