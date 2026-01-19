# ESPHome Base Template

A comprehensive starting template for ESP32 IoT projects with Home Assistant integration. This template provides a solid foundation for building IoT devices with professional features like automatic WiFi setup, factory reset, OTA updates, and sensor integration.

## 🚀 Features

### Core Functionality
- **WiFi Setup**: Automatic AP (Access Point) mode for initial configuration with captive portal
- **ESP32 Improv**: Mobile app support for easy WiFi configuration
- **Factory Reset**: Hardware button for device reset (GPIO4 by default)
- **OTA Updates**: Over-the-air firmware updates
- **mDNS Discovery**: Better network discovery for Home Assistant
- **API Integration**: Native Home Assistant API support

### Hardware Support
- **ESP32 Variants**: Configurable for different ESP32 boards
- **I2C Bus**: Pre-configured for sensors and displays
- **GPIO Pins**: Flexible pin configuration
- **Optional Display**: SSD1306 OLED support (commented out by default)
- **Sensor Framework**: Ready for various sensors (BME280 example included)

### Home Assistant Integration
- **Auto-Discovery**: Enhanced discovery with unique device names
- **Device Status**: Uptime and WiFi signal monitoring
- **Remote Control**: API services for device control
- **Diagnostic Sensors**: Built-in health monitoring

## 📋 Requirements

- ESP32 development board
- ESPHome CLI (`pip install esphome`)
- Home Assistant (recommended)

## 🛠️ Quick Start

### 1. Clone and Configure

```bash
# Copy the template
cp base-template.yaml my-project.yaml

# Edit configuration
nano my-project.yaml
```

### 2. Basic Configuration

```yaml
substitutions:
  device_name: "my-sensor"          # Unique device identifier
  device_description: "My IoT Sensor" # Friendly description
  button_pin: GPIO4                 # Reset button pin
  i2c_sda_pin: GPIO21              # I2C SDA pin
  i2c_scl_pin: GPIO22              # I2C SCL pin
```

### 3. Flash Device

```bash
# Validate configuration
esphome config my-project.yaml

# Flash to ESP32
esphome run my-project.yaml
```

### 4. Initial Setup

1. Device starts in AP mode: `my-sensor-xxxx`
2. Connect to WiFi: `my-sensor-xxxx` (password: `esphome123`)
3. Open `192.168.4.1` in browser
4. Configure your WiFi network
5. Device connects and appears in Home Assistant

### 5. Add to Home Assistant

**Option A: Auto-Discovery** (may not work in all network configurations)
- Home Assistant should detect the device automatically

**Option B: Manual Addition**
- Go to **Settings → Devices & Services → ESPHome**
- Click **"Add Device"**
- Enter device IP address (check your router's DHCP table)
- Port: `6053` (default)

## 📖 Configuration Guide

### WiFi Configuration

The template includes three WiFi connection methods:

1. **AP Mode**: Initial setup access point
2. **ESP32 Improv**: Mobile apps (ESP Touch, etc.)
3. **Captive Portal**: Automatic browser redirect

### Adding Sensors

Uncomment and modify the BME280 example:

```yaml
sensor:
  - platform: bme280_i2c
    temperature:
      name: Temperature
    humidity:
      name: Humidity
    pressure:
      name: Pressure
    address: 0x76
```

### Display Support

For OLED displays, uncomment the display section:

```yaml
display:
  - platform: ssd1306_i2c
    model: "SSD1306 128x64"
    address: 0x3C
    lambda: |-
      it.clear();
      it.printf(0, 0, id(font_small), "Device Status");
      // Add your display logic here
```

### Button Customization

Modify button behavior in the `on_press` section:

```yaml
on_press:
  then:
    - lambda: |-
        // Your custom button logic
        ESP_LOGI("button", "Custom action triggered");
```

## 🔧 Customization Examples

### Temperature Sensor Only

```yaml
sensor:
  - platform: dht
    pin: GPIO5
    model: DHT22
    temperature:
      name: Temperature
    humidity:
      name: Humidity
```

### Relay Control

```yaml
switch:
  - platform: gpio
    pin: GPIO5
    name: "Relay"
    id: relay
```

### RGB LED

```yaml
light:
  - platform: rgb
    name: "RGB LED"
    red: output_red
    green: output_green
    blue: output_blue

output:
  - platform: esp8266_pwm
    id: output_red
    pin: GPIO12
  - platform: esp8266_pwm
    id: output_green
    pin: GPIO13
  - platform: esp8266_pwm
    id: output_blue
    pin: GPIO14
```

## 🐛 Troubleshooting

### Device Not Detected by Home Assistant

1. **Check Network**: Ensure device and HA are on same subnet
2. **Manual Addition**: Add by IP address instead of auto-discovery
3. **Firewall**: Check if mDNS (port 5353) is blocked

### WiFi Connection Issues

1. **AP Mode**: Device shows as `device-name-xxxx` WiFi network
2. **Password**: Default AP password is `esphome123`
3. **Captive Portal**: Access `192.168.4.1` when connected to AP

### Sensor Not Working

1. **I2C Address**: Use `i2c.scan: true` to find correct addresses
2. **Pin Configuration**: Verify SDA/SCL pins match your wiring
3. **Power**: Ensure sensors have proper power supply

## 📚 Advanced Features

### Custom Services

Add API services for remote control:

```yaml
api:
  services:
    - service: custom_action
      then:
        - lambda: |-
            // Your custom service logic
            ESP_LOGI("service", "Custom action executed");
```

### Multiple Sensors

```yaml
sensor:
  - platform: dht
    pin: GPIO5
    temperature:
      name: "Room Temperature"
    humidity:
      name: "Room Humidity"

  - platform: adc
    pin: GPIO34
    name: "Light Level"
    update_interval: 30s
```

### Deep Sleep Support

For battery-powered devices:

```yaml
deep_sleep:
  run_duration: 60s
  sleep_duration: 5min
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Test your changes
4. Submit a pull request

## 📄 License

This project is open source. Feel free to use, modify, and distribute.

## 🙏 Acknowledgments

This ESPHome base template was developed with the assistance of an AI language model (Claude by Anthropic). The template incorporates best practices from the ESPHome community and provides a solid foundation for IoT projects.

## 📞 Support

- [ESPHome Documentation](https://esphome.io/)
- [ESPHome Community Forum](https://community.home-assistant.io/c/esphome/)
- [Home Assistant Community](https://community.home-assistant.io/)

---

**Happy Hacking!** 🎉