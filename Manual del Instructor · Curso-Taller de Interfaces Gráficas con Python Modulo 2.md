# Manual del Instructor · Curso-Taller de Interfaces Gráficas con Python

Sep 27, 2026 · @Noe Cazarez Camargo

## Módulo 2 · Modernización visual con CustomTkinter

El grupo ya sabe colocar widgets y conectarlos con variables. En este módulo cambia la capa visual: los mismos conceptos (gestores de geometría, `command`, variables de control) se aplican a los widgets de CustomTkinter, que dibujan esquinas redondeadas, soportan modo claro y oscuro y escalan bien en pantallas de alta densidad.

### Objetivos de aprendizaje

Al cerrar el módulo, el participante:

- Explica qué limita visualmente a Tkinter estándar y qué resuelve CustomTkinter.
- Traduce una interfaz de `tk.*` a `ctk.*` sin cambiar su lógica.
- Cambia el modo de apariencia en tiempo de ejecución y aplica un tema JSON propio.
- Usa `CTkOptionMenu`, `CTkComboBox`, `CTkSwitch` y `CTkSlider` con sus callbacks.
- Organiza una pantalla compleja con `CTkFrame`, `CTkTabview` y `CTkScrollableFrame`.

### Distribución por sesiones

| Sesión | Duración | Contenido | Actividad práctica |
| --- | --- | --- | --- |
| 5 | 2 h | Tema 2.1: de Tkinter a CustomTkinter. Tema 2.2: modos de apariencia y temas incluidos | Ejercicio 4 (migrar un formulario del Módulo 1) |
| 6 | 2 h | Tema 2.2: tema JSON propio. Tema 2.3: widgets modernos | Ejercicio 5 (panel de preferencias) |
| 7 | 2 h | Tema 2.4: `CTkFrame`, `CTkTabview`, `CTkScrollableFrame` | Ejercicio 6 (lista desplazable de tareas) |
| 8 | 2 h | Diseño del dashboard: boceto en papel y construcción | Desarrollo y revisión del entregable |

### Preparación del instructor

- Instalar CustomTkinter en cada entorno virtual con `pip install customtkinter` y verificar con `python -c "import customtkinter; print(customtkinter.__version__)"`. El material está escrito para la serie 5.2.
- Tener a mano la carpeta de temas incluidos, que se abre en la Sesión 6. Su ruta se obtiene con `python -c "import customtkinter, os; print(os.path.join(os.path.dirname(customtkinter.__file__), 'assets', 'themes'))"`.
- Llevar una captura de un dashboard administrativo real (por ejemplo, el panel de un sistema escolar o de un gestor de contenidos) para la sesión de boceto.

### Tema 2.1 · De Tkinter a CustomTkinter (Sesión 5)

CustomTkinter no reemplaza a Tkinter: lo usa por debajo. Cada `CTkButton` es un widget de Tkinter que dibuja su propio aspecto. Por eso todo lo aprendido en el Módulo 1 sigue valiendo: `mainloop()`, `.pack()`, `.grid()`, `command` y las variables de control.

| Minutos | Actividad |
| --- | --- |
| 0–25 | Comparación visual: el mismo formulario en Tkinter y en CustomTkinter |
| 25–55 | Equivalencias y diferencias de parámetros |
| 55–80 | Tema 2.2 (primera parte): modos de apariencia y temas incluidos |
| 80–120 | Ejercicio 4 |

#### Qué limita a Tkinter estándar

- **Aspecto.** Los widgets clásicos (`tk.Button`, `tk.Entry`) tienen bordes en relieve y no admiten esquinas redondeadas. El módulo `ttk` mejora el aspecto con temas del sistema, pero su personalización es limitada.
- **Modo oscuro.** No existe un interruptor global; hay que cambiar colores widget por widget.
- **Pantallas de alta densidad.** En Windows con escalado al 150 % o más, una aplicación Tkinter sin configuración adicional puede verse borrosa o diminuta. CustomTkinter detecta el escalado del sistema y ajusta el tamaño de sus widgets.

#### Equivalencias

| Tkinter | CustomTkinter | Cambio que hay que recordar |
| --- | --- | --- |
| `tk.Tk()` | `ctk.CTk()` | Sin cambios en `title()`, `geometry()`, `mainloop()` |
| `tk.Frame` | `ctk.CTkFrame` | `bg` pasa a ser `fg_color`; admite `corner_radius` |
| `tk.Label` | `ctk.CTkLabel` | `fg` pasa a ser `text_color` |
| `tk.Entry` | `ctk.CTkEntry` | `width` en píxeles, no en caracteres; admite `placeholder_text` |
| `tk.Button` | `ctk.CTkButton` | `hover_color`, `corner_radius`, `image` |
| `tk.Checkbutton` | `ctk.CTkCheckBox` | Mismo uso de `variable` y `command` |
| `tk.Text` | `ctk.CTkTextbox` | Barra de desplazamiento incluida |
| `font=("Arial", 14, "bold")` | `font=ctk.CTkFont(family="Arial", size=14, weight="bold")` | Se acepta también la tupla |

Tres diferencias que causan errores en clase:

- Los colores pueden ser una tupla `(claro, oscuro)`: `fg_color=("#e8ecf1", "#23272e")`. El widget elige el color según el modo activo.
- Para modificar un widget se usa `configure()`. El alias `config()` de Tkinter no está redefinido en los widgets de CustomTkinter y no debe usarse.
- Con `.place()`, el ancho y el alto van en el constructor (`CTkButton(..., width=120)`), no en `place()`. Pasarlos a `place()` lanza `ValueError`.

#### Código de demostración: `m2_migracion.py`

Un formulario de acceso escrito con las técnicas del Módulo 1, ya migrado. La lógica no cambia; sólo la capa visual.

```python
"""Formulario de acceso migrado de Tkinter a CustomTkinter."""
import customtkinter as ctk

ctk.set_appearance_mode("system")      # sigue el modo del sistema operativo
ctk.set_default_color_theme("blue")    # tema incluido: blue, green o dark-blue


def crear_ventana() -> ctk.CTk:
    """Construye la ventana de acceso."""
    ventana = ctk.CTk()
    ventana.title("Acceso")
    ventana.geometry("380x300")
    ventana.columnconfigure(0, weight=1)

    ctk.CTkLabel(
        ventana, text="Iniciar sesión", font=ctk.CTkFont(size=20, weight="bold")
    ).grid(row=0, column=0, pady=(28, 16))

    correo = ctk.CTkEntry(ventana, placeholder_text="Correo", width=260)
    correo.grid(row=1, column=0, pady=6)
    clave = ctk.CTkEntry(ventana, placeholder_text="Contraseña", show="•", width=260)
    clave.grid(row=2, column=0, pady=6)

    estado = ctk.CTkLabel(ventana, text="")
    estado.grid(row=4, column=0, pady=(6, 0))

    def entrar() -> None:
        """Validación mínima para demostrar la retroalimentación."""
        if not correo.get() or not clave.get():
            estado.configure(text="Completa ambos campos", text_color=("#c0392b", "#ff6b6b"))
        else:
            estado.configure(text="Datos recibidos", text_color=("#1e8449", "#58d68d"))

    ctk.CTkButton(ventana, text="Entrar", width=260, command=entrar).grid(row=3, column=0, pady=12)
    return ventana


if __name__ == "__main__":
    crear_ventana().mainloop()
```

Para la comparación visual, conviene proyectar la versión Tkinter del Módulo 1 y ésta, una junto a otra, con el sistema en modo oscuro.

### Tema 2.2 · Temas y apariencia (Sesiones 5 y 6)

CustomTkinter separa dos decisiones: el **modo de apariencia** (claro u oscuro) y el **tema de color** (qué colores usa cada widget en cada modo). El modo puede cambiarse con la aplicación abierta; el tema se fija al arrancar.

| Función | Valores | Cuándo llamarla |
| --- | --- | --- |
| `ctk.set_appearance_mode(modo)` | `"light"`, `"dark"`, `"system"` | En cualquier momento; la interfaz se redibuja al instante |
| `ctk.get_appearance_mode()` | Devuelve `"Light"` o `"Dark"` | Para saber qué modo está activo |
| `ctk.set_default_color_theme(tema)` | `"blue"`, `"green"`, `"dark-blue"` o la ruta a un `.json` | Antes de crear cualquier widget; los ya creados no cambian |
| `ctk.set_widget_scaling(factor)` | `1.0` = 100 %, `1.25` = 125 % | En cualquier momento; útil para accesibilidad |

#### Código de demostración: `m2_apariencia.py`

```python
"""Cambio de modo de apariencia y escala en tiempo de ejecución."""
import customtkinter as ctk

ctk.set_default_color_theme("green")  # antes de crear widgets

ventana = ctk.CTk()
ventana.title("Apariencia")
ventana.geometry("360x260")

modo_oscuro = ctk.BooleanVar(value=ctk.get_appearance_mode() == "Dark")


def cambiar_modo() -> None:
    """Callback del interruptor: alterna entre claro y oscuro."""
    ctk.set_appearance_mode("dark" if modo_oscuro.get() else "light")


def cambiar_escala(opcion: str) -> None:
    """CTkOptionMenu entrega la opción elegida como texto, por ejemplo \"125 %\"."""
    ctk.set_widget_scaling(int(opcion.rstrip(" %")) / 100)


ctk.CTkSwitch(ventana, text="Modo oscuro", variable=modo_oscuro, command=cambiar_modo).pack(
    pady=(32, 16)
)
ctk.CTkLabel(ventana, text="Tamaño de la interfaz").pack()
ctk.CTkOptionMenu(
    ventana, values=["80 %", "100 %", "125 %", "150 %"], command=cambiar_escala
).pack(pady=8)
ctk.CTkButton(ventana, text="Botón de muestra").pack(pady=16)

ventana.mainloop()
```

El `CTkOptionMenu` arranca mostrando su primer valor ("80 %") aunque la escala real sea 100 %. Es un buen momento para preguntar al grupo cómo corregirlo (respuesta: `menu.set("100 %")` después de crearlo) y relacionarlo con la retroalimentación del Módulo 1.

#### Tema JSON propio (Sesión 6)

Un tema es un archivo JSON con una entrada por widget. Cada color es una lista `[claro, oscuro]`. Escribirlo desde cero es propenso a errores; el procedimiento en clase es:

1. Copiar `blue.json` de la carpeta de temas de CustomTkinter al proyecto, con otro nombre: `tema_institucional.json`.
2. Buscar y reemplazar los colores de acento (los azules) por los de la institución. Las entradas que más impacto tienen son `CTkButton`, `CTkSwitch`, `CTkCheckBox`, `CTkSlider`, `CTkProgressBar` y `CTkSegmentedButton`.
3. Cargarlo con `ctk.set_default_color_theme("tema_institucional.json")` antes de crear la ventana.

Fragmento de la entrada de botones con un acento guinda:

```json
"CTkButton": {
  "corner_radius": 6,
  "border_width": 0,
  "fg_color": ["#7a1f3d", "#9c2a50"],
  "hover_color": ["#5e172f", "#7a1f3d"],
  "border_color": ["#3E454A", "#949A9F"],
  "text_color": ["#DCE4EE", "#DCE4EE"],
  "text_color_disabled": ["gray74", "gray60"]
}
```

Si falta una clave, CustomTkinter falla al crear el primer widget que la necesita, con un `KeyError` que nombra la clave. Por eso se parte de una copia completa y sólo se editan valores.

Regla de contraste para revisar el tema: texto sobre el color de acento con una relación de al menos 4.5:1 (criterio WCAG 2.1 nivel AA para texto normal). Los participantes pueden comprobarlo con cualquier verificador de contraste en línea.

### Tema 2.3 · Widgets modernos (Sesión 6)

Los widgets de selección evitan que el usuario escriba lo que puede elegir, y con eso eliminan una buena parte de la validación. El punto que más confunde es la firma del callback: algunos widgets envían el valor elegido y otros no envían nada.

| Widget | Parámetros clave | El `command` recibe | Métodos |
| --- | --- | --- | --- |
| `CTkButton` | `text`, `command`, `width`, `height`, `corner_radius`, `fg_color`, `hover_color`, `state` | Nada | `configure()`, `invoke()` |
| `CTkEntry` | `placeholder_text`, `show`, `width`, `textvariable`, `state` | — | `get()`, `insert()`, `delete()` |
| `CTkOptionMenu` | `values`, `command`, `variable` | El texto elegido (`str`) | `get()`, `set()` |
| `CTkComboBox` | `values`, `command`, `variable`, `state` | El texto elegido en la lista (`str`) | `get()`, `set()` |
| `CTkSwitch` | `text`, `variable`, `onvalue`, `offvalue`, `command` | Nada | `get()`, `select()`, `deselect()`, `toggle()` |
| `CTkSlider` | `from_`, `to`, `number_of_steps`, `command`, `variable` | El valor (`float`) | `get()`, `set()` |

Diferencias que conviene subrayar:

- `CTkOptionMenu` sólo permite elegir. `CTkComboBox` además deja escribir, y su `command` no se dispara al escribir, sólo al elegir de la lista. Si no se quiere texto libre, se usa `CTkOptionMenu` o `CTkComboBox(..., state="readonly")`.
- `CTkSlider` entrega un `float` aunque tenga `number_of_steps`. Para mostrar enteros hay que convertir con `round()` o `int()`.
- En `CTkEntry`, el `placeholder_text` no se muestra si el campo tiene `textvariable`. Cuando se necesitan ambos, se lee el campo con `get()` en lugar de usar una variable.

#### Código de demostración: `m2_widgets.py`

Un configurador de reporte: cada control actualiza un resumen en vivo, aplicando la retroalimentación del Módulo 1 con widgets nuevos.

```python
"""Widgets de selección de CustomTkinter con resumen en vivo."""
import customtkinter as ctk

FORMATOS = ["PDF", "Excel", "CSV"]
PERIODOS = ["Semanal", "Mensual", "Trimestral"]

ventana = ctk.CTk()
ventana.title("Configurar reporte")
ventana.geometry("420x420")
ventana.columnconfigure(1, weight=1)

formato = ctk.StringVar(value=FORMATOS[0])
periodo = ctk.StringVar(value=PERIODOS[1])
incluir_graficas = ctk.StringVar(value="sí")
registros = ctk.IntVar(value=50)

resumen = ctk.CTkLabel(ventana, text="", justify="left", anchor="w")


def actualizar_resumen(*_args: object) -> None:
    """Compone el texto del resumen a partir de las variables."""
    resumen.configure(
        text=(
            f"Formato: {formato.get()}\n"
            f"Periodo: {periodo.get()}\n"
            f"Gráficas: {incluir_graficas.get()}\n"
            f"Registros por página: {registros.get()}"
        )
    )


def al_mover_slider(valor: float) -> None:
    """El slider entrega float; se guarda como entero."""
    registros.set(round(valor))


filas = [
    ("Formato", ctk.CTkOptionMenu(ventana, values=FORMATOS, variable=formato)),
    ("Periodo", ctk.CTkComboBox(ventana, values=PERIODOS, variable=periodo, state="readonly")),
    (
        "Gráficas",
        ctk.CTkSwitch(
            ventana, text="", variable=incluir_graficas, onvalue="sí", offvalue="no"
        ),
    ),
    (
        "Registros",
        ctk.CTkSlider(
            ventana, from_=10, to=100, number_of_steps=9, command=al_mover_slider
        ),
    ),
]
for fila, (texto, widget) in enumerate(filas):
    ctk.CTkLabel(ventana, text=texto).grid(row=fila, column=0, sticky="w", padx=(20, 12), pady=10)
    widget.grid(row=fila, column=1, sticky="ew", padx=(0, 20), pady=10)

filas[3][1].set(registros.get())  # posición inicial del slider

resumen.grid(row=len(filas), column=0, columnspan=2, sticky="ew", padx=20, pady=(16, 8))
ctk.CTkButton(ventana, text="Generar").grid(row=len(filas) + 1, column=0, columnspan=2, pady=12)

for variable in (formato, periodo, incluir_graficas, registros):
    variable.trace_add("write", actualizar_resumen)
actualizar_resumen()

ventana.mainloop()
```

`ctk.StringVar` y las demás son las mismas clases de `tkinter`, reexportadas por CustomTkinter para no tener que importar ambos módulos. Conviene decirlo para que el grupo no piense que hay que aprender variables nuevas.

### Tema 2.4 · Organización modular (Sesión 7)

Una pantalla compleja se diseña como cajas dentro de cajas. Cada `CTkFrame` es una región con su propio gestor de geometría, lo que resuelve la regla del Módulo 1 (un gestor por contenedor) sin renunciar a combinar `pack` y `grid` en la misma ventana.

| Minutos | Actividad |
| --- | --- |
| 0–25 | Boceto: dividir una pantalla real en regiones y decidir el gestor de cada una |
| 25–60 | `CTkFrame` y `CTkTabview`: demostración |
| 60–80 | `CTkScrollableFrame` y sus límites |
| 80–120 | Ejercicio 6 |

#### Contenedores

| Contenedor | Uso | Detalles |
| --- | --- | --- |
| `CTkFrame` | Agrupar widgets en una región: barra lateral, encabezado, tarjeta | `fg_color="transparent"` crea una agrupación invisible; `corner_radius=0` para barras pegadas al borde |
| `CTkTabview` | Varias vistas en el mismo espacio | `add("Nombre")` devuelve el frame de la pestaña; `set()` cambia de pestaña; `get()` dice cuál está activa; `command` se llama sin argumentos al cambiar |
| `CTkScrollableFrame` | Contenido más alto (o ancho) que el espacio disponible | Los widgets se colocan dentro como en un frame normal; `label_text` añade un título; `orientation="horizontal"` para desplazamiento lateral |

Límite práctico: `CTkScrollableFrame` crea un widget real por cada elemento. Con unos cientos de filas la interfaz se vuelve lenta. Para listas grandes de datos tabulares se usa `ttk.Treeview`, que se estudia en el Módulo 3.

#### Código de demostración: `m2_ajustes.py`

Una ventana de ajustes con pestañas. La pestaña de notificaciones tiene más opciones de las que caben y usa un área desplazable.

```python
"""Ventana de ajustes con CTkTabview y CTkScrollableFrame."""
import customtkinter as ctk

EVENTOS = [
    "Nuevo usuario", "Inicio de sesión fallido", "Respaldo completado",
    "Respaldo fallido", "Stock bajo", "Pedido recibido", "Pedido cancelado",
    "Pago confirmado", "Reporte mensual listo", "Actualización disponible",
    "Licencia por vencer", "Mensaje de soporte",
]

ventana = ctk.CTk()
ventana.title("Ajustes")
ventana.geometry("480x420")


def al_cambiar_pestana() -> None:
    """command de CTkTabview: no recibe argumentos; se consulta get()."""
    ventana.title(f"Ajustes · {pestanas.get()}")


pestanas = ctk.CTkTabview(ventana, command=al_cambiar_pestana)
pestanas.pack(fill="both", expand=True, padx=16, pady=16)

# --- Pestaña General: grid dentro del frame de la pestaña ---
general = pestanas.add("General")
general.columnconfigure(1, weight=1)
ctk.CTkLabel(general, text="Nombre de la organización").grid(row=0, column=0, sticky="w", pady=8)
ctk.CTkEntry(general).grid(row=0, column=1, sticky="ew", padx=(12, 0), pady=8)
ctk.CTkLabel(general, text="Idioma").grid(row=1, column=0, sticky="w", pady=8)
ctk.CTkOptionMenu(general, values=["Español", "English"]).grid(
    row=1, column=1, sticky="w", padx=(12, 0), pady=8
)

# --- Pestaña Notificaciones: pack dentro de un área desplazable ---
notificaciones = pestanas.add("Notificaciones")
lista = ctk.CTkScrollableFrame(notificaciones, label_text="Avisarme cuando ocurra...")
lista.pack(fill="both", expand=True)

activos: dict[str, ctk.BooleanVar] = {}
for evento in EVENTOS:
    activos[evento] = ctk.BooleanVar(value=True)
    ctk.CTkSwitch(lista, text=evento, variable=activos[evento]).pack(anchor="w", pady=4)

pestanas.set("General")

ventana.mainloop()
```

Pregunta de cierre para el grupo: ¿qué gestor usa la ventana, cuál la pestaña General y cuál la lista? (`pack`, `grid` y `pack`). Tres gestores distintos conviven sin error porque cada uno vive en su propio contenedor.

### Entregable del Módulo 2 · Rediseño de un dashboard administrativo

La Sesión 8 empieza con 20 minutos de boceto en papel: cada participante divide la pantalla en regiones y anota qué contenedor y qué gestor usará en cada una. El instructor aprueba el boceto antes de que se escriba código; así se detectan a tiempo los diseños que no se adaptarán al redimensionar.

Enunciado para el participante: construir con CustomTkinter el panel principal de un sistema administrativo (el dominio es libre: escuela, clínica, tienda) con estos elementos.

- Barra lateral con el nombre del sistema, al menos cuatro botones de navegación y un interruptor de modo oscuro al pie. El botón de la sección activa se distingue de los demás y el título del encabezado cambia al elegir otra sección. No hace falta construir las otras pantallas.
- Encabezado con título y campo de búsqueda.
- Fila de cuatro tarjetas de indicadores con título, valor destacado y detalle; las cuatro del mismo ancho.
- `CTkTabview` con dos pestañas como mínimo; una contiene una lista desplazable de actividad reciente con 10 elementos o más.
- Tema JSON propio con el color de acento del dominio elegido.
- Al agrandar la ventana, el área principal crece y la barra lateral mantiene su ancho.

Se entregan el `.py`, el tema `.json`, la foto del boceto y dos capturas (modo claro y oscuro).

#### Rúbrica

| Criterio | Peso | Excelente (100 %) | Suficiente (70 %) | Insuficiente (30 %) |
| --- | --- | --- | --- | --- |
| Estructura con contenedores | 25 % | Todas las regiones pedidas, cada una en su frame; pestañas y lista desplazable funcionan | Falta una región o la lista no se desplaza | Faltan dos o más regiones |
| Tema y modo de apariencia | 20 % | Tema JSON propio aplicado; ambos modos legibles, con colores en tupla (claro, oscuro) | Tema propio, pero un modo con problemas de contraste | Tema incluido sin cambios |
| Jerarquía y consistencia | 20 % | Valores destacados, textos secundarios atenuados, espaciado uniforme, sección activa visible | Un problema de jerarquía o espaciado | Todo el texto con el mismo peso; espaciado irregular |
| Adaptación al tamaño | 15 % | Área principal crece, tarjetas de igual ancho, lateral fijo | Crece pero las tarjetas quedan desiguales | No se adapta |
| Calidad del código | 20 % | PEP 8, type hints, docstrings, funciones auxiliares para elementos repetidos | Código correcto pero repetitivo | Código difícil de seguir |

#### Solución de referencia: `m2_entregable.py`

Usa el tema `blue` para que funcione sin archivos extra. Los colores de la sección activa se leen del tema cargado con `ctk.ThemeManager.theme`, de modo que al cambiar al tema propio el panel se adapta solo.

```python
"""Solución de referencia del entregable del Módulo 2: dashboard administrativo."""
from typing import Callable

import customtkinter as ctk

ctk.set_appearance_mode("system")
ctk.set_default_color_theme("blue")  # el participante usa aquí su tema .json

SECCIONES = ["Inicio", "Usuarios", "Reportes", "Configuración"]
INDICADORES = [
    ("Usuarios activos", "1 248", "+4.2 % frente al mes anterior"),
    ("Ventas del mes", "$86,400", "+12 % frente al mes anterior"),
    ("Tickets abiertos", "37", "8 menos que la semana pasada"),
    ("Tiempo de respuesta", "2.4 h", "Meta: 3 h"),
]
ACTIVIDAD = [
    ("09:12", "Ana López creó el usuario jperez"),
    ("09:30", "Respaldo diario completado"),
    ("10:05", "Se registró el pedido 1042"),
    ("10:18", "Luis Ramos cerró el ticket 311"),
    ("10:47", "Stock bajo: Silla ergonómica (5 piezas)"),
    ("11:02", "Se generó el reporte semanal de ventas"),
    ("11:26", "Inicio de sesión fallido para admin"),
    ("12:10", "Pago confirmado del pedido 1042"),
    ("12:44", "María Soto actualizó la categoría Oficina"),
    ("13:15", "Se registró el pedido 1043"),
    ("13:38", "Luis Ramos abrió el ticket 318"),
    ("14:01", "Actualización disponible: versión 1.3"),
]
COLOR_LATERAL = ("#e3e7ec", "#1d2025")
COLOR_TARJETA = ("#ffffff", "#2a2d33")
COLOR_SECUNDARIO = ("gray40", "gray65")


def crear_tarjeta(padre: ctk.CTkFrame, titulo: str, valor: str, detalle: str) -> ctk.CTkFrame:
    """Tarjeta de indicador. La jerarquía se marca con tamaño, peso y color."""
    tarjeta = ctk.CTkFrame(padre, fg_color=COLOR_TARJETA, corner_radius=12)
    ctk.CTkLabel(tarjeta, text=titulo, text_color=COLOR_SECUNDARIO).pack(
        anchor="w", padx=16, pady=(14, 0)
    )
    ctk.CTkLabel(tarjeta, text=valor, font=ctk.CTkFont(size=26, weight="bold")).pack(
        anchor="w", padx=16
    )
    ctk.CTkLabel(
        tarjeta, text=detalle, font=ctk.CTkFont(size=12), text_color=COLOR_SECUNDARIO
    ).pack(anchor="w", padx=16, pady=(0, 14))
    return tarjeta


def crear_lateral(ventana: ctk.CTk, al_elegir: Callable[[str], None]) -> dict[str, ctk.CTkButton]:
    """Barra lateral fija con navegación e interruptor de modo oscuro."""
    lateral = ctk.CTkFrame(ventana, corner_radius=0, fg_color=COLOR_LATERAL)
    lateral.grid(row=0, column=0, sticky="ns")

    ctk.CTkLabel(lateral, text="Mi Empresa", font=ctk.CTkFont(size=20, weight="bold")).pack(
        padx=16, pady=(24, 20)
    )
    botones: dict[str, ctk.CTkButton] = {}
    for seccion in SECCIONES:
        boton = ctk.CTkButton(
            lateral,
            text=seccion,
            anchor="w",
            width=168,  # el ancho de los botones fija el ancho de la barra
            height=36,
            fg_color="transparent",
            text_color=("gray10", "gray90"),
            hover_color=("gray75", "gray30"),
            command=lambda s=seccion: al_elegir(s),
        )
        boton.pack(padx=16, pady=3)
        botones[seccion] = boton

    modo_oscuro = ctk.BooleanVar(value=ctk.get_appearance_mode() == "Dark")
    ctk.CTkSwitch(
        lateral,
        text="Modo oscuro",
        variable=modo_oscuro,
        command=lambda: ctk.set_appearance_mode("dark" if modo_oscuro.get() else "light"),
    ).pack(side="bottom", padx=16, pady=20)
    return botones


def crear_pestanas(padre: ctk.CTkFrame) -> ctk.CTkTabview:
    """Pestañas de actividad reciente y ajustes rápidos."""
    pestanas = ctk.CTkTabview(padre)

    actividad = pestanas.add("Actividad reciente")
    lista = ctk.CTkScrollableFrame(actividad, fg_color="transparent")
    lista.pack(fill="both", expand=True)
    lista.columnconfigure(1, weight=1)
    for fila, (hora, texto) in enumerate(ACTIVIDAD):
        ctk.CTkLabel(lista, text=hora, text_color=COLOR_SECUNDARIO, width=60, anchor="w").grid(
            row=fila, column=0, sticky="w", pady=4
        )
        ctk.CTkLabel(lista, text=texto, anchor="w").grid(row=fila, column=1, sticky="w", pady=4)

    ajustes = pestanas.add("Ajustes rápidos")
    ctk.CTkSwitch(ajustes, text="Notificaciones por correo").pack(anchor="w", padx=8, pady=8)
    ctk.CTkLabel(ajustes, text="Escala de la interfaz").pack(anchor="w", padx=8, pady=(16, 4))
    escala = ctk.CTkOptionMenu(
        ajustes,
        values=["90 %", "100 %", "115 %"],
        command=lambda opcion: ctk.set_widget_scaling(int(opcion.rstrip(" %")) / 100),
    )
    escala.set("100 %")
    escala.pack(anchor="w", padx=8)
    return pestanas


def main() -> None:
    """Ensambla las regiones del dashboard."""
    ventana = ctk.CTk()
    ventana.title("Panel administrativo")
    ventana.geometry("1100x680")
    ventana.minsize(900, 560)
    ventana.columnconfigure(1, weight=1)  # sólo el área principal crece
    ventana.rowconfigure(0, weight=1)

    principal = ctk.CTkFrame(ventana, fg_color="transparent")
    principal.grid(row=0, column=1, sticky="nsew", padx=24, pady=20)
    principal.columnconfigure(0, weight=1)
    principal.rowconfigure(2, weight=1)

    encabezado = ctk.CTkFrame(principal, fg_color="transparent")
    encabezado.grid(row=0, column=0, sticky="ew")
    titulo = ctk.CTkLabel(encabezado, text="", font=ctk.CTkFont(size=24, weight="bold"))
    titulo.pack(side="left")
    ctk.CTkEntry(encabezado, placeholder_text="Buscar...", width=240).pack(side="right")

    tema_boton = ctk.ThemeManager.theme["CTkButton"]

    def elegir_seccion(nombre: str) -> None:
        """Resalta el botón activo con los colores del tema y cambia el título."""
        for seccion, boton in botones.items():
            if seccion == nombre:
                boton.configure(fg_color=tema_boton["fg_color"], text_color=tema_boton["text_color"])
            else:
                boton.configure(fg_color="transparent", text_color=("gray10", "gray90"))
        titulo.configure(text=nombre)

    botones = crear_lateral(ventana, elegir_seccion)

    indicadores = ctk.CTkFrame(principal, fg_color="transparent")
    indicadores.grid(row=1, column=0, sticky="ew", pady=20)
    for columna, (texto, valor, detalle) in enumerate(INDICADORES):
        # uniform obliga a que las cuatro columnas tengan el mismo ancho.
        indicadores.columnconfigure(columna, weight=1, uniform="tarjetas")
        crear_tarjeta(indicadores, texto, valor, detalle).grid(
            row=0, column=columna, sticky="ew", padx=(0 if columna == 0 else 12, 0)
        )

    crear_pestanas(principal).grid(row=2, column=0, sticky="nsew")

    elegir_seccion("Inicio")
    ventana.mainloop()


if __name__ == "__main__":
    main()
```

Puntos que suelen fallar al revisar: tarjetas de ancho distinto (falta `uniform`), textos secundarios ilegibles en modo oscuro (color único en lugar de tupla) y botón activo con un azul fijo que no cambia al aplicar el tema propio.

### Ejercicios de clase con solución

#### Ejercicio 4 · Migrar el conversor a CustomTkinter (Sesión 5, 25 min)

Enunciado: tomar el conversor de temperatura (Ejercicio 1) y pasarlo a CustomTkinter. El campo debe mostrar el texto guía "Grados Celsius" y el mensaje de error debe leerse bien en ambos modos.

```python
"""Solución E4: conversor de temperatura migrado a CustomTkinter."""
import customtkinter as ctk

ctk.set_appearance_mode("system")

COLOR_ERROR = ("#c0392b", "#ff6b6b")
COLOR_NORMAL = ("gray10", "gray90")


def celsius_a_fahrenheit(celsius: float) -> float:
    """Convierte grados Celsius a Fahrenheit."""
    return celsius * 9 / 5 + 32


ventana = ctk.CTk()
ventana.title("Conversor °C a °F")
ventana.geometry("320x240")

# Sin textvariable: así el placeholder sí se muestra.
campo = ctk.CTkEntry(ventana, placeholder_text="Grados Celsius", justify="center", width=200)
resultado = ctk.CTkLabel(ventana, text="", font=ctk.CTkFont(size=18, weight="bold"))


def convertir() -> None:
    """Lee el campo, convierte y muestra el resultado o el error."""
    try:
        grados = float(campo.get().replace(",", "."))
    except ValueError:
        resultado.configure(text="Escribe un número", text_color=COLOR_ERROR)
        return
    resultado.configure(text=f"{celsius_a_fahrenheit(grados):.1f} °F", text_color=COLOR_NORMAL)


campo.pack(pady=(40, 12))
ctk.CTkButton(ventana, text="Convertir", command=convertir).pack()
resultado.pack(pady=16)

ventana.mainloop()
```

Lo que se evalúa: `config` cambiado por `configure`, `fg` por `text_color`, colores en tupla y la decisión consciente de quitar `textvariable`.

#### Ejercicio 5 · Panel de preferencias con vista previa (Sesión 6, 35 min)

Enunciado: un panel con idioma (`CTkOptionMenu`), ciudad (`CTkComboBox` editable), tamaño de texto de 10 a 24 pt (`CTkSlider`) y modo oscuro (`CTkSwitch`). Una etiqueta de muestra refleja todos los cambios al instante. Un botón "Restablecer" devuelve todo a los valores iniciales.

```python
"""Solución E5: panel de preferencias con vista previa."""
import customtkinter as ctk

IDIOMAS = ["Español", "English", "Português"]
CIUDADES = ["Ciudad de México", "Guadalajara", "Monterrey"]
SALUDOS = {"Español": "Hola", "English": "Hello", "Português": "Olá"}
TAMANO_INICIAL = 14

ctk.set_appearance_mode("light")

ventana = ctk.CTk()
ventana.title("Preferencias")
ventana.geometry("460x400")
ventana.columnconfigure(1, weight=1)

idioma = ctk.StringVar(value=IDIOMAS[0])
ciudad = ctk.StringVar(value=CIUDADES[1])
tamano = ctk.IntVar(value=TAMANO_INICIAL)
oscuro = ctk.BooleanVar(value=False)

# Un CTkFont compartido: al cambiar su tamaño, todos los widgets que lo usan se actualizan.
fuente_muestra = ctk.CTkFont(size=TAMANO_INICIAL)
muestra = ctk.CTkLabel(ventana, text="", font=fuente_muestra, wraplength=400)


def actualizar_muestra(*_args: object) -> None:
    """Refleja las preferencias actuales en la etiqueta de muestra."""
    fuente_muestra.configure(size=tamano.get())
    muestra.configure(text=f"{SALUDOS[idioma.get()]} desde {ciudad.get()} ({tamano.get()} pt)")


def al_mover(valor: float) -> None:
    """El slider entrega float; se guarda como entero."""
    tamano.set(round(valor))


def cambiar_modo() -> None:
    """Aplica el modo según el interruptor."""
    ctk.set_appearance_mode("dark" if oscuro.get() else "light")


deslizador = ctk.CTkSlider(ventana, from_=10, to=24, number_of_steps=14, command=al_mover)
deslizador.set(TAMANO_INICIAL)
controles = [
    ("Idioma", ctk.CTkOptionMenu(ventana, values=IDIOMAS, variable=idioma)),
    ("Ciudad", ctk.CTkComboBox(ventana, values=CIUDADES, variable=ciudad)),
    ("Tamaño de texto", deslizador),
    ("Modo oscuro", ctk.CTkSwitch(ventana, text="", variable=oscuro, command=cambiar_modo)),
]
for fila, (texto, control) in enumerate(controles):
    ctk.CTkLabel(ventana, text=texto).grid(row=fila, column=0, sticky="w", padx=(20, 12), pady=10)
    control.grid(row=fila, column=1, sticky="ew", padx=(0, 20), pady=10)


def restablecer() -> None:
    """Cambiar una variable no dispara el command del widget: se llama a mano."""
    idioma.set(IDIOMAS[0])
    ciudad.set(CIUDADES[1])
    tamano.set(TAMANO_INICIAL)
    deslizador.set(TAMANO_INICIAL)
    oscuro.set(False)
    cambiar_modo()


muestra.grid(row=len(controles), column=0, columnspan=2, pady=(20, 8))
ctk.CTkButton(ventana, text="Restablecer", command=restablecer).grid(
    row=len(controles) + 1, column=0, columnspan=2, pady=12
)

for variable in (idioma, ciudad, tamano):
    variable.trace_add("write", actualizar_muestra)
actualizar_muestra()

ventana.mainloop()
```

Dos detalles para comentar en plenaria: el slider no tiene `variable` y por eso `restablecer()` lo mueve con `set()`; y `oscuro.set(False)` cambia el interruptor pero no ejecuta `cambiar_modo()`, que hay que llamar explícitamente.

#### Ejercicio 6 · Lista de tareas desplazable (Sesión 7, 35 min)

Enunciado: un campo y un botón "Agregar" (también con Enter) crean una casilla por tarea dentro de un `CTkScrollableFrame`. Un contador muestra "n de m completadas" y un botón "Limpiar completadas" elimina las marcadas.

```python
"""Solución E6: lista de tareas en un área desplazable."""
import customtkinter as ctk

ventana = ctk.CTk()
ventana.title("Tareas")
ventana.geometry("420x480")
ventana.columnconfigure(0, weight=1)
ventana.rowconfigure(1, weight=1)

tareas: list[tuple[ctk.CTkCheckBox, ctk.BooleanVar]] = []

entrada = ctk.CTkEntry(ventana, placeholder_text="Nueva tarea")
entrada.grid(row=0, column=0, sticky="ew", padx=(16, 8), pady=16)
lista = ctk.CTkScrollableFrame(ventana, label_text="Pendientes")
lista.grid(row=1, column=0, columnspan=2, sticky="nsew", padx=16)
contador = ctk.CTkLabel(ventana, text="")
contador.grid(row=2, column=0, sticky="w", padx=16, pady=12)


def actualizar_contador() -> None:
    """Cuenta las casillas marcadas."""
    hechas = sum(completada.get() for _casilla, completada in tareas)
    contador.configure(text=f"{hechas} de {len(tareas)} completadas")


def agregar() -> None:
    """Crea una casilla nueva con el texto del campo."""
    texto = entrada.get().strip()
    if not texto:
        return
    completada = ctk.BooleanVar(value=False)
    casilla = ctk.CTkCheckBox(lista, text=texto, variable=completada, command=actualizar_contador)
    casilla.pack(anchor="w", pady=4)
    tareas.append((casilla, completada))
    entrada.delete(0, "end")
    actualizar_contador()


def limpiar_completadas() -> None:
    """destroy() quita el widget de la pantalla; también se quita de la lista."""
    for casilla, completada in list(tareas):
        if completada.get():
            casilla.destroy()
            tareas.remove((casilla, completada))
    actualizar_contador()


ctk.CTkButton(ventana, text="Agregar", width=90, command=agregar).grid(
    row=0, column=1, padx=(0, 16)
)
ctk.CTkButton(ventana, text="Limpiar completadas", command=limpiar_completadas).grid(
    row=2, column=1, padx=(0, 16), pady=12
)
entrada.bind("<Return>", lambda _evento: agregar())
actualizar_contador()

ventana.mainloop()
```

Error típico: recorrer `tareas` y borrar de ella en el mismo ciclo. Se saltan elementos; por eso la solución itera sobre una copia (`list(tareas)`).

### Errores frecuentes del Módulo 2

| Síntoma | Causa | Solución |
| --- | --- | --- |
| `ModuleNotFoundError: No module named 'customtkinter'` | El paquete se instaló en otro entorno o el `venv` no está activo | Activar el entorno e instalar ahí; en VS Code, elegir el intérprete del `venv` |
| `ValueError: ['bg'] are not supported arguments` | Parámetros de Tkinter clásico (`bg`, `fg`, `relief`) en un widget CTk | Usar `fg_color`, `text_color`, `border_width` |
| `ValueError` al usar `place(width=...)` | CustomTkinter no acepta tamaño en `place()` | Pasar `width` y `height` al constructor |
| El tema propio no se aplica | `set_default_color_theme()` se llamó después de crear widgets | Llamarlo antes de `ctk.CTk()` |
| `KeyError` al crear el primer widget con un tema propio | Falta una clave en el `.json` | Partir de una copia completa de `blue.json` y editar sólo valores |
| El texto guía de `CTkEntry` no aparece | El campo tiene `textvariable` | Quitar la variable y leer con `get()` |
| El valor del slider se ve como `37.77777` | `CTkSlider` entrega `float` | Convertir con `round()` |
| Texto ilegible en modo oscuro | Color único en lugar de tupla `(claro, oscuro)` | Definir los colores como tupla |
| La lista desplazable se vuelve lenta | Cientos de widgets dentro de `CTkScrollableFrame` | Usar `ttk.Treeview` (Módulo 3) para datos tabulares |
