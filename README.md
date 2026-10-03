# Informática Forense · UdeCataluña

> Repositorio oficial del programa de **Informática Forense** · Cohorte 2026-II

🌐 **Sitio web del programa:** [fabig76.github.io/Informatica-forense](https://fabig76.github.io/Informatica-forense/)

Material de apoyo, guías de laboratorio y recursos complementarios para los estudiantes de la cohorte 2026.

---

## 📘 Recursos principales

| Documento | Descripción |
|---|---|
| [🌐 Sitio web del programa](https://fabig76.github.io/Informatica-forense/) | **Empezá aquí** — landing page con el programa completo |
| [📘 GUIA-FORENSE-UdeC.md](./GUIA-FORENSE-UdeC.md) | Guía principal — lee esta para montar tu laboratorio |
| [🌐 GUIA-FORENSE-UdeC.html](./GUIA-FORENSE-UdeC.html) | Misma guía, versión con estilo para navegador |

### ¿Qué vas a encontrar en la guía?

- 🪟 **Opción A · WSL2 (Windows)** — la más rápida, recomendada para Windows 10/11
- 💻 **Opción B · VirtualBox** — Linux completo con escritorio gráfico (cualquier SO)
- 🍎 **Opción C · Mac + Homebrew** — para usuarios macOS
- 🔧 Instalación de `exiftool`, `dc3dd`, `sleuthkit`, `ewf-tools`, `avml`, Python 3
- 🐛 Solución de problemas comunes (errores de BIOS, USB, RAM, etc.)
- 📤 Checklist de entregables por unidad

**Tiempo estimado:** 2-3 horas la primera vez (la mayoría son descargas), 30 minutos en sesiones siguientes.

---

## 🚀 Cómo empezar

1. **Visita la landing** → [fabig76.github.io/Informatica-forense](https://fabig76.github.io/Informatica-forense/) para una vista general del programa.
2. **Lee la guía:** [GUIA-FORENSE-UdeC.md](./GUIA-FORENSE-UdeC.md) (o la [versión HTML](./GUIA-FORENSE-UdeC.html) si preferís con formato).
3. **Elegí tu opción** según tu sistema operativo (Windows / Mac / Linux).
4. **Seguí los pasos** en orden. Si algo falla, consultá la sección "Solución de problemas" al final.
5. **Verificá** con los comandos de verificación al final de cada opción.

---

## 🛠️ Herramientas cubiertas

| Herramienta | Uso |
|---|---|
| `exiftool` | Lectura de metadatos en imágenes y archivos |
| `dc3dd` | Adquisición forense de discos (variante de `dd` del DoD) |
| `sleuthkit` | Análisis forense de sistemas de archivos |
| `ewf-tools` | Manejo de imágenes en formato EnCase (`.E01`) |
| `avml` | Captura de memoria RAM (Microsoft) |
| `foremost` / `testdisk` | Recuperación de archivos |
| Python 3 | Forense Dashboard, scripts personalizados |

---

## 📂 Estructura del repositorio

```
.
├── index.html              ← Landing page del programa (GitHub Pages)
├── README.md               ← Este archivo
├── GUIA-FORENSE-UdeC.md    ← Guía principal (Markdown)
└── GUIA-FORENSE-UdeC.html  ← Misma guía, con estilo para navegador
```

---

## ❓ Soporte

- **Foro del LMS** — para preguntas durante la cohorte.
- **Issues en este repositorio** — para reportar errores en la guía o sugerir mejoras.

---

© 2026 UdeCataluña · Cohorte 2026-II