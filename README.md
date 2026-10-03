# Informática Forense

> Repositorio oficial del programa de **Informática Forense** — UdeCataluña
>
> Material de apoyo, guías de laboratorio y recursos complementarios para los estudiantes de la cohorte 2026.

---

## 📘 Guía principal para estudiantes

La guía que necesitás para montar tu laboratorio forense en casa (Windows, Mac o Linux). Contiene todo el paso a paso para los talleres de las Unidades 1 y 2.

| Documento | Descripción |
|---|---|
| [📘 GUIA-FORENSE-UdeC.md](./GUIA-FORENSE-UdeC.md) | **Guía principal** — lee esta primero |
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

1. **Lee la guía:** [GUIA-FORENSE-UdeC.md](./GUIA-FORENSE-UdeC.md) (o la versión HTML si preferís con formato).
2. **Elegí tu opción** según tu sistema operativo (Windows / Mac / Linux).
3. **Seguí los pasos** en orden. Si algo falla, consultá la sección "Solución de problemas" al final.
4. **Verificá** con los comandos de verificación al final de cada opción.

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

## ❓ Soporte

- **Foro del LMS** — para preguntas durante la cohorte.
- **Issues en este repositorio** — para reportar errores en la guía o sugerir mejoras.

---

## 📄 Licencia y atribución

Material académico desarrollado por **Fabio** para UdeCataluña · Ingeniería Informática.

Versión: `v.2026.1` — generada el 2026-10-02.