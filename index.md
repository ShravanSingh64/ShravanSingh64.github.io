---
layout: home
title: Home
---

# Shravan Singh

**Senior Embedded Linux & Software Engineer** — Montreal, QC

9+ years building embedded systems: custom Linux distributions with
Yocto, Qt/QML applications in C++, device drivers, OTA update systems,
and hardware–software integration from bootloader to UI.

**Core stack:** C++ · Qt/QML · Yocto/OpenEmbedded · POCO · U-Boot ·
Mender OTA · Embedded Linux drivers

📫 [shravansingh64@gmail.com](mailto:shravansingh64@gmail.com) ·
[GitHub](https://github.com/ShravanSingh64) ·
[LinkedIn](https://linkedin.com/in/atshravan)

---

## Experience

### Embedded Software Engineer — dcbel Inc., Montreal
*June 2025 – Present*

- Implemented U-Boot rollback for Yocto Linux, recovering automatically
  from faulty OTA downloads in production.
- Improved DevOps pipelines for Yocto image generation, reducing
  integration overhead for the engineering team.
- Spearheading refactoring of a legacy Qt codebase to a state
  machine–based architecture for maintainability and testability.

### Embedded Linux Engineer — BlueSparq, Inc., Cape Coral, FL
*January 2019 – June 2025*

- Designed a custom Yocto-based Linux image for Raspberry Pi CM3,
  cutting production imaging time by 60%.
- Wrote a Linux touchscreen driver (I2C) for a 7" display.
- Built Yocto recipes for out-of-tree kernel modules, services, and Qt
  apps, producing multiple tailored boot2Qt images from one hardware
  platform.
- Integrated Mender OTA and migrated the rootfs to read-only for
  security and reliability.
- Ported EDUP WiFi and Quectel modem drivers to kernel 5.15.

### Software Developer — BlueSparq, Inc., Cape Coral, FL
*June 2016 – January 2019*

- Developed Qt front-end/back-end applications for Windows and Linux.
- Certified 3G and LTE cellular modules with Verizon.
- Built a BLE peripheral stack in Python (D-Bus/BlueZ) with a Qt C++
  backend.
- Designed stepper motor control on PIC32 using UART, SPI, and
  interrupt-driven peripherals.

### Software Engineer — Accenture, Mumbai
*July 2012 – July 2014*

- Back-end support for corporate banking systems (PL/SQL, UNIX).
- Automated data-entry workflows with VBA and shell scripts, reducing
  processing time by 80%.

---

## Projects

{% for project in site.projects %}
- **[{{ project.title }}]({{ project.docs_url | default: project.repo_url }})**
  — {{ project.summary }}
  {% if project.repo_url %}([code]({{ project.repo_url }})){% endif %}
{% else %}
*Coming soon — cleaned-up tooling and technical write-ups.*
{% endfor %}

---

## Skills

**Languages:** C, C++, Python, QML, JavaScript, PL/SQL, Shell
**Frameworks & tools:** Qt, POCO, Yocto/OpenEmbedded, Mender OTA,
U-Boot, MPLAB
**Linux:** device drivers, kernel modules, boot2Qt, read-only rootfs,
BlueZ/D-Bus
**Protocols:** I2C, SPI, UART, CAN, USB, BLE, 3G/LTE
**Hardware:** Raspberry Pi CM3, i.MX6, PIC family; logic analyzer,
oscilloscope
**DevOps:** Yocto image pipelines, OTA rollback systems, embedded CI/CD

---

## Education

**M.Eng. Electrical Engineering** — Stevens Institute of Technology,
Hoboken, NJ (2014–2015) · Outstanding Performance Award

**B.Eng. Electronics & Telecommunication** — University of Mumbai
(2008–2014)

---

## Certifications

- CCNA (2018–2021)
- UCSC Extension — Linux Kernel & Drivers, LINX.X411 (2021)
- edX: Embedded Systems — Shape the World (6.02x); Electronic
  Interfaces (EE40LX)
