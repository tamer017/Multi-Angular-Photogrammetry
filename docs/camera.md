# Camera Details: MicaSense RedEdge-P

## Overview
The **MicaSense RedEdge-P** is a high-resolution multispectral and panchromatic camera system designed for UAV integration. It supports advanced photogrammetry tasks such as vegetation mapping, crop health analysis, and multi-angular data acquisition.

---

## Specifications

### **Multispectral Bands**
| Band Name  | Center Wavelength (nm) | Bandwidth (nm) |
|------------|-------------------------|----------------|
| Blue       | 475                     | 32             |
| Green      | 560                     | 27             |
| Red        | 668                     | 16             |
| Red Edge   | 717                     | 12             |
| Near-Infrared (NIR) | 842           | 57             |

### **Panchromatic Band**
| Property             | Value             |
|----------------------|-------------------|
| Center Wavelength    | 634.5 nm         |
| Bandwidth            | 463 nm           |
| Resolution           | 2464 x 2056      |
| Pixel Size           | 3.45 μm          |
| Field of View        | 44.5° HFOV x 37.7° VFOV |

---

## Physical Dimensions

| Property     | Value             |
|--------------|-------------------|
| Length       | 86.8 mm          |
| Width        | 63 mm            |
| Height       | 67.35 mm         |
| Mass (Camera Only) | 245 g      |
| Mass (Full Kit) | 315 g (with DLS 2, Wi-Fi, Storage) |

### Mounting Information
- Four M3 screw holes at 60 mm x 35 mm on-center.
- Use at least two opposite mounting points for stability.
- Ensure vibration isolation to maintain image quality.

---

## Operational Features

### **Capture Rate**
- Maximum: 3 captures per second (3 Hz), depending on storage device.
- Recommended overlap for high-quality mosaics: 75% or higher.

### **Input Voltage Range**
- **Voltage:** 7.0 V - 25.2 V
- **Power Consumption:** 7 W (continuous), 10 W (peak).

### **Environmental Requirements**
- Air exchange is required to maintain optimal operating temperature.
- Avoid stagnant environments; use ducted airflow for cooling if necessary.

---

## Downwelling Light Sensor (DLS 2)

### **Purpose**
The **DLS 2** measures ambient light and sun angle during flights, recording metadata for global lighting corrections in software like Pix4Dmapper and Agisoft Metashape.

### **Specifications**
| Property       | Value        |
|----------------|--------------|
| Height         | 14.03 mm    |
| Width          | 46.00 mm    |
| Length         | 63.50 mm    |
| Weight         | 54 g        |

### **Installation Notes**
- Mount at the highest point on the UAV with a clear view of the sky.
- Avoid interference from devices like GPS or data transmitters.

---

## Integration Example

Below is an example of the RedEdge-P and DLS 2 mounted on a DJI Matrice 300 UAV:

![Integration Example](../images/matrice300_integration.jpg)

---

## References
1. **MicaSense RedEdge-P Integration Guide (December 2021)**
2. [MicaSense Firmware Updates](https://www.micasense.com/firmware-updates)
3. [MicaSense API Documentation](http://micasense.github.io/rededge-api/api/http.html)

---

## Notes
For detailed installation and operation instructions, refer to the official [MicaSense User Guide](https://www.micasense.com/support). Additional resources for image processing are available at [GitHub](https://github.com/micasense/imageprocessing).
