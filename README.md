# MineGuard — Digital Safety Mesh

Public-facing marketing landing page for the **MineGuard** mining safety platform, developed by **Vertex**. Presents the Digital Safety Mesh value proposition to open-pit mining operators, fleet supervisors, and light-vehicle drivers.

Static site — no build step, no backend dependency.
# Pregunta 1: Definition of application service layer requirements

| Application Service Layer Requirement | Especificación de requisitos |
| :--- | :--- |
| **Servicio de Control de Temperatura Local** |  Interfaz visual en el hardware mediante un display LCD-1602 que muestre la temperatura actual de la taza y la temperatura ideal configurada.<br> Controles de entrada físicos mediante dos botones táctiles etiquetados como "+" y "-" para que el usuario configure la temperatura ideal (desde 40.5°C hasta 79.5°C en intervalos de 0.5°C).<br> Indicador visual mediante un LED que se encienda únicamente cuando la bebida alcance el nivel de temperatura ideal. |
| **Servicio de Notificación de Estado Operativo** |  Interfaz de texto en el display LCD que informe dinámicamente en la esquina superior derecha el estado del calentador: mostrar "READY", "WAITING" o "WARMING".<br> La interfaz visual debe apagarse automáticamente para indicar que el dispositivo ha entrado en modo "Stand By" tras 30 segundos sin detectar una taza.<br> Al detectar nuevamente una taza, la interfaz visual debe reactivarse automáticamente y actualizar el estado mostrado. |
| **Servicio de Monitoreo y Telemetría** |  Interfaz de consola donde se pueda monitorear el log de actividad del dispositivo en tiempo real.<br> El sistema debe imprimir en la consola cada 5 segundos una estructura de datos en formato JSON.<br> El JSON debe contener la información vital del dispositivo: `deviceMacAddress`, `operationMode`, `targetTemperature`, `currentTemperature`, `warmerState` y `createdAt`. |
---

## Pages

| File | Description |
|---|---|
| `index.html` | Main landing page |
| `terms.html` | Terms & Conditions (full visual parity with main page) |

---

## Sections (`index.html`)

| Section | ID | Description |
|---|---|---|
| Hero | `#banner` | Looping background video, animated title, 4 live KPI metrics |
| Data Strip | — | Scrolling ticker with real-time system status indicators |
| Solution | `#solution` | Problem statement, live alert simulation, mission & vision cards |
| Profiles | `#profiles` | Three stakeholder cards: Management, Fleet Supervision, Light Vehicle Drivers |
| Edge Technology | `#edge` | Edge Computing, V2X Protocol, Digital Safety Mesh — sticky scroll panels |
| How It Works | `#how-it-works` | 4-step process: Signal Detection → Edge Processing → Multi-Layer Alert → Resolution |
| Product | `#about-the-product` | Lazy-loaded product demo video in chrome frame |
| Pricing | `#subscription` | Enterprise Full Safety Suite plan card |
| Team | `#about-the-team` | 7-member team grid + lazy-loaded team presentation video |
| FAQ | `#faq` | 4 native `<details>`/`<summary>` accordions with exclusive-open logic |
| Footer | — | Brand, platform links, company links, contact info |

---

## Project Structure

```
mineguard-website/
├── index.html          # Main landing page
├── terms.html          # Terms & Conditions
├── style.css           # Design system + all component styles
├── main.js             # Runtime: GSAP, i18n, theme, facade, FAQ, counters
├── i18n/
│   ├── en.json         # English strings (13 sections)
│   └── es.json         # Spanish strings (13 sections)
└── assets/
    ├── truck.mp4       # Hero background video
    ├── mineguard3.png  # Logo & favicon
    └── img/team/       # Team member photos
```

---

## Technology Stack

| Technology | Version / Notes |
|---|---|
| HTML5 | Semantic markup, `<details>/<summary>` accordions |
| CSS3 | Custom properties (design tokens), grid, flexbox, `clip-path`, `aspect-ratio`, animations |
| JavaScript | Vanilla ES6+, no framework |
| GSAP | 3.12.2 — scroll-triggered fade-up animations |
| ScrollTrigger | GSAP plugin — `.fade-up` elements animate on viewport entry |
| Google Fonts | Bebas Neue, Barlow, Barlow Condensed |

---

## Key Features

### Dark / Light Theme
- Toggle button in the nav bar (sun/moon icon)
- Theme stored in `localStorage` key `mg-theme`
- CSS custom properties (`--bg`, `--white`, `--yellow`, etc.) swap via `[data-theme="light"]`
- **Nav always stays dark** regardless of theme, preserving logo visibility
- WCAG contrast fix: yellow `#FCB502` replaced by dark amber `#9B5000` for text on light backgrounds

### Internationalization (EN / ES)
- Language toggle in the nav bar, persisted to `localStorage` key `mg-lang`
- Every translatable element carries `data-section` and `data-value` attributes
- **Pattern — "HTML as English default"**: the hardcoded text in `index.html` and `terms.html` is the English content. No fetch occurs for English on first load. When the user switches to ES, `es.json` is fetched and `innerHTML` is replaced for all `[data-section][data-value]` elements. Switching back to EN fetches `en.json` to restore without a page reload
- To add a translatable string: add `data-section`/`data-value` to the HTML element and add the matching key to both `i18n/en.json` and `i18n/es.json`

**i18n sections in both JSON files:**

```
nav · banner · strip · solution · profiles · edge
how-it-works · product · subscription · team · faq · footer · terms
```

### Video Facade (Lazy Load)
Both the product demo and team presentation videos use the **Facade pattern**:
- On load: a YouTube thumbnail image is shown with a yellow `#FCB502` play button and a pulsing ring animation
- On click: the thumbnail is replaced with an autoplay `<iframe>` — no YouTube resources are fetched until the user explicitly plays the video
- Reduces initial page weight and avoids unwanted autoplay

### GSAP Scroll Animations
- All `.fade-up` elements start at `opacity: 0; translateY(48px)`
- GSAP + ScrollTrigger animates them to visible as they enter the viewport (trigger at `top 88%`)

### Dashboard Alert Feed
- Live rotating alert feed in the Solution section
- Cycles through 8 alert types (critical, warning, info, resolved) every 3.8 s with smooth fade/slide transitions

### Live Metrics Counter
- KPI numbers in the pricing and profile cards animate from 0 to their target value on first viewport intersection
- Ease-out cubic easing over 1.8 s

---

## Running Locally

Open `index.html` directly in a browser, or use a static server so that `fetch` calls for i18n JSON files work correctly:

```bash
# Node.js
npx serve .

# Python
python -m http.server 8080
```

Then open `http://localhost:8080`.

> **Note:** Opening `index.html` via `file://` protocol will block the `fetch` calls for translation files in most browsers. A local HTTP server is recommended for full i18n functionality.

---

## Design Tokens (CSS Custom Properties)

| Token | Dark value | Light value |
|---|---|---|
| `--bg` | `#141414` | `#F4F5F7` |
| `--yellow` | `#FCB502` | `#FCB502` |
| `--blue` | `#2578F4` | `#2578F4` |
| `--white` | `#F4F4F5` | `#1C1C2E` |
| `--text` | `#C8D8E8` | `#344054` |
| `--muted` | `#6B7280` | `#667085` |

---

## Legal

Terms & Conditions are available at [`terms.html`](terms.html). Footer legal links point to this page.

Product brand: **MineGuard** — Company brand: **Vertex**



.ino :

```
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <ArduinoJson.h>
#include <WiFi.h>
#include <Adafruit_MLX90614.h>

class Sensor {
public:
    virtual void update() = 0;
};

class Actuator {
public:
    virtual void execute() = 0;
};


class LedActuator : public Actuator {
private:
    int pin;
public:
    LedActuator(int p) : pin(p) { 
        pinMode(pin, OUTPUT); 
        turnOff();
    }
    void turnOn() { digitalWrite(pin, HIGH); }
    void turnOff() { digitalWrite(pin, LOW); }
    void execute() override {} 
};

class RelayActuator : public Actuator {
private:
    int pin;
public:
    RelayActuator(int p) : pin(p) { 
        pinMode(pin, OUTPUT); 
        turnOff();
    }
    void turnOn() { digitalWrite(pin, HIGH); }
    void turnOff() { digitalWrite(pin, LOW); }
    void execute() override {}
};


class ButtonSensor : public Sensor {
private:
    int pin;
    bool lastState;
public:
    bool isPressed;
    ButtonSensor(int p) : pin(p), lastState(HIGH), isPressed(false) { 
        pinMode(pin, INPUT_PULLUP); 
    }
    void update() override {
        bool currentState = digitalRead(pin);
        isPressed = (currentState == LOW && lastState == HIGH);
        lastState = currentState;
    }
};

class TempSensor : public Sensor {
public:
    Adafruit_MLX90614 mlx;
    float objectTemp;
    float ambientTemp;
    bool isSimulated;

    TempSensor() : objectTemp(40.0), ambientTemp(25.0), isSimulated(false) {}

    void init() {
        if (!mlx.begin()) {
            Serial.println("Warning: MLX90614 Custom Chip not found. Using Simulation Engine.");
            isSimulated = true;
        }
    }

    void update() override {
        if (!isSimulated) {
            objectTemp = mlx.readObjectTempC();
            ambientTemp = mlx.readAmbientTempC();
        }
    }
};

class HeatCoasterDevice {
private:
    float targetTemperature = 45.0;
    String operationMode = "ACTIVE";
    String warmerState = "WAITING";
    
    unsigned long lastTelemetryTime = 0;
    unsigned long cupRemovedTime = 0;
    bool isCupPresent = true;

    LiquidCrystal_I2C lcd;
    LedActuator statusLed;
    RelayActuator heaterRelay;
    ButtonSensor btnUp;
    ButtonSensor btnDown;
    TempSensor tempSensor;

public:
    HeatCoasterDevice() : lcd(0x27, 16, 2), statusLed(2), heaterRelay(15), btnUp(12), btnDown(14) {}

    void setup() {
        Serial.begin(115200);
        Wire.begin(21, 22);
        
        Serial.println("Company: Ikago");
        Serial.println("Developer: Rodrigo Alaya Cabrera - FIRSTstudent team");
        Serial.print("Device MAC: ");
        Serial.println(WiFi.macAddress());

        lcd.init();
        lcd.backlight();
        tempSensor.init();
    }

    void loop() {
        btnUp.update();
        btnDown.update();
        tempSensor.update();

        if (btnUp.isPressed && targetTemperature < 79.5) targetTemperature += 0.5;
        if (btnDown.isPressed && targetTemperature > 40.5) targetTemperature -= 0.5;

        if (tempSensor.isSimulated && operationMode == "ACTIVE") {
            if (warmerState == "WARMING") tempSensor.objectTemp += 0.5; 
            if (warmerState == "WAITING" && tempSensor.objectTemp > 25.0) tempSensor.objectTemp -= 0.2;
        }

        evaluateOperationMode();
        
        if (operationMode == "ACTIVE") {
            processTemperatureRules();
            updateDisplay();
        }

        handleTelemetry();
        delay(50);
    }

private:
    void evaluateOperationMode() {
        float diff = abs(tempSensor.objectTemp - tempSensor.ambientTemp);
        
        if (diff < 1.0) {
            if (isCupPresent) {
                isCupPresent = false;
                cupRemovedTime = millis();
            } else if (millis() - cupRemovedTime > 30000 && operationMode == "ACTIVE") {
                operationMode = "STAND_BY";
                warmerState = "WAITING";
                lcd.noBacklight();
                lcd.clear();
                statusLed.turnOff();
                heaterRelay.turnOff();
            }
        } else {
            isCupPresent = true;
            if (operationMode == "STAND_BY") {
                operationMode = "ACTIVE";
                lcd.backlight();
            }
        }
    }

    void processTemperatureRules() {
        if (abs(tempSensor.objectTemp - targetTemperature) <= 0.5) {
            warmerState = "READY";
            statusLed.turnOn();
            heaterRelay.turnOff();
        } 
        else if (tempSensor.objectTemp < targetTemperature) {
            warmerState = "WARMING";
            statusLed.turnOff();
            heaterRelay.turnOn();
        } 
        else {
            warmerState = "WAITING";
            statusLed.turnOff();
            heaterRelay.turnOff();
        }
    }

    void updateDisplay() {
        lcd.setCursor(0, 0);
        lcd.print(String(tempSensor.objectTemp, 1) + "C   ");
        
        lcd.setCursor(9, 0);
        lcd.print(warmerState == "READY" ? "READY  " : (warmerState == "WARMING" ? "WARMING" : "WAITING"));

        lcd.setCursor(0, 1);
        lcd.print("Target: " + String(targetTemperature, 1) + "C ");
    }

    void handleTelemetry() {
        if (millis() - lastTelemetryTime >= 5000) {
            StaticJsonDocument<256> doc;
            doc["deviceMacAddress"] = WiFi.macAddress();
            doc["operationMode"] = operationMode;
            doc["targetTemperature"] = targetTemperature;
            doc["currentTemperature"] = tempSensor.objectTemp;
            doc["warmerState"] = warmerState;
            doc["createdAt"] = millis(); 

            serializeJson(doc, Serial);
            Serial.println();
            
            lastTelemetryTime = millis();
        }
    }
};
HeatCoasterDevice device;

void setup() {
    device.setup();
}

void loop() {
    device.loop();
}
```

json:
```
{
  "version": 1,
  "author": "Rodrigo Alaya Cabrera",
  "editor": "wokwi",
  "parts": [
    {
      "type": "board-esp32-devkit-v1",
      "id": "esp",
      "top": 0,
      "left": 0,
      "attrs": { "macAddress": "24:0A:C4:00:94:81" } 
    },
    {
      "type": "wokwi-lcd1602",
      "id": "lcd",
      "top": -147.2,
      "left": 332,
      "attrs": { "pins": "i2c" }
    },
    {
      "type": "wokwi-pushbutton-6mm",
      "id": "btn_up",
      "top": -40.6,
      "left": -105.6,
      "attrs": { "color": "green", "label": "+" }
    },
    {
      "type": "wokwi-pushbutton-6mm",
      "id": "btn_down",
      "top": 170.6,
      "left": -115.2,
      "attrs": { "color": "red", "label": "-" }
    },
    {
      "type": "wokwi-led",
      "id": "led",
      "top": -138,
      "left": -53.8,
      "attrs": { "color": "blue", "label": "READY" }
    },
    { "type": "wokwi-relay-module", "id": "relay", "top": 67.4, "left": 355.2, "attrs": {} }
  ],
  "connections": [
    [ "esp:VIN", "lcd:VCC", "red", [] ],
    [ "esp:GND.1", "lcd:GND", "black", [] ],
    [ "esp:D21", "lcd:SDA", "green", [] ],
    [ "esp:D22", "lcd:SCL", "blue", [] ],
    [ "esp:VIN", "relay:VCC", "red", [] ],
    [ "esp:GND.1", "relay:GND", "black", [] ],
    [ "esp:D15", "relay:IN", "orange", [] ],
    [ "esp:D2", "led:A", "purple", [] ],
    [ "esp:GND.1", "led:C", "black", [] ],
    [ "esp:D12", "btn_up:2.l", "green", [] ],
    [ "esp:GND.1", "btn_up:1.l", "black", [] ],
    [ "esp:D14", "btn_down:2.l", "red", [] ],
    [ "esp:GND.1", "btn_down:1.l", "black", [] ]
  ],
  "dependencies": {}
}
```

