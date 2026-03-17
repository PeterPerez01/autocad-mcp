# AutoCAD MCP Server

**Autor:** MC. Peter Alejandro Pérez Chel

## Descripción
**AutoCAD MCP Server** es un potente servidor basado en Python que implementa el estándar **Model Context Protocol (MCP)** para proporcionar un control programático conversacional sobre **Autodesk AutoCAD**.

A través de la automatización COM de Windows, este sistema permite que asistentes de Inteligencia Artificial (como Claude de Anthropic, Cursor, o extensiones de VS Code) interactúen en **tiempo real** con AutoCAD. La IA puede desde dibujar geometrías básicas, hasta diseñar planos arquitectónicos complejos (habitaciones, muebles, instalaciones), gestionar capas y eliminar elementos de forma automatizada.

🎥 **Demostración en video:** [Youtube video of the working demo](https://www.youtube.com/watch?v=cr9zSmDDX3A)

---

## 🚀 Características Principales

El servidor expone una variedad de herramientas inteligentes directamente al asistente de IA:

* **Creación Estructural Inteligente:** Generación automática de muros, puertas, ventanas, habitaciones individuales, y mobiliario (`create_structure`). Estos elementos se asignan automáticamente a las capas correctas y pueden ser etiquetados.
* **Geometría Básica:** Creación paramétrica de Líneas, Círculos, Arcos, Rectángulos y Textos (controlando grosor y color).
* **Gestión Avanzada de Capas (Layers):** Consulta de capas existentes, establecimiento de capas activas, y creación de nuevas capas con colores y descripciones personalizadas.
* **Búsqueda e Inspección de Entidades:** Capacidad para obtener detalles del dibujo actual (`get_drawing_info`) y listar en tiempo real todas las entidades dibujadas en el espacio de trabajo con su _handle_, tipo, capa y color (`get_entities`).
* **Edición y Limpieza Automatizada:** 
  * Eliminación de entidades por Handle, Capa, Color o Tipo (ej. borrar todos los textos verdes, o toda la capa "WALLS").
  * Cambio de color de entidades existentes (`change_entity_color`).
  * Deshacer operaciones recientes (`undo_last_operation`).
* **Visualización:** Control dinámico de la vista como ajustar el zoom a todos los elementos (`zoom_extents`).

---

## 📋 Requisitos del Sistema

1. **Sistema Operativo:** Windows (exclusivo, ya que depende de la API COM de Windows).
2. **Software CAD:** Autodesk AutoCAD (Cualquier versión posterior a 2000 que sea compatible con automatización COM) con una licencia válida. *Debe estar activo y abierto antes de usar el servidor.*
3. **Python:** Versión `3.8` o superior.
4. **Cliente MCP:** Por ejemplo, la aplicación de escritorio Claude (Claude Desktop).

---

## ⚙️ Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/PeterPerez01/autocad-mcp-server
cd autocad-mcp-server

# 2. Recomenadado: Crear un entorno virtual de Python
python -m venv venv
venv\Scripts\activate

# 3. Instalar la paquetería actual y sus dependencias (mcp, pywin32, etc.)
pip install -e .
```

---

## 🔧 Configuración para Claude Desktop

Para que Claude reconozca las herramientas de AutoCAD, añade el servidor a tu archivo de configuración de Claude Desktop:

* **Ruta del archivo en Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

Añade este bloque (reemplazando la ruta con la ubicación real de tu proyecto):

```json
{
  "mcpServers": {
    "autocad-mcp": {
      "command": "python",
      "args": [
        "C:\\Ruta\\Absoluta\\hacia\\tu\\autocad-mcp-server\\server.py"
      ]
    }
  }
}
```

> **Importante:** Recuerda reiniciar Claude Desktop por completo tras modificar este archivo para que registre las herramientas de AutoCAD y siempre mantener la aplicación de **AutoCAD abierta y con un dibujo inicializado** antes de iniciar la solicitud desde la IA.

---

## 💡 Ejemplos de uso (Prompting)

Una vez lo tengas conectado a tu asistente mediante MCP, puedes utilizar indicaciones en lenguaje natural como:

* _"Revisa el dibujo actual y dime cuántas entidades tenemos."_
* _"Dibuja un plano de una casa de 10x15 metros. Incluye un baño, área de sala y cocina, y pon ventanas."_
* _"Borra todas las entidades de color azul y pon el zoom completo del dibujo."_
* _"Crea una nueva capa llamada ELÉCTRICO de color rojo, y empieza a dibujar círculos en las esquinas de la misma."_

---

## 👨‍💻 Desarrollo

Este proyecto utiliza:
* `mcp`: Como SDK base del Model Context Protocol.
* `pywin32`: Paquete de Python para la interacción directa y automatización con objetos COM de Windows.

### Arquitectura Principal
El archivo base (`server.py` o módulo de entrada) expone métodos decorados bajo el servidor MCP para conectarse a `win32com.client.Dispatch("AutoCAD.Application")`, y traduce automáticamente el JSON entrante a llamadas de funciones como `AddLine`, `AddCircle`, o `Delete()` dentro del `ModelSpace` del dibujo activo de AutoCAD.

---

## Licencia / Aviso Legal

Este software utiliza la automatización local permitida por Autodesk pero no está oficialmente respaldado por Autodesk, Inc. El uso requiere una instalación legítima y licencia válida bajo los términos correspondientes de AutoCAD.
