# 🌬️ Purificador de Aire Inteligente con IoT

<div align="center">
![IoT Air Purifier](https://img.shields.io/badge/Version-1.0.0-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Open%20Source-orange)

**Sistema de Purificación de Aire con Monitoreo en Tiempo Real y Control Automático vía IoT**

[Descripción](#-descripción) • [Estado del Arte](#-estado-del-arte-2025-2026) • [Patentes Similares](#-patentes-similares) • [Cómo Construirlo](#-cómo-construirlo-paso-a-paso) • [Arquitectura](#-arquitectura-del-sistema) • [Fuentes](#-fuentes)
</div>

---

## 📋 Descripción
Purificador de aire casero/inteligente que combina **filtro HEPA** con **sensores IoT** (PM2.5, PM10, CO₂, VOC, temperatura y humedad) para monitorear la calidad del aire en tiempo real, ajustar automáticamente la velocidad del ventilador y enviar alertas vía app o nube. Ideal para hogares, oficinas o espacios cerrados con contaminación, alergias o humo.

**Objetivo:** Mejorar la calidad del aire interior de forma automática, eficiente y accesible, con datos en la nube y control remoto.

### 🎯 Beneficios
- Monitoreo en tiempo real (AQI calculado automáticamente)
- Modo automático inteligente (ajusta velocidad según contaminación)
- Alertas por app / WhatsApp / email
- Bajo consumo energético
- Fácil de escalar o integrar con Home Assistant / Alexa

---

## 📖 Estado del Arte (2025-2026)

En 2025-2026 los purificadores inteligentes han dejado de ser un lujo y se han convertido en dispositivos **estándar** con las siguientes tendencias clave:

- **Sensores avanzados + IA/ML**: Detectan PM2.5/PM10, VOC, CO₂, formaldehído y ajustan automáticamente la velocidad (Levoit Vital 200S, Coway Airmega 50, Dyson Big+Quiet).
- **Conectividad IoT completa**: Wi-Fi, app (VeSync, SmartThings), voz (Alexa/Google) y monitoreo remoto.
- **Eficiencia energética**: Modo eco + auto que reduce consumo hasta 70 %.
- **Filtración híbrida**: HEPA H13/H14 + carbón activado + UV-C o ionización controlada (sin ozono excesivo).
- **Integración con edificios inteligentes**: Sistemas APVS (Adaptive Air Purification and Ventilation) que combinan purificación con ventilación y ahorro energético.
- **Tendencia actual**: Modelos compactos, con IA predictiva (aprenden hábitos del usuario) y monitoreo predictivo de filtros.

**Brecha de oportunidad para proyectos DIY/IoT**: Los dispositivos comerciales son caros (> $150 USD). Un sistema casero con ESP32 cuesta < $40 y permite total personalización + integración abierta (MQTT, Home Assistant).

---

## 🔬 Patentes Similares

Aquí las patentes más relevantes que sirven de referencia tecnológica:

### 1. US10864471B1 - IoT enabled smart filter device (2020, actualizaciones 2025)
Sistema inteligente de filtro con detección de obstrucción y alertas IoT. Usa sensores para monitorear flujo de aire y enviar notificaciones cuando el filtro necesita cambio.

**Relevancia**: Base perfecta para el control automático y notificaciones de nuestro proyecto.

### 2. US11772103B2 - Filter-less intelligent air purification device (2023)
Purificador sin filtro tradicional que usa ionización controlada + sensores IoT para purificar el aire.

**Relevancia**: Alternativa interesante si quieres evitar filtros HEPA caros.

### 3. US6494940B1 - Air purifier with air quality sensor and controller
Purificador con sensor óptico de calidad del aire y controlador que ajusta automáticamente el ventilador.

**Relevancia**: Concepto básico que evolucionó a los sistemas IoT actuales.

Otras patentes chinas recientes (CN) incorporan IA y monitoreo remoto similar al que proponemos.

---

## 🛠️ Cómo Construirlo (Paso a Paso) – Versión Económica con ESP32

### Materiales (costo aproximado < $40 USD en AliExpress/Amazon)
| Componente                  | Modelo recomendado          | Precio aprox. |
|-----------------------------|-----------------------------|---------------|
| Microcontrolador            | ESP32 DevKit V1             | $6            |
| Sensor Partículas           | PMS5003 o SDS011            | $12           |
| Sensor Gases/VOC            | MQ135 o MP503               | $3            |
| Sensor CO₂ (opcional)       | MH-Z19C                     | $15           |
| Sensor Temp/Humedad         | DHT22 o SHT31               | $3            |
| Ventilador + Filtro HEPA    | Ventilador 120mm + HEPA H13 | $10           |
| Pantalla                    | OLED 0.96" I2C              | $3            |
| Relé / MOSFET               | Relé 5V o IRF540            | $2            |
| Caja / Estructura           | Caja impresa 3D o acrílico  | $5            |
| Fuente                      | 5V 2A USB                   | $2            |

### Pasos de construcción
1. **Estructura física**: Coloca el filtro HEPA en una caja con entrada/salida de aire. El ventilador empuja el aire a través del filtro.
2. **Conexiones**:
   - PMS5003 → UART2 del ESP32 (TX2/RX2)
   - OLED → I2C (GPIO21 SDA, GPIO22 SCL)
   - Ventilador → Relé o PWM (GPIO23)
   - Sensores restantes → pines digitales/analógicos
3. **Programación** (Arduino IDE o ESP-IDF):
   - Lee sensores cada 10 segundos
   - Calcula AQI (índice de calidad del aire)
   - Si AQI > umbral → enciende ventilador a máxima velocidad
   - Envía datos por MQTT a ThingsBoard / Home Assistant / Blynk
4. **App / Nube**: Usa Blynk, Home Assistant o Thingspeak para ver datos en tiempo real desde el celular.
5. **Modo automático**: Ajusta velocidad del ventilador según niveles (bajo/medio/alto).

**Código básico de ejemplo** (disponible en GitHub repositorios como “ESP32 Air Quality Monitor”).

---

## 🏗️ Arquitectura del Sistema

```mermaid
graph TB
    A[Sensores: PMS5003, MQ135, MH-Z19, DHT22] --> B[ESP32]
    B --> C[Cálculo AQI + Lógica Automática]
    C --> D[Control Ventilador (PWM/Relé)]
    B --> E[WiFi / MQTT]
    E --> F[Nube: ThingsBoard / Home Assistant]
    F --> G[App Móvil + Alertas]
    B --> H[OLED Display]
