# Manual del Instructor · Curso-Taller de Interfaces Gráficas con Python

Sep 27, 2026 · @Noe Cazarez Camargo

### Módulo 3 · Programación orientada a eventos y diálogos

1. **3.1 Eventos de ratón y teclado.** Secuencias (`<Button-1>`, `<Double-Button-1>`, `<Return>`, `<KeyRelease>`), el objeto `event` y la propagación.
2. **3.2 Callbacks con argumentos.** `lambda`, el error de enlace tardío en ciclos y `functools.partial`.
3. **3.3 Diálogos.** `messagebox` y `CTkMessagebox` (paquete externo), `filedialog.askopenfilename` / `asksaveasfilename`, `colorchooser.askcolor` y selección de fechas con `tkcalendar` (paquete externo).
4. **3.4 Tablas con `ttk.Treeview`.** Columnas, inserción, actualización, ordenamiento al hacer clic en el encabezado y selección de filas. Entregable: explorador de archivos local con vista tabular y visor de contenido.

### Módulo 4 · Arquitectura UI, hilos y multiventana

1. **4.1 Ventanas como clases.** `class MainWindow(ctk.CTk)`, frames como componentes reutilizables.
2. **4.2 Separar vista y lógica.** Patrón MVC aplicado a escritorio: la vista no calcula ni guarda datos.
3. **4.3 Varias ventanas.** `CTkToplevel` modal (`transient()`, `grab_set()`, `wait_window()`) y paso de datos entre ventanas.
4. **4.4 Concurrencia.** Por qué se congela la interfaz, `threading.Thread`, comunicación con `queue.Queue` y actualización de la GUI sólo desde el hilo principal mediante `after()`. Entregable: login, panel principal y proceso en segundo plano con `CTkProgressBar`.

### Módulo 5 · Proyecto final, estilos avanzados y empaquetado

1. **5.1 Imágenes.** Pillow (`PIL.Image`) y `CTkImage` con versiones clara y oscura.
2. **5.2 Tipografía e íconos.** Fuentes propias, `CTkFont` e íconos vectoriales exportados a PNG.
3. **5.3 Integración y accesibilidad.** Contraste, tamaño de texto, orden de tabulación y atajos de teclado.
4. **5.4 Empaquetado con PyInstaller.** `--onedir` / `--onefile`, `--windowed`, `--add-data`, `--icon`, rutas de recursos con `sys._MEIPASS` y la carpeta de temas de CustomTkinter.
5. **5.5 Distribución.** Ícono de la aplicación y acceso directo. Entregable final: aplicación ejecutable con documentación de componentes.

## Módulo 1 · Fundamentos de UI/UX y anatomía de Tkinter

En 8 horas el participante pasa de una ventana vacía a un formulario que valida cada campo mientras se escribe. El módulo usa sólo Tkinter estándar: CustomTkinter entra en el Módulo 2, cuando el grupo ya entiende qué hay debajo.

### Objetivos de aprendizaje

Al cerrar el módulo, el participante:

- Explica qué hace `mainloop()` y por qué el código escrito después de él sólo corre al cerrar la ventana.
- Aplica tres principios de UI/UX (consistencia, jerarquía visual, retroalimentación) al revisar una pantalla.
- Elige entre `.pack()`, `.grid()` y `.place()` y construye layouts que se adaptan al redimensionar.
- Lee y escribe el estado de la interfaz con variables de control.
- Valida entradas con `validatecommand` (antes de aceptar la tecla) y con `trace_add()` (después del cambio).

### Distribución por sesiones

| Sesión | Duración | Contenido | Actividad práctica |
| --- | --- | --- | --- |
| 1 | 2 h | Tema 1.1: principios de UI/UX y anatomía de la ventana | Crear, centrar y configurar una ventana; análisis de dos pantallas reales |
| 2 | 2 h | Tema 1.2: `.pack()` y `.grid()` | Ejercicio 1 (pack) y Ejercicio 2 (grid) |
| 3 | 2 h | Tema 1.2: `.place()`. Tema 1.3: widgets y variables de control | Ejercicio 3 (contador de caracteres) |
| 4 | 2 h | Tema 1.4: validación en tiempo real | Desarrollo y revisión del entregable |

### Preparación del instructor

- Verificar en cada equipo que `python -m tkinter` abre la ventana de prueba y anotar la versión de Tk (`import tkinter; print(tkinter.TkVersion)`). Con 8.6 todo el material funciona.
- Tener abiertas dos aplicaciones de escritorio conocidas por el grupo (por ejemplo, el panel de configuración del sistema y un formulario administrativo mal diseñado) para la discusión de UI/UX.
- Preparar los archivos de demostración de esta sección en una carpeta `modulo1/` y proyectarlos con fuente de 16 pt o más.

### Tema 1.1 · Diseño de interfaces y anatomía de la ventana (Sesión 1)

La sesión arranca con diseño y no con código: el grupo debe llegar a la primera ventana sabiendo qué va a evaluar en ella.

| Minutos | Actividad |
| --- | --- |
| 0–20 | Presentación del curso, verificación de entornos |
| 20–50 | Principios de UI/UX con dos pantallas reales |
| 50–90 | Anatomía de la ventana: demostración en vivo |
| 90–120 | Práctica: ventana centrada con título, tamaño mínimo y un botón que da retroalimentación |

#### Principios de UI/UX para escritorio

Se trabajan tres principios. Para cada uno, el instructor proyecta una pantalla real y pide al grupo que señale dónde se cumple y dónde falla.

- **Consistencia.** Los mismos elementos se ven y se comportan igual en toda la aplicación: el botón de confirmar siempre en la misma posición, la misma fuente para todas las etiquetas, el mismo verbo para la misma acción ("Guardar", no "Guardar" en una ventana y "Aceptar" en otra).
- **Jerarquía visual.** El ojo debe encontrar primero lo importante. Se logra con tamaño, peso de fuente, color y espacio. Un título a 18 pt y los campos a 11 pt comunican orden sin necesidad de explicarlo.
- **Retroalimentación.** Cada acción del usuario produce una respuesta visible: un botón que se desactiva mientras procesa, un mensaje junto al campo con error, una barra de estado que confirma "Guardado".

Pregunta para el grupo: ¿qué pasa cuando un botón no responde en 2 segundos? La respuesta habitual ("le doy clic otra vez") sirve para anticipar el Módulo 4, donde se resuelve la interfaz congelada.

#### Anatomía de una ventana

- **Ventana principal (`Tk`).** Una sola por aplicación. Crea el intérprete de Tcl/Tk que da vida a todos los widgets.
- **Barra de título.** La dibuja el sistema operativo, no Tkinter. Se controla su texto con `title()` y su ícono con `iconbitmap()` (Windows) o `iconphoto()`.
- **Área cliente.** El espacio interior donde se colocan los widgets. No debe confundirse con el widget `Canvas`, que es un lienzo de dibujo y se ve en módulos posteriores.
- **Bucle principal (`mainloop`).** Un ciclo que espera eventos (clics, teclas, redibujado) y los despacha a sus manejadores. Mientras corre, la línea siguiente del script no se ejecuta.

#### Código de demostración: `m1_ventana.py`

```python
"""Anatomía mínima de una aplicación Tkinter.

Muestra la creación de la ventana principal, su configuración básica
y el comportamiento bloqueante de mainloop().
"""
import tkinter as tk

ANCHO = 480
ALTO = 320


def centrar_ventana(ventana: tk.Tk, ancho: int, alto: int) -> None:
    """Ubica la ventana en el centro de la pantalla.

    La cadena de geometría tiene el formato "ANCHOxALTO+X+Y".
    """
    x = (ventana.winfo_screenwidth() - ancho) // 2
    y = (ventana.winfo_screenheight() - alto) // 2
    ventana.geometry(f"{ancho}x{alto}+{x}+{y}")


def crear_ventana() -> tk.Tk:
    """Crea y configura la ventana principal."""
    ventana = tk.Tk()
    ventana.title("Mi primera ventana")
    centrar_ventana(ventana, ANCHO, ALTO)
    ventana.minsize(320, 240)  # evita que el usuario la reduzca demasiado

    mensaje = tk.Label(ventana, text="Aún no has hecho clic", font=("Arial", 12))
    mensaje.pack(pady=(60, 12))

    def al_hacer_clic() -> None:
        """Retroalimentación: el usuario ve que su acción tuvo efecto."""
        mensaje.config(text="¡Clic recibido!", fg="#1e8449")

    tk.Button(ventana, text="Haz clic", command=al_hacer_clic).pack()
    return ventana


if __name__ == "__main__":
    app = crear_ventana()
    app.mainloop()
    print("mainloop terminó: la ventana ya se cerró")
```

Puntos a señalar durante la demostración:

- El `print` final aparece en la terminal sólo después de cerrar la ventana. Es la forma más directa de mostrar que `mainloop()` bloquea.
- `command=al_hacer_clic` pasa la función; `command=al_hacer_clic()` la ejecutaría una vez al crear el botón y pasaría `None`. Conviene provocar el error a propósito frente al grupo.
- Si se omite `mainloop()` y el script se ejecuta desde terminal, la ventana aparece y desaparece al instante.

#### Práctica de la sesión

Cada participante modifica `m1_ventana.py` para que: la ventana no pueda redimensionarse en altura (`resizable(True, False)`), el botón cuente cuántos clics lleva y el texto cambie de color al llegar a 5. Se revisa en plenaria qué principio de UI/UX aplica cada cambio.

### Tema 1.2 · Gestores de geometría (Sesiones 2 y 3)

Un widget creado no aparece hasta que un gestor de geometría lo coloca. La regla que más errores evita: dentro de un mismo contenedor se usa un solo gestor. Mezclar `.pack()` y `.grid()` en el mismo padre provoca `TclError`; en cambio, cada `Frame` puede usar el suyo.

| Gestor | Cómo piensa | Úsalo para | Evítalo cuando |
| --- | --- | --- | --- |
| `.pack()` | Apila widgets contra un borde, en el orden en que se llaman | Barras de herramientas, barras de estado, paneles laterales, columnas simples | Necesitas alinear etiquetas y campos en filas y columnas |
| `.grid()` | Rejilla de filas y columnas con pesos | Formularios, teclados, tableros | Solo hay un widget o una pila simple |
| `.place()` | Coordenadas absolutas o relativas al padre | Superposiciones: insignias, marcas de agua, elementos flotantes | Casi siempre: no se adapta al texto ni a cambios de fuente o idioma |

#### `.pack()`: el orden importa

Opciones clave: `side` (top, bottom, left, right), `fill` (x, y, both), `expand` (reparte el espacio sobrante), `padx`/`pady` (margen externo), `ipadx`/`ipady` (relleno interno) y `anchor`.

```python
"""pack: estructura típica de ventana (m1_pack.py)."""
import tkinter as tk

raiz = tk.Tk()
raiz.title("pack: barra, lateral, contenido y estado")
raiz.geometry("560x340")

barra = tk.Frame(raiz, bg="#dfe3e8")
barra.pack(side="top", fill="x")
for texto in ("Nuevo", "Abrir", "Guardar"):
    tk.Button(barra, text=texto).pack(side="left", padx=4, pady=4)

# La barra de estado se empaqueta ANTES que el lateral para ocupar todo el ancho.
estado = tk.Label(raiz, text="Listo", anchor="w", bg="#f2f2f2")
estado.pack(side="bottom", fill="x")

lateral = tk.Frame(raiz, bg="#c9d6e3", width=140)
lateral.pack(side="left", fill="y")

contenido = tk.Frame(raiz, bg="white")
contenido.pack(side="left", fill="both", expand=True, padx=8, pady=8)

raiz.mainloop()
```

Demostración recomendada: mover la línea `estado.pack(...)` al final y redimensionar. La barra de estado queda recortada junto al lateral, porque `pack` reparte el espacio en el orden de las llamadas.

#### `.grid()`: filas, columnas y pesos

Opciones clave: `row`, `column`, `sticky` (combinación de n, s, e, w), `columnspan`, `rowspan`, `padx`, `pady`. Sin `columnconfigure(i, weight=1)` las columnas no crecen al agrandar la ventana; es el olvido más común.

```python
"""grid: formulario que se adapta al ancho (m1_grid.py)."""
import tkinter as tk

raiz = tk.Tk()
raiz.title("grid: formulario de registro")
raiz.columnconfigure(1, weight=1)  # sólo la columna de los campos crece

entradas: dict[str, tk.Entry] = {}
for fila, etiqueta in enumerate(("Nombre", "Correo", "Teléfono")):
    tk.Label(raiz, text=f"{etiqueta}:").grid(
        row=fila, column=0, sticky="e", padx=(12, 6), pady=4
    )
    entrada = tk.Entry(raiz)
    entrada.grid(row=fila, column=1, sticky="ew", padx=(0, 12), pady=4)
    entradas[etiqueta] = entrada

tk.Label(raiz, text="Comentarios:").grid(row=3, column=0, sticky="ne", padx=(12, 6), pady=4)
comentarios = tk.Text(raiz, height=4, width=30)
comentarios.grid(row=3, column=1, sticky="nsew", padx=(0, 12), pady=4)
raiz.rowconfigure(3, weight=1)  # el cuadro de comentarios crece en altura

tk.Button(raiz, text="Guardar").grid(row=4, column=0, columnspan=2, pady=12)

raiz.mainloop()
```

Puntos a señalar: `sticky="e"` alinea las etiquetas a la derecha y deja un borde limpio junto a los campos (jerarquía y consistencia); `sticky="ew"` estira cada campo al ancho de su celda.

#### `.place()`: cuándo sí

Opciones clave: `x`, `y` (píxeles), `relx`, `rely` (0.0 a 1.0), `relwidth`, `relheight` y `anchor`. Se enseña con un caso donde es la herramienta correcta: una insignia sobre otra superficie.

```python
"""place: insignia superpuesta a una tarjeta (m1_place.py)."""
import tkinter as tk

raiz = tk.Tk()
raiz.geometry("360x220")

tarjeta = tk.Frame(raiz, bg="#2b5797")
tarjeta.place(relx=0.5, rely=0.5, anchor="center", relwidth=0.8, relheight=0.7)

tk.Label(
    tarjeta, text="Bandeja de entrada", fg="white", bg="#2b5797", font=("Arial", 14)
).place(relx=0.5, rely=0.5, anchor="center")

insignia = tk.Label(tarjeta, text="3", fg="white", bg="#d9534f", font=("Arial", 10, "bold"))
insignia.place(relx=1.0, rely=0.0, anchor="ne", x=-8, y=8)  # esquina superior derecha

raiz.mainloop()
```

Para cerrar el tema, se reemplaza el formulario de `m1_grid.py` por coordenadas fijas con `place(x=..., y=...)` y se cambia la fuente a 16 pt: los campos se enciman. Esa demostración explica mejor que cualquier regla por qué `place` se reserva para superposiciones.

### Tema 1.3 · Widgets esenciales y variables de control (Sesión 3)

Tres widgets bastan para la mayoría de los formularios: `Label` muestra, `Entry` captura y `Button` dispara una acción. Las variables de control conectan esos widgets con el código sin tener que leer cada uno por separado.

| Widget | Opciones que se usan en el curso | Métodos clave |
| --- | --- | --- |
| `Label` | `text`, `textvariable`, `font`, `fg`, `bg`, `anchor`, `justify`, `wraplength` | `config()` |
| `Entry` | `textvariable`, `width`, `show` (contraseñas), `state` (normal, disabled, readonly), `validate`, `validatecommand` | `get()`, `insert(0, texto)`, `delete(0, tk.END)`, `focus_set()` |
| `Button` | `text`, `command`, `state`, `width` | `config(state=...)`, `invoke()` |

#### Variables de control

| Clase | Tipo en Python | Se usa con |
| --- | --- | --- |
| `StringVar` | `str` | `Label`, `Entry` (`textvariable`) |
| `IntVar` | `int` | Contadores, `Radiobutton`, `Spinbox` |
| `DoubleVar` | `float` | `Scale`, cálculos |
| `BooleanVar` | `bool` | `Checkbutton` (`variable`) |

Todas comparten `get()`, `set(valor)` y `trace_add(modo, callback)`. Dos detalles que hay que enseñar explícitamente:

- Deben crearse después de `tk.Tk()`. Antes de que exista la ventana principal, Python lanza `RuntimeError: Too early to create variable`.
- `IntVar.get()` y `DoubleVar.get()` lanzan `TclError` si el `Entry` asociado está vacío o contiene texto no numérico. Para campos que el usuario escribe es más seguro usar `StringVar` y convertir con `int()` dentro de un `try`.

```python
"""Variables de control: contador, eco y casilla (m1_variables.py)."""
import tkinter as tk

raiz = tk.Tk()
raiz.title("Variables de control")
raiz.columnconfigure(0, weight=1)

# IntVar: el Label se actualiza solo cuando cambia el valor.
contador = tk.IntVar(value=0)
tk.Label(raiz, textvariable=contador, font=("Arial", 24)).grid(row=0, column=0, pady=8)
tk.Button(raiz, text="+1", command=lambda: contador.set(contador.get() + 1)).grid(
    row=1, column=0
)

# StringVar + trace_add: reacciona a cada tecla.
texto = tk.StringVar()
eco = tk.StringVar(value="Escribe algo")


def al_cambiar_texto(*_args: object) -> None:
    """Callback de trace_add: Tk envía (nombre, índice, modo), que aquí no se usan."""
    eco.set(texto.get().upper() or "Escribe algo")


texto.trace_add("write", al_cambiar_texto)
tk.Entry(raiz, textvariable=texto).grid(row=2, column=0, sticky="ew", padx=12, pady=(16, 4))
tk.Label(raiz, textvariable=eco).grid(row=3, column=0)

# BooleanVar: habilita o deshabilita un botón.
acepta = tk.BooleanVar(value=False)
boton = tk.Button(raiz, text="Continuar", state="disabled")


def al_marcar() -> None:
    """Retroalimentación inmediata: el botón sólo se activa con la casilla marcada."""
    boton.config(state="normal" if acepta.get() else "disabled")


tk.Checkbutton(raiz, text="Acepto", variable=acepta, command=al_marcar).grid(
    row=4, column=0, pady=(16, 4)
)
boton.grid(row=5, column=0, pady=(0, 12))

raiz.mainloop()
```

#### Dos formas de validar

Esta comparación abre la Sesión 4 y es la base del entregable.

|  | `validatecommand` | `trace_add("write", ...)` |
| --- | --- | --- |
| Momento | Antes de aceptar el cambio | Después del cambio |
| Puede impedir la tecla | Sí, devolviendo `False` | No |
| Uso típico | Restringir caracteres: sólo dígitos, longitud máxima | Mostrar mensajes de error, habilitar botones |
| Cuidado | Debe devolver siempre `bool`; si lanza una excepción o devuelve otra cosa, Tk desactiva la validación de ese campo sin avisar | El callback recibe tres argumentos; se aceptan con `*args` |

```python
"""validatecommand: campo que sólo acepta hasta 3 dígitos (m1_validacion.py)."""
import tkinter as tk


def solo_digitos(propuesto: str) -> bool:
    """Recibe el texto como quedaría (%P) y decide si se acepta la tecla."""
    return propuesto == "" or (propuesto.isdigit() and len(propuesto) <= 3)


raiz = tk.Tk()
vcmd = (raiz.register(solo_digitos), "%P")  # %P = valor propuesto
tk.Label(raiz, text="Edad:").pack(side="left", padx=(12, 4), pady=12)
tk.Entry(raiz, validate="key", validatecommand=vcmd, width=6).pack(side="left", padx=(0, 12))
raiz.mainloop()
```

Otros códigos de sustitución útiles: `%S` (el texto insertado o borrado), `%d` (1 inserción, 0 borrado) y `%V` (qué disparó la validación).

### Tema 1.4 · Validación en tiempo real y entregable (Sesión 4)

La sesión es casi toda taller. El instructor presenta un patrón y el grupo lo aplica al entregable.

| Minutos | Actividad |
| --- | --- |
| 0–20 | Repaso: `validatecommand` frente a `trace_add()` |
| 20–40 | Demostración del patrón "cada validador devuelve un mensaje" |
| 40–105 | Desarrollo del entregable, con el instructor recorriendo los equipos |
| 105–120 | Dos o tres participantes muestran su formulario; retroalimentación grupal |

El patrón: cada campo tiene una función que recibe el texto y devuelve una cadena vacía si es válido o el mensaje de error si no lo es. Una sola función `actualizar()` recorre todos los campos, pinta los mensajes y decide si el botón se habilita. Así la lógica de validación queda separada de los widgets, una idea que vuelve con fuerza en el Módulo 4.

### Entregable del Módulo 1 · Formulario de registro con validación en tiempo real

Enunciado para el participante: construir un formulario de registro de usuario con Tkinter estándar que cumpla lo siguiente.

- Campos: nombre completo, correo, edad, contraseña, confirmación de contraseña y una casilla de aceptación.
- Reglas: nombre de al menos 3 caracteres; correo con formato válido; edad entre 18 y 99; contraseña de 8 o más caracteres con al menos un número y una mayúscula; confirmación igual a la contraseña.
- El campo de edad no debe aceptar teclas que no sean dígitos (`validatecommand`).
- Cada campo muestra su estado mientras se escribe: mensaje de error en rojo o marca de válido en verde. Un campo vacío no muestra error.
- El botón "Registrar" permanece deshabilitado hasta que todo sea válido y la casilla esté marcada.
- Layout con `.grid()`; la columna de los campos se estira al agrandar la ventana.
- Código con type hints, docstrings y nombres conforme a PEP 8.

Se entrega un archivo `.py` y dos capturas: el formulario con errores y el formulario completo.

#### Rúbrica

| Criterio | Peso | Excelente (100 %) | Suficiente (70 %) | Insuficiente (30 %) |
| --- | --- | --- | --- | --- |
| Reglas de validación | 30 % | Las cinco reglas funcionan, incluida la confirmación cuando cambia la contraseña | Falla una regla o un caso borde | Fallan dos o más reglas |
| Retroalimentación visual | 20 % | Mensaje por campo mientras se escribe; botón se habilita y deshabilita correctamente | Mensajes sólo al pulsar el botón | Sin mensajes por campo |
| Layout y redimensionado | 20 % | `grid` alineado, columna con peso, sin saltos al aparecer mensajes | Alineado pero no se adapta al tamaño | Widgets desalineados o encimados |
| Calidad del código | 20 % | PEP 8, type hints, docstrings, validadores separados de los widgets | Cumple dos de los cuatro aspectos | Cumple uno o ninguno |
| Criterio de UI/UX | 10 % | Textos claros, orden de tabulación lógico, foco inicial en el primer campo | Un problema menor de consistencia | Varios problemas de uso |

#### Solución de referencia: `m1_entregable.py`

Usa `messagebox`, que se estudia en el Módulo 3; si el grupo aún no lo conoce, basta con cambiar el texto de una etiqueta.

```python
"""Solución de referencia del entregable del Módulo 1.

Formulario de registro con validación en tiempo real:
- validatecommand restringe la edad a dígitos;
- trace_add revalida todos los campos en cada cambio;
- el botón Registrar sólo se habilita cuando todo es válido.
"""
import re
import tkinter as tk
from tkinter import messagebox
from typing import Any, Callable

PATRON_CORREO = re.compile(r"^[\w.+-]+@[\w-]+(\.[\w-]+)+$")
COLOR_ERROR = "#c0392b"
COLOR_OK = "#1e8449"

Validador = Callable[[str], str]


def validar_nombre(texto: str) -> str:
    """Devuelve el mensaje de error, o cadena vacía si el valor es válido."""
    return "" if len(texto.strip()) >= 3 else "Mínimo 3 caracteres"


def validar_correo(texto: str) -> str:
    """Comprueba un formato básico usuario@dominio.ext."""
    return "" if PATRON_CORREO.match(texto.strip()) else "Correo no válido"


def validar_edad(texto: str) -> str:
    """validatecommand ya garantiza que el texto sólo contiene dígitos."""
    if not texto:
        return "Campo obligatorio"
    return "" if 18 <= int(texto) <= 99 else "Entre 18 y 99 años"


def validar_password(texto: str) -> str:
    """Política mínima: 8 caracteres, un número y una mayúscula."""
    if len(texto) < 8:
        return "Mínimo 8 caracteres"
    if not any(c.isdigit() for c in texto):
        return "Incluye al menos un número"
    if not any(c.isupper() for c in texto):
        return "Incluye una mayúscula"
    return ""


def solo_digitos(propuesto: str) -> bool:
    """validatecommand: acepta la tecla si el resultado son 0 a 2 dígitos."""
    return propuesto == "" or (propuesto.isdigit() and len(propuesto) <= 2)


def main() -> None:
    """Construye el formulario y arranca el bucle principal."""
    raiz = tk.Tk()
    raiz.title("Registro de usuario")
    raiz.minsize(560, 0)
    raiz.columnconfigure(1, weight=1)

    variables: dict[str, tk.StringVar] = {}
    mensajes: dict[str, tk.Label] = {}
    entradas: list[tk.Entry] = []
    acepta = tk.BooleanVar(value=False)

    def validar_confirmacion(texto: str) -> str:
        """Depende de otro campo, por eso se define dentro de main()."""
        return "" if texto == variables["password"].get() else "Las contraseñas no coinciden"

    vcmd_edad = (raiz.register(solo_digitos), "%P")
    campos: list[tuple[str, str, Validador, dict[str, Any]]] = [
        ("nombre", "Nombre completo", validar_nombre, {}),
        ("correo", "Correo", validar_correo, {}),
        ("edad", "Edad", validar_edad, {"validate": "key", "validatecommand": vcmd_edad}),
        ("password", "Contraseña", validar_password, {"show": "•"}),
        ("confirmacion", "Confirmar contraseña", validar_confirmacion, {"show": "•"}),
    ]

    def actualizar(*_args: object) -> None:
        """Revalida todos los campos y habilita el botón sólo si no hay errores."""
        todo_valido = True
        for clave, _etiqueta, validador, _opciones in campos:
            valor = variables[clave].get()
            error = validador(valor)
            if not valor:
                mensajes[clave].config(text="")  # campo sin tocar: sin mensaje
            elif error:
                mensajes[clave].config(text=error, fg=COLOR_ERROR)
            else:
                mensajes[clave].config(text="✓", fg=COLOR_OK)
            todo_valido = todo_valido and not error
        boton.config(state="normal" if todo_valido and acepta.get() else "disabled")

    for fila, (clave, etiqueta, _validador, opciones) in enumerate(campos):
        variables[clave] = tk.StringVar()
        tk.Label(raiz, text=f"{etiqueta}:").grid(
            row=fila, column=0, sticky="e", padx=(16, 6), pady=4
        )
        entrada = tk.Entry(raiz, textvariable=variables[clave], **opciones)
        entrada.grid(row=fila, column=1, sticky="ew", pady=4)
        entradas.append(entrada)
        # Ancho fijo: la columna no "salta" cuando aparece o cambia un mensaje.
        mensajes[clave] = tk.Label(raiz, text="", width=28, anchor="w")
        mensajes[clave].grid(row=fila, column=2, sticky="w", padx=(6, 16))

    # Las trazas se enlazan cuando ya existen todas las variables y etiquetas.
    for variable in variables.values():
        variable.trace_add("write", actualizar)

    fila_final = len(campos)
    tk.Checkbutton(
        raiz, text="Acepto el aviso de privacidad", variable=acepta, command=actualizar
    ).grid(row=fila_final, column=1, sticky="w", pady=(8, 4))

    def registrar() -> None:
        """Acción final; en módulos posteriores se delega a la capa de lógica."""
        nombre = variables["nombre"].get().strip()
        messagebox.showinfo("Registro", f"Usuario {nombre} registrado.")

    # El botón se crea al final para que sea el último en el orden de tabulación.
    boton = tk.Button(raiz, text="Registrar", state="disabled", width=14, command=registrar)
    boton.grid(row=fila_final + 1, column=1, sticky="w", pady=(4, 16))

    entradas[0].focus_set()
    raiz.mainloop()


if __name__ == "__main__":
    main()
```

Casos que conviene probar al revisar entregables: escribir la confirmación primero y luego la contraseña (la confirmación debe pasar a válida); pegar texto con letras en el campo de edad (debe rechazarse completo); desmarcar la casilla con todo válido (el botón debe desactivarse).

### Ejercicios de clase con solución

Son independientes entre sí y del entregable. El tiempo indicado es para un participante con el tema recién visto.

#### Ejercicio 1 · Conversor de temperatura con `.pack()` (Sesión 2, 20 min)

Enunciado: una ventana con un campo para grados Celsius, un botón "Convertir" y una etiqueta con el resultado en Fahrenheit. Si el texto no es un número, la etiqueta muestra el error en rojo. Aceptar coma o punto decimal.

```python
"""Solución E1: conversor de temperatura con pack."""
import tkinter as tk


def celsius_a_fahrenheit(celsius: float) -> float:
    """Convierte grados Celsius a Fahrenheit."""
    return celsius * 9 / 5 + 32


raiz = tk.Tk()
raiz.title("Conversor °C a °F")
entrada = tk.StringVar()
resultado = tk.Label(raiz, text="", font=("Arial", 14))


def convertir() -> None:
    """Lee el campo, convierte y muestra el resultado o el error."""
    try:
        grados = float(entrada.get().replace(",", "."))
    except ValueError:
        resultado.config(text="Escribe un número", fg="#c0392b")
        return
    resultado.config(text=f"{celsius_a_fahrenheit(grados):.1f} °F", fg="black")


tk.Label(raiz, text="Grados Celsius:").pack(padx=16, pady=(16, 4))
campo = tk.Entry(raiz, textvariable=entrada, justify="center")
campo.pack(padx=16, fill="x")
tk.Button(raiz, text="Convertir", command=convertir).pack(pady=8)
resultado.pack(pady=(0, 16))
campo.focus_set()

raiz.mainloop()
```

Variante para quien termina antes: convertir también al pulsar Enter con `campo.bind("<Return>", lambda _evento: convertir())`, adelanto del Módulo 3.

#### Ejercicio 2 · Teclado numérico con `.grid()` (Sesión 2, 30 min)

Enunciado: una pantalla de sólo lectura y un teclado de 4 × 4 cuyos botones crecen con la ventana. "C" limpia la pantalla y "⌫" borra el último carácter. El ejercicio evalúa el layout, no el cálculo.

```python
"""Solución E2: teclado numérico con grid."""
import tkinter as tk

TECLAS = (
    ("7", "8", "9", "/"),
    ("4", "5", "6", "*"),
    ("1", "2", "3", "-"),
    ("C", "0", "⌫", "+"),
)

raiz = tk.Tk()
raiz.title("Teclado")
pantalla = tk.StringVar()
tk.Entry(
    raiz, textvariable=pantalla, justify="right", font=("Arial", 18), state="readonly"
).grid(row=0, column=0, columnspan=4, sticky="ew", padx=6, pady=6)


def pulsar(tecla: str) -> None:
    """Actualiza la pantalla según la tecla pulsada."""
    if tecla == "C":
        pantalla.set("")
    elif tecla == "⌫":
        pantalla.set(pantalla.get()[:-1])
    else:
        pantalla.set(pantalla.get() + tecla)


for fila, teclas in enumerate(TECLAS, start=1):
    raiz.rowconfigure(fila, weight=1)
    for columna, tecla in enumerate(teclas):
        # t=tecla congela el valor actual; sin él todos los botones usarían "+".
        tk.Button(
            raiz, text=tecla, font=("Arial", 14), command=lambda t=tecla: pulsar(t)
        ).grid(row=fila, column=columna, sticky="nsew", padx=2, pady=2)

for columna in range(4):
    raiz.columnconfigure(columna, weight=1)

raiz.mainloop()
```

Error que casi todos cometen: escribir `lambda: pulsar(tecla)`. Todos los botones escriben la última tecla del ciclo. Conviene dejar que ocurra y explicarlo; el Módulo 3 retoma el tema con `functools.partial`.

#### Ejercicio 3 · Contador de caracteres (Sesión 3, 25 min)

Enunciado: un campo para un mensaje de hasta 140 caracteres. Una etiqueta muestra "n/140" en negro, en naranja desde el carácter 121 y en rojo al pasarse. El botón "Publicar" sólo se habilita entre 1 y 140 caracteres.

```python
"""Solución E3: contador de caracteres con trace_add."""
import tkinter as tk

LIMITE = 140
AVISO = LIMITE - 20

raiz = tk.Tk()
raiz.title("Mensaje corto")
raiz.columnconfigure(0, weight=1)

mensaje = tk.StringVar()
tk.Entry(raiz, textvariable=mensaje, width=50).grid(
    row=0, column=0, sticky="ew", padx=12, pady=(12, 4)
)
contador = tk.Label(raiz, text=f"0/{LIMITE}")
contador.grid(row=1, column=0, sticky="e", padx=12)
publicar = tk.Button(raiz, text="Publicar", state="disabled")
publicar.grid(row=2, column=0, pady=12)


def actualizar_contador(*_args: object) -> None:
    """Actualiza texto y color del contador y el estado del botón."""
    longitud = len(mensaje.get())
    if longitud > LIMITE:
        color = "#c0392b"
    elif longitud > AVISO:
        color = "#d68910"
    else:
        color = "black"
    contador.config(text=f"{longitud}/{LIMITE}", fg=color)
    publicar.config(state="normal" if 0 < longitud <= LIMITE else "disabled")


mensaje.trace_add("write", actualizar_contador)
raiz.mainloop()
```

### Errores frecuentes del Módulo 1

| Síntoma | Causa | Solución |
| --- | --- | --- |
| La ventana aparece y se cierra al instante | Falta `mainloop()` | Llamar `raiz.mainloop()` al final |
| `AttributeError: 'NoneType' object has no attribute ...` | `boton = tk.Button(...).grid(...)` guarda el resultado de `grid()`, que es `None` | Crear el widget en una línea y ubicarlo en otra |
| La función del botón se ejecuta al abrir y luego no responde | `command=funcion()` en lugar de `command=funcion` | Pasar la función sin paréntesis, o usar `lambda` si lleva argumentos |
| `TclError: cannot use geometry manager grid inside . which already has slaves managed by pack` | `pack` y `grid` en el mismo contenedor | Un gestor por contenedor; agrupar con `Frame` |
| Los campos no crecen al agrandar la ventana | Falta `columnconfigure(..., weight=1)` o `sticky="ew"` | Dar peso a la columna y estirar el widget |
| `RuntimeError: Too early to create variable` | `StringVar()` creada antes de `tk.Tk()` | Crear primero la ventana principal |
| `TclError: expected floating-point number` al leer una variable | `DoubleVar.get()` o `IntVar.get()` con el campo vacío o con texto | Usar `StringVar` y convertir dentro de `try` |
| El `validatecommand` deja de validar sin avisar | La función lanzó una excepción o no devolvió `bool` | Devolver siempre `True` o `False`; probar con `print` dentro de la función |
| `ModuleNotFoundError: No module named 'tkinter'` en Linux | Python instalado sin Tk | `sudo apt install python3-tk` |

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

## Módulo 3 · Programación orientada a eventos y manejo de diálogos

Hasta ahora todo pasaba por `command`, que sólo responde al clic principal de un botón. Este módulo abre el resto de las interacciones: teclas, doble clic, clic derecho, ruedas de ratón, diálogos del sistema y tablas con miles de filas.

### Objetivos de aprendizaje

Al cerrar el módulo, el participante:

- Enlaza eventos de ratón y teclado con `.bind()` y lee la información del objeto `event`.
- Pasa argumentos a un callback con `lambda` y con `functools.partial`, y explica el error de enlace tardío en ciclos.
- Usa diálogos de mensaje, de confirmación, de archivos, de color y de fecha, y maneja la cancelación del usuario.
- Construye tablas con `ttk.Treeview`: columnas, inserción, actualización, ordenamiento por encabezado y selección.
- Ajusta el estilo de `ttk.Treeview` para que no desentone con el modo oscuro de CustomTkinter.

### Distribución por sesiones

| Sesión | Duración | Contenido | Actividad práctica |
| --- | --- | --- | --- |
| 9 | 2 h | Tema 3.1: eventos con `.bind()`. Tema 3.2: callbacks con argumentos | Ejercicio 7 (atajos de teclado y menú contextual) |
| 10 | 2 h | Tema 3.3: diálogos | Ejercicio 8 (bloc de notas con abrir y guardar) |
| 11 | 2 h | Tema 3.4: `ttk.Treeview` | Ejercicio 9 (tabla de calificaciones ordenable) |
| 12 | 2 h | Integración: explorador de archivos | Desarrollo y revisión del entregable |

### Preparación del instructor

- Instalar en cada entorno los dos paquetes externos del módulo: `pip install CTkMessagebox tkcalendar`.
- Preparar una carpeta `muestras/` con unos 30 archivos de tipos variados (`.txt`, `.csv`, `.py`, imágenes, un PDF) y dos subcarpetas. Se usa en la demostración de `Treeview` y en el entregable.
- Incluir en `muestras/` un archivo de texto con acentos guardado en UTF-8 y otro guardado en Windows-1252 (ANSI), para provocar el error de codificación en clase.

### Tema 3.1 · Eventos de ratón y teclado (Sesión 9)

Una aplicación de escritorio no ejecuta un programa de arriba abajo: espera a que ocurra algo y reacciona. `mainloop()`, visto en el Módulo 1, es ese ciclo de espera. `.bind()` le dice qué función llamar cuando ocurre un evento concreto sobre un widget concreto.

| Minutos | Actividad |
| --- | --- |
| 0–20 | El modelo orientado a eventos: cola de eventos, despacho y callbacks |
| 20–55 | `.bind()`, secuencias y objeto `event`: demostración de la pizarra |
| 55–75 | Tema 3.2: callbacks con argumentos |
| 75–120 | Ejercicio 7 |

#### Sintaxis

```python
widget.bind("<Secuencia>", callback)   # callback(event) recibe SIEMPRE un argumento
```

La secuencia sigue el patrón `<Modificador-Tipo-Detalle>`. Las más usadas:

| Secuencia | Se dispara cuando | Atributos útiles de `event` |
| --- | --- | --- |
| `<Button-1>` | Clic izquierdo | `x`, `y` (relativas al widget), `x_root`, `y_root` (pantalla) |
| `<Button-3>` | Clic derecho en Windows y Linux (en macOS suele ser `<Button-2>`) | `x_root`, `y_root` para ubicar un menú contextual |
| `<Double-Button-1>` | Doble clic | `widget` |
| `<B1-Motion>` | Arrastre con el botón izquierdo presionado | `x`, `y` |
| `<Motion>` | El puntero se mueve sobre el widget | `x`, `y` |
| `<Enter>` / `<Leave>` | El puntero entra o sale del widget | `widget` |
| `<MouseWheel>` | Rueda del ratón en Windows y macOS (en Linux: `<Button-4>` y `<Button-5>`) | `delta` |
| `<Return>`, `<Escape>`, `<Tab>` | Teclas especiales | `keysym` |
| `<KeyRelease>` | Se suelta cualquier tecla | `keysym`, `char` |
| `<Control-s>` | Ctrl + S (en macOS, `<Command-s>`) | — |
| `<FocusIn>` / `<FocusOut>` | El widget gana o pierde el foco | `widget` |
| `<Configure>` | El widget cambia de tamaño o posición | `width`, `height` |

Puntos que hay que dejar claros:

- `command` llama a la función sin argumentos; `.bind()` siempre pasa el objeto `event`. Una misma función usada en ambos lugares se declara con un parámetro opcional: `def guardar(_event: tk.Event | None = None) -> None`.
- `<Control-s>` y `<Control-S>` son distintos: el segundo requiere Mayúsculas o Bloq Mayús. Para un atajo robusto se enlazan ambos.
- Un `bind` sobre la ventana principal recibe los eventos de todos sus widgets hijos, porque cada widget pasa el evento a su ventana después de procesarlo. Si el callback devuelve la cadena `"break"`, el evento no sigue propagándose.

#### Código de demostración: `m3_pizarra.py`

Una pizarra de dibujo que usa cinco eventos distintos. El widget `tk.Canvas` no tiene equivalente en CustomTkinter y se combina sin problema con una ventana `CTk`.

```python
"""Pizarra de dibujo: eventos de ratón y teclado con .bind()."""
import tkinter as tk

import customtkinter as ctk

COLORES = ["#1f2937", "#c0392b", "#1e8449", "#2b5797"]

ventana = ctk.CTk()
ventana.title("Pizarra")
ventana.geometry("640x480")

lienzo = tk.Canvas(ventana, bg="white", highlightthickness=0, cursor="crosshair")
lienzo.pack(fill="both", expand=True, padx=12, pady=(12, 0))
estado = ctk.CTkLabel(ventana, text="", anchor="w")
estado.pack(fill="x", padx=12, pady=6)

ultimo_punto: tuple[int, int] | None = None
indice_color = 0


def mostrar_estado(texto: str = "") -> None:
    """Barra de estado con el color activo y un texto adicional."""
    estado.configure(text=f"Color: {COLORES[indice_color]}   {texto}")


def iniciar_trazo(event: tk.Event) -> None:
    """<Button-1>: guarda el punto donde empieza el trazo."""
    global ultimo_punto
    ultimo_punto = (event.x, event.y)


def dibujar(event: tk.Event) -> None:
    """<B1-Motion>: une el punto anterior con el actual."""
    global ultimo_punto
    if ultimo_punto is not None:
        lienzo.create_line(*ultimo_punto, event.x, event.y, fill=COLORES[indice_color],
                           width=3, capstyle="round")
    ultimo_punto = (event.x, event.y)


def cambiar_color(_event: tk.Event) -> None:
    """<Button-3>: pasa al siguiente color."""
    global indice_color
    indice_color = (indice_color + 1) % len(COLORES)
    mostrar_estado()


def mostrar_posicion(event: tk.Event) -> None:
    """<Motion>: coordenadas del puntero en la barra de estado."""
    mostrar_estado(f"x={event.x}  y={event.y}")


def limpiar(_event: tk.Event) -> None:
    """<Escape>: borra todo el lienzo."""
    lienzo.delete("all")


lienzo.bind("<Button-1>", iniciar_trazo)
lienzo.bind("<B1-Motion>", dibujar)
lienzo.bind("<Button-3>", cambiar_color)
lienzo.bind("<Motion>", mostrar_posicion)
ventana.bind("<Escape>", limpiar)  # en la ventana: funciona sin importar el foco

mostrar_estado()
ventana.mainloop()
```

El uso de `global` es deliberado en esta demostración y se señala como deuda: en el Módulo 4 el mismo estado vivirá como atributos de una clase.

### Tema 3.2 · Callbacks con argumentos (Sesión 9)

`command` y `.bind()` reciben una función, no una llamada. Cuando la función necesita datos extra (qué botón se pulsó, qué fila se eligió), hay que envolverla. El grupo ya vio el problema en el Ejercicio 2; aquí se explica la causa.

#### El error de enlace tardío

```python
"""Tres botones que deberían imprimir 0, 1 y 2 (m3_enlace_tardio.py)."""
from functools import partial

import customtkinter as ctk


def elegir(numero: int) -> None:
    """Muestra qué botón se pulsó."""
    print(f"Botón {numero}")


ventana = ctk.CTk()

for i in range(3):
    # INCORRECTO: la lambda busca i cuando se ejecuta, y para entonces i vale 2.
    ctk.CTkButton(ventana, text=f"Mal {i}", command=lambda: elegir(i)).grid(row=0, column=i, padx=4, pady=4)

    # Correcto 1: el argumento por omisión congela el valor de i en cada vuelta.
    ctk.CTkButton(ventana, text=f"Bien {i}", command=lambda n=i: elegir(n)).grid(row=1, column=i, padx=4, pady=4)

    # Correcto 2: partial guarda los argumentos en el momento de crearlo.
    ctk.CTkButton(ventana, text=f"Partial {i}", command=partial(elegir, i)).grid(row=2, column=i, padx=4, pady=4)

ventana.mainloop()
```

Explicación para el grupo: una `lambda` no copia las variables que usa; guarda una referencia a ellas. Todas las lambdas del ciclo apuntan a la misma `i`, y cuando se hace clic el ciclo ya terminó.

#### `lambda` o `partial`

|  | `lambda n=i: elegir(n)` | `partial(elegir, i)` |
| --- | --- | --- |
| Legibilidad | Familiar para principiantes | Más explícita en código profesional |
| Con `.bind()` | Hay que aceptar el evento: `lambda event, n=i: elegir(n)` | `partial(abrir, ruta)` llama a `abrir(ruta, event)`: el evento llega al final |
| Cuándo preferirla | Callbacks de una línea que transforman argumentos | Pasar datos fijos a una función ya definida |

Con `.bind()` la firma importa: `partial` antepone sus argumentos, así que la función debe declararse como `def abrir(ruta: Path, event: tk.Event) -> None`. Es un error frecuente escribirla al revés.

### Tema 3.3 · Diálogos (Sesión 10)

Los diálogos son la forma más directa de retroalimentación y de pedir una decisión. Todos son modales: detienen la ejecución del callback hasta que el usuario responde, sin congelar la ventana. El punto que más se olvida es la cancelación: cada diálogo tiene un valor de retorno para "el usuario cerró sin elegir", y el código debe manejarlo.

| Minutos | Actividad |
| --- | --- |
| 0–30 | Mensajes y confirmaciones: `messagebox` y `CTkMessagebox` |
| 30–60 | Archivos, color y fecha |
| 60–70 | Confirmar antes de cerrar la ventana |
| 70–120 | Ejercicio 8 |

#### Referencia rápida

| Función | Módulo | Devuelve | Si el usuario cancela |
| --- | --- | --- | --- |
| `showinfo`, `showwarning`, `showerror` | `tkinter.messagebox` | `"ok"` | — |
| `askyesno`, `askokcancel` | `tkinter.messagebox` | `True` / `False` | `False` |
| `askyesnocancel` | `tkinter.messagebox` | `True` / `False` | `None` |
| `CTkMessagebox(...).get()` | `CTkMessagebox` (externo) | Texto del botón pulsado | `None` |
| `askopenfilename` | `tkinter.filedialog` | Ruta como `str` | Cadena vacía |
| `asksaveasfilename` | `tkinter.filedialog` | Ruta como `str` | Cadena vacía |
| `askdirectory` | `tkinter.filedialog` | Ruta de carpeta como `str` | Cadena vacía |
| `askcolor` | `tkinter.colorchooser` | `((r, g, b), "#rrggbb")` | `(None, None)` |
| `DateEntry(...).get_date()` | `tkcalendar` (externo) | `datetime.date` | — |

Según el sistema operativo, los diálogos de archivo pueden devolver una tupla vacía en lugar de cadena vacía al cancelar. Comprobar con `if not ruta:` cubre ambos casos.

`messagebox` usa el aspecto nativo del sistema; `CTkMessagebox` sigue el modo claro u oscuro de CustomTkinter. Para el curso se recomienda `messagebox` en los ejercicios y `CTkMessagebox` en el proyecto final. `CTkMessagebox` y `tkcalendar` son paquetes de terceros con mantenimiento propio; conviene revisar su página en PyPI antes de cada edición del curso.

#### Código de demostración: `m3_dialogos.py`

```python
"""Catálogo de diálogos con manejo de cancelación."""
from pathlib import Path
from tkinter import colorchooser, filedialog, messagebox

import customtkinter as ctk
from CTkMessagebox import CTkMessagebox
from tkcalendar import DateEntry

ventana = ctk.CTk()
ventana.title("Diálogos")
ventana.geometry("420x460")

resultado = ctk.CTkLabel(ventana, text="Elige un diálogo", wraplength=380)


def mostrar(texto: str) -> None:
    """Escribe el resultado del diálogo en la etiqueta inferior."""
    resultado.configure(text=texto)


def abrir_archivo() -> None:
    """askopenfilename con filtros por tipo."""
    ruta = filedialog.askopenfilename(
        parent=ventana,
        title="Abrir archivo",
        filetypes=[("Texto", "*.txt"), ("CSV", "*.csv"), ("Todos", "*.*")],
    )
    if not ruta:
        mostrar("Apertura cancelada")
        return
    mostrar(f"Elegiste {Path(ruta).name}")


def guardar_como() -> None:
    """defaultextension agrega .txt si el usuario no escribe extensión."""
    ruta = filedialog.asksaveasfilename(
        parent=ventana, defaultextension=".txt", initialfile="notas.txt",
        filetypes=[("Texto", "*.txt")],
    )
    mostrar(f"Se guardaría en {ruta}" if ruta else "Guardado cancelado")


def elegir_color() -> None:
    """askcolor devuelve (None, None) si se cancela."""
    _rgb, hexadecimal = colorchooser.askcolor(parent=ventana, title="Color de acento")
    if hexadecimal is None:
        mostrar("Color sin cambios")
        return
    boton_color.configure(fg_color=hexadecimal)
    mostrar(f"Color elegido: {hexadecimal}")


def confirmar_nativo() -> None:
    """askyesnocancel distingue tres respuestas."""
    respuesta = messagebox.askyesnocancel("Cambios", "¿Guardar antes de continuar?", parent=ventana)
    textos = {True: "Guardar", False: "No guardar", None: "Cancelado"}
    mostrar(f"messagebox: {textos[respuesta]}")


def confirmar_ctk() -> None:
    """CTkMessagebox devuelve el texto del botón pulsado."""
    dialogo = CTkMessagebox(
        title="Eliminar", message="¿Eliminar el registro seleccionado?",
        icon="warning", option_1="Cancelar", option_2="Eliminar",
    )
    mostrar(f"CTkMessagebox: {dialogo.get()}")


for texto, accion in [
    ("Abrir archivo", abrir_archivo),
    ("Guardar como", guardar_como),
    ("Confirmar (messagebox)", confirmar_nativo),
    ("Confirmar (CTkMessagebox)", confirmar_ctk),
]:
    ctk.CTkButton(ventana, text=texto, width=240, command=accion).pack(pady=6)

boton_color = ctk.CTkButton(ventana, text="Elegir color", width=240, command=elegir_color)
boton_color.pack(pady=6)

ctk.CTkLabel(ventana, text="Fecha de entrega").pack(pady=(12, 0))
fecha = DateEntry(ventana, date_pattern="dd/mm/yyyy")
fecha.pack(pady=4)
fecha.bind("<<DateEntrySelected>>", lambda _e: mostrar(f"Fecha: {fecha.get_date():%d/%m/%Y}"))

resultado.pack(pady=16)


def al_cerrar() -> None:
    """protocol WM_DELETE_WINDOW intercepta el botón de cerrar de la ventana."""
    if messagebox.askokcancel("Salir", "¿Cerrar la aplicación?", parent=ventana):
        ventana.destroy()


ventana.protocol("WM_DELETE_WINDOW", al_cerrar)
ventana.mainloop()
```

Se señalan tres detalles: `parent=ventana` mantiene el diálogo encima de la ventana correcta; `<<DateEntrySelected>>` es un evento virtual (doble corchete) que genera el propio widget; y `protocol("WM_DELETE_WINDOW", ...)` es la única forma de intervenir cuando el usuario cierra con la X.

### Tema 3.4 · Tablas con `ttk.Treeview` (Sesión 11)

CustomTkinter no incluye un widget de tabla. `ttk.Treeview` cubre ese hueco: maneja miles de filas sin problema porque sólo dibuja las visibles, a diferencia de `CTkScrollableFrame`. Su costo es que pertenece a `ttk`, no a CustomTkinter, y hay que darle estilo a mano para que no rompa el modo oscuro.

| Minutos | Actividad |
| --- | --- |
| 0–30 | Anatomía: columnas, encabezados, filas, `iid` y barra de desplazamiento |
| 30–55 | Operaciones: insertar, actualizar, eliminar, seleccionar |
| 55–75 | Ordenar por columna y estilo claro/oscuro |
| 75–120 | Ejercicio 9 |

#### Operaciones

| Tarea | Código |
| --- | --- |
| Crear tabla sin columna de árbol | `ttk.Treeview(padre, columns=("sku", "nombre"), show="headings", selectmode="browse")` |
| Encabezado y ancho | `tabla.heading("sku", text="SKU")` y `tabla.column("sku", width=90, anchor="center", stretch=False)` |
| Insertar | `tabla.insert("", "end", iid="7", values=("PROD-007", "Mouse"), tags=("par",))` |
| Actualizar | `tabla.item("7", values=(...))` |
| Leer una celda | `tabla.set("7", "nombre")` |
| Eliminar todo | `tabla.delete(*tabla.get_children())` |
| Fila seleccionada | `seleccion = tabla.selection()` (tupla de `iid`; vacía si no hay) |
| Seleccionar desde código | `tabla.selection_set("7")` y `tabla.see("7")` |
| Reordenar | `tabla.move(iid, "", nueva_posicion)` |
| Colores por fila | `tabla.tag_configure("par", background="#f3f5f8")` |
| Eventos | `<<TreeviewSelect>>` (cambió la selección), `<Double-1>` (doble clic) |

El `iid` es el identificador de la fila dentro de la tabla. Si no se indica, Tkinter genera uno (`I001`, `I002`...). Conviene usar la clave del dato (un id, una ruta de archivo) para encontrar la fila sin recorrer la tabla.

#### Código de demostración: `m3_tabla.py`

Tabla de productos en memoria con ordenamiento por encabezado, filas alternas, detalle de la selección y estilo que sigue al modo de apariencia.

```python
"""ttk.Treeview integrado en una ventana CustomTkinter."""
import tkinter as tk
from tkinter import ttk

import customtkinter as ctk

COLUMNAS = {  # clave: (título, ancho, alineación)
    "sku": ("SKU", 100, "center"),
    "nombre": ("Nombre", 240, "w"),
    "precio": ("Precio", 100, "e"),
    "stock": ("Stock", 80, "e"),
}
PRODUCTOS = [
    ("PROD-001", "Teclado mecánico", 85.50, 15),
    ("PROD-002", "Monitor 24 pulgadas", 190.00, 8),
    ("PROD-003", "Silla ergonómica", 220.00, 5),
    ("PROD-004", "Mouse inalámbrico", 24.90, 40),
    ("PROD-005", "Base para laptop", 32.00, 12),
    ("PROD-006", "Audífonos con micrófono", 45.00, 3),
    ("PROD-007", "Cámara web HD", 58.75, 9),
    ("PROD-008", "Hub USB-C", 39.99, 21),
]
ESTILOS = {  # modo: (fondo, texto, fondo alterno, selección, encabezado)
    "Light": ("#ffffff", "#1f2937", "#f3f5f8", "#3a7ebf", "#e3e7ec"),
    "Dark": ("#2a2d33", "#e5e7eb", "#31353c", "#1f538d", "#1d2025"),
}

ventana = ctk.CTk()
ventana.title("Productos")
ventana.geometry("620x400")
ventana.columnconfigure(0, weight=1)
ventana.rowconfigure(0, weight=1)

estilo = ttk.Style()
estilo.theme_use("clam")  # el tema "clam" sí respeta colores personalizados

tabla = ttk.Treeview(ventana, columns=tuple(COLUMNAS), show="headings", selectmode="browse")
tabla.grid(row=0, column=0, sticky="nsew", padx=(16, 0), pady=16)
barra = ctk.CTkScrollbar(ventana, command=tabla.yview)
barra.grid(row=0, column=1, sticky="ns", padx=(0, 16), pady=16)
tabla.configure(yscrollcommand=barra.set)

detalle = ctk.CTkLabel(ventana, text="Selecciona un producto", anchor="w")
detalle.grid(row=1, column=0, columnspan=2, sticky="ew", padx=16, pady=(0, 12))

orden_descendente: dict[str, bool] = {}


def aplicar_estilo(modo: str) -> None:
    """Traduce el modo de CustomTkinter a colores de ttk.Style."""
    fondo, texto, alterno, seleccion, encabezado = ESTILOS[modo]
    estilo.configure("Treeview", background=fondo, fieldbackground=fondo, foreground=texto,
                     rowheight=28, borderwidth=0)
    estilo.map("Treeview", background=[("selected", seleccion)], foreground=[("selected", "white")])
    estilo.configure("Treeview.Heading", background=encabezado, foreground=texto, relief="flat")
    tabla.tag_configure("alterna", background=alterno)


def clave_orden(valor: str) -> tuple[int, float | str]:
    """Ordena números como números y texto sin distinguir mayúsculas."""
    try:
        return (0, float(valor.replace("$", "").replace(",", "")))
    except ValueError:
        return (1, valor.lower())


def ordenar(columna: str) -> None:
    """Clic en el encabezado: ordena y alterna ascendente/descendente."""
    descendente = orden_descendente.get(columna, False)
    filas = [(tabla.set(iid, columna), iid) for iid in tabla.get_children()]
    filas.sort(key=lambda par: clave_orden(par[0]), reverse=descendente)
    for posicion, (_valor, iid) in enumerate(filas):
        tabla.move(iid, "", posicion)
        tabla.item(iid, tags=("alterna",) if posicion % 2 else ())
    for clave, (titulo, _ancho, _alineacion) in COLUMNAS.items():
        flecha = (" ▼" if descendente else " ▲") if clave == columna else ""
        tabla.heading(clave, text=titulo + flecha)
    orden_descendente[columna] = not descendente


def al_seleccionar(_event: tk.Event) -> None:
    """<<TreeviewSelect>>: muestra el detalle de la fila elegida."""
    seleccion = tabla.selection()
    if not seleccion:
        return
    sku, nombre, precio, stock = tabla.item(seleccion[0], "values")
    detalle.configure(text=f"{sku} · {nombre} · {precio} · {stock} piezas en inventario")


for clave, (titulo, ancho, alineacion) in COLUMNAS.items():
    tabla.heading(clave, text=titulo, command=lambda c=clave: ordenar(c))
    tabla.column(clave, width=ancho, anchor=alineacion, stretch=(clave == "nombre"))

for posicion, (sku, nombre, precio, stock) in enumerate(PRODUCTOS):
    tabla.insert("", "end", iid=sku, values=(sku, nombre, f"${precio:,.2f}", stock),
                 tags=("alterna",) if posicion % 2 else ())

tabla.bind("<<TreeviewSelect>>", al_seleccionar)
aplicar_estilo(ctk.get_appearance_mode())
ventana.mainloop()
```

Para conectar el estilo con un interruptor de modo, basta con llamar `aplicar_estilo()` justo después de `ctk.set_appearance_mode()`. El entregable lo exige.

Sobre `rowheight=28`: `ttk` no conoce la escala de CustomTkinter. Si la aplicación cambia la escala con `set_widget_scaling()`, hay que multiplicar este valor por el mismo factor.

### Entregable del Módulo 3 · Explorador de archivos con vista tabular y visor

El entregable reúne los cuatro temas: eventos, callbacks con argumentos, diálogos y `Treeview`. También obliga a manejar errores reales del sistema de archivos (carpetas sin permiso, codificaciones distintas), que es donde la retroalimentación al usuario deja de ser teórica.

Enunciado para el participante: construir con CustomTkinter un explorador de archivos local con estas funciones.

- Barra superior con "Abrir carpeta" (`askdirectory`), "Subir" (carpeta padre), la ruta actual y un campo que filtra por nombre mientras se escribe.
- Tabla `ttk.Treeview` con columnas Nombre, Tipo, Tamaño y Modificado. Las carpetas van siempre antes que los archivos. Clic en un encabezado ordena por esa columna; un segundo clic invierte el orden. El tamaño se ordena por bytes, no por el texto mostrado.
- Doble clic en una carpeta la abre. Seleccionar un archivo de texto muestra su contenido en un visor de sólo lectura; para otros tipos, un mensaje de vista previa no disponible.
- Menú contextual (clic derecho) con la opción "Copiar ruta".
- Atajos: Ctrl+O abre carpeta, F5 recarga, Alt+↑ sube un nivel.
- Mensajes de error claros si una carpeta no tiene permiso de lectura o un archivo no se puede leer. Los archivos en UTF-8 y en Windows-1252 se ven con acentos correctos.
- Interruptor de modo oscuro que también cambia el estilo de la tabla.
- Barra de estado con el número de carpetas y archivos mostrados.

Se entregan el `.py` y un video corto (1 a 2 minutos) o una secuencia de capturas mostrando cada función.

#### Rúbrica

| Criterio | Peso | Excelente (100 %) | Suficiente (70 %) | Insuficiente (30 %) |
| --- | --- | --- | --- | --- |
| Navegación | 20 % | Abrir, subir, doble clic, filtro y atajos funcionan | Falta una forma de navegar o un atajo | Sólo abre una carpeta fija |
| Tabla | 25 % | Cuatro columnas, carpetas primero, orden por bytes y fechas reales, indicador de orden | Ordena, pero el tamaño se ordena como texto | Sin ordenamiento |
| Visor y errores | 25 % | Vista previa correcta en ambas codificaciones; permisos y cancelaciones manejados con mensajes claros | Un caso de error sin manejar | La aplicación se cierra con una excepción |
| Eventos y diálogos | 15 % | Menú contextual, `<<TreeviewSelect>>`, doble clic y diálogos con `parent` | Falta el menú contextual o un evento | Sólo usa `command` |
| Calidad del código | 15 % | PEP 8, type hints, docstrings; datos separados de su presentación | Correcto pero con lógica mezclada | Difícil de seguir |

#### Solución de referencia: `m3_entregable.py`

La decisión de diseño que conviene explicar: la tabla muestra textos ("1.2 MB", "27/09/2026 14:05"), pero el diccionario `datos` guarda los valores originales (bytes, marca de tiempo). Se ordena con los datos y se muestra la presentación. Es el primer contacto del grupo con la separación entre modelo y vista que formaliza el Módulo 4.

```python
"""Solución de referencia del entregable del Módulo 3: explorador de archivos."""
import tkinter as tk
from datetime import datetime
from pathlib import Path
from tkinter import filedialog, messagebox, ttk
from typing import Any

import customtkinter as ctk

EXTENSIONES_TEXTO = {".txt", ".md", ".csv", ".py", ".json", ".log", ".ini", ".sql", ".html", ".css"}
LIMITE_VISTA = 200_000  # bytes que se leen como máximo para la vista previa
COLUMNAS = {  # clave: (título, ancho, alineación)
    "nombre": ("Nombre", 280, "w"),
    "tipo": ("Tipo", 90, "w"),
    "tamano": ("Tamaño", 90, "e"),
    "modificado": ("Modificado", 140, "center"),
}
ESTILOS = {  # modo: (fondo, texto, fondo alterno, selección, encabezado)
    "Light": ("#ffffff", "#1f2937", "#f3f5f8", "#3a7ebf", "#e3e7ec"),
    "Dark": ("#2a2d33", "#e5e7eb", "#31353c", "#1f538d", "#1d2025"),
}


def formatear_tamano(num_bytes: int) -> str:
    """Convierte bytes a la unidad más legible."""
    tamano = float(num_bytes)
    for unidad in ("B", "KB", "MB", "GB"):
        if tamano < 1024:
            return f"{tamano:.0f} {unidad}" if unidad == "B" else f"{tamano:.1f} {unidad}"
        tamano /= 1024
    return f"{tamano:.1f} TB"


def leer_texto(ruta: Path) -> str:
    """Lee el inicio del archivo probando UTF-8 y después Windows-1252."""
    with ruta.open("rb") as archivo:
        contenido = archivo.read(LIMITE_VISTA)
    for codificacion in ("utf-8", "cp1252"):
        try:
            return contenido.decode(codificacion)
        except UnicodeDecodeError:
            continue
    return contenido.decode("utf-8", errors="replace")


def main() -> None:
    """Construye el explorador y arranca el bucle principal."""
    ctk.set_appearance_mode("system")
    ventana = ctk.CTk()
    ventana.title("Explorador de archivos")
    ventana.geometry("1040x620")
    ventana.minsize(780, 440)
    ventana.columnconfigure(0, weight=3)
    ventana.columnconfigure(1, weight=2)
    ventana.rowconfigure(1, weight=1)

    datos: dict[str, dict[str, Any]] = {}  # iid (ruta) -> valores originales
    estado: dict[str, Any] = {"carpeta": Path.home(), "orden": ("nombre", False)}

    # ---------- Barra superior ----------
    barra = ctk.CTkFrame(ventana, fg_color="transparent")
    barra.grid(row=0, column=0, columnspan=2, sticky="ew", padx=16, pady=(16, 8))
    barra.columnconfigure(2, weight=1)
    ruta_etiqueta = ctk.CTkLabel(barra, text="", anchor="w")
    ruta_etiqueta.grid(row=0, column=2, sticky="ew", padx=12)
    filtro = ctk.CTkEntry(barra, placeholder_text="Filtrar por nombre", width=200)
    filtro.grid(row=0, column=3, padx=(0, 12))
    modo_oscuro = ctk.BooleanVar(value=ctk.get_appearance_mode() == "Dark")

    # ---------- Tabla ----------
    marco_tabla = ctk.CTkFrame(ventana, fg_color="transparent")
    marco_tabla.grid(row=1, column=0, sticky="nsew", padx=(16, 8))
    marco_tabla.columnconfigure(0, weight=1)
    marco_tabla.rowconfigure(0, weight=1)
    estilo = ttk.Style()
    estilo.theme_use("clam")
    tabla = ttk.Treeview(marco_tabla, columns=tuple(COLUMNAS), show="headings", selectmode="browse")
    tabla.grid(row=0, column=0, sticky="nsew")
    desplazamiento = ctk.CTkScrollbar(marco_tabla, command=tabla.yview)
    desplazamiento.grid(row=0, column=1, sticky="ns")
    tabla.configure(yscrollcommand=desplazamiento.set)

    # ---------- Visor ----------
    visor = ctk.CTkTextbox(ventana, wrap="none", font=ctk.CTkFont(family="Consolas", size=13))
    visor.grid(row=1, column=1, sticky="nsew", padx=(8, 16))
    visor.configure(state="disabled")
    estado_etiqueta = ctk.CTkLabel(ventana, text="", anchor="w")
    estado_etiqueta.grid(row=2, column=0, columnspan=2, sticky="ew", padx=16, pady=8)

    def aplicar_estilo() -> None:
        """Sincroniza el estilo de ttk con el modo de CustomTkinter."""
        fondo, texto, alterno, seleccion, encabezado = ESTILOS[ctk.get_appearance_mode()]
        estilo.configure("Treeview", background=fondo, fieldbackground=fondo, foreground=texto,
                         rowheight=26, borderwidth=0)
        estilo.map("Treeview", background=[("selected", seleccion)], foreground=[("selected", "white")])
        estilo.configure("Treeview.Heading", background=encabezado, foreground=texto, relief="flat")
        tabla.tag_configure("alterna", background=alterno)

    def cambiar_modo() -> None:
        """Interruptor de modo: CustomTkinter y después ttk."""
        ctk.set_appearance_mode("dark" if modo_oscuro.get() else "light")
        aplicar_estilo()

    def escribir_visor(texto: str) -> None:
        """El visor es de sólo lectura: se habilita, se escribe y se vuelve a bloquear."""
        visor.configure(state="normal")
        visor.delete("1.0", "end")
        visor.insert("1.0", texto)
        visor.configure(state="disabled")

    def mostrar_tabla() -> None:
        """Ordena los datos originales y pinta su presentación en la tabla."""
        columna, descendente = estado["orden"]

        def valor(iid: str) -> Any:
            dato = datos[iid][columna]
            return dato.lower() if isinstance(dato, str) else dato

        carpetas = sorted((i for i in datos if datos[i]["es_carpeta"]), key=valor, reverse=descendente)
        archivos = sorted((i for i in datos if not datos[i]["es_carpeta"]), key=valor, reverse=descendente)

        tabla.delete(*tabla.get_children())
        for posicion, iid in enumerate(carpetas + archivos):
            dato = datos[iid]
            tabla.insert(
                "", "end", iid=iid,
                values=(
                    dato["nombre"],
                    dato["tipo"],
                    "" if dato["es_carpeta"] else formatear_tamano(dato["tamano"]),
                    datetime.fromtimestamp(dato["modificado"]).strftime("%d/%m/%Y %H:%M"),
                ),
                tags=("alterna",) if posicion % 2 else (),
            )
        for clave, (titulo, _ancho, _alineacion) in COLUMNAS.items():
            flecha = (" ▼" if descendente else " ▲") if clave == columna else ""
            tabla.heading(clave, text=titulo + flecha)
        estado_etiqueta.configure(text=f"{len(carpetas)} carpetas · {len(archivos)} archivos")

    def abrir_carpeta(carpeta: Path) -> None:
        """Lista la carpeta; sólo la vuelve actual si se pudo leer."""
        try:
            entradas = list(carpeta.iterdir())
        except PermissionError:
            messagebox.showerror("Sin permiso", f"No tienes permiso para abrir:\n{carpeta}", parent=ventana)
            return
        except OSError as error:
            messagebox.showerror("Error", str(error), parent=ventana)
            return

        texto_filtro = filtro.get().strip().lower()
        datos.clear()
        for entrada in entradas:
            if texto_filtro and texto_filtro not in entrada.name.lower():
                continue
            try:
                info = entrada.stat()
            except OSError:
                continue  # enlaces rotos o archivos que desaparecieron
            es_carpeta = entrada.is_dir()
            datos[str(entrada)] = {
                "ruta": entrada,
                "es_carpeta": es_carpeta,
                "nombre": entrada.name,
                "tipo": "Carpeta" if es_carpeta else (entrada.suffix.lower() or "Archivo"),
                "tamano": 0 if es_carpeta else info.st_size,
                "modificado": info.st_mtime,
            }
        estado["carpeta"] = carpeta
        ruta_etiqueta.configure(text=str(carpeta))
        escribir_visor("")
        mostrar_tabla()

    def recargar(_event: tk.Event | None = None) -> None:
        """F5 y filtro: vuelve a leer la carpeta actual."""
        abrir_carpeta(estado["carpeta"])

    def elegir_carpeta(_event: tk.Event | None = None) -> None:
        """Ctrl+O o botón: diálogo de selección de carpeta."""
        ruta = filedialog.askdirectory(parent=ventana, initialdir=str(estado["carpeta"]))
        if ruta:
            filtro.delete(0, "end")
            abrir_carpeta(Path(ruta))

    def subir(_event: tk.Event | None = None) -> None:
        """Alt+↑ o botón: abre la carpeta padre."""
        padre = estado["carpeta"].parent
        if padre != estado["carpeta"]:  # en la raíz, parent es la misma ruta
            filtro.delete(0, "end")
            abrir_carpeta(padre)

    def ordenar(columna: str) -> None:
        """Misma columna: invierte el orden. Otra columna: ascendente."""
        actual, descendente = estado["orden"]
        estado["orden"] = (columna, not descendente if columna == actual else False)
        mostrar_tabla()

    def al_seleccionar(_event: tk.Event) -> None:
        """<<TreeviewSelect>>: vista previa o descripción del elemento."""
        seleccion = tabla.selection()
        if not seleccion:
            return
        dato = datos[seleccion[0]]
        ruta: Path = dato["ruta"]
        if dato["es_carpeta"]:
            escribir_visor(f"Carpeta: {ruta.name}\n\nDoble clic para abrirla.")
            return
        if ruta.suffix.lower() not in EXTENSIONES_TEXTO:
            escribir_visor(f"{ruta.name}\n{formatear_tamano(dato['tamano'])}\n\n"
                           "Vista previa no disponible para este tipo de archivo.")
            return
        try:
            texto = leer_texto(ruta)
        except OSError as error:
            messagebox.showerror("No se pudo leer", str(error), parent=ventana)
            return
        if dato["tamano"] > LIMITE_VISTA:
            texto += f"\n\n[Vista previa limitada a {formatear_tamano(LIMITE_VISTA)}]"
        escribir_visor(texto)

    def al_doble_clic(event: tk.Event) -> None:
        """identify_row traduce la coordenada del clic a la fila."""
        iid = tabla.identify_row(event.y)
        if iid and datos[iid]["es_carpeta"]:
            filtro.delete(0, "end")
            abrir_carpeta(datos[iid]["ruta"])

    menu = tk.Menu(ventana, tearoff=0)

    def copiar_ruta() -> None:
        """Copia la ruta completa del elemento seleccionado."""
        seleccion = tabla.selection()
        if seleccion:
            ventana.clipboard_clear()
            ventana.clipboard_append(seleccion[0])
            estado_etiqueta.configure(text="Ruta copiada al portapapeles")

    def mostrar_menu(event: tk.Event) -> None:
        """Clic derecho: selecciona la fila bajo el puntero y abre el menú."""
        iid = tabla.identify_row(event.y)
        if not iid:
            return
        tabla.selection_set(iid)
        try:
            menu.tk_popup(event.x_root, event.y_root)
        finally:
            menu.grab_release()

    menu.add_command(label="Copiar ruta", command=copiar_ruta)

    # ---------- Controles y enlaces ----------
    ctk.CTkButton(barra, text="Abrir carpeta", width=120, command=elegir_carpeta).grid(row=0, column=0)
    ctk.CTkButton(barra, text="↑ Subir", width=80, command=subir).grid(row=0, column=1, padx=(8, 0))
    ctk.CTkSwitch(barra, text="Modo oscuro", variable=modo_oscuro, command=cambiar_modo).grid(row=0, column=4)

    for clave, (titulo, ancho, alineacion) in COLUMNAS.items():
        tabla.heading(clave, text=titulo, command=lambda c=clave: ordenar(c))
        tabla.column(clave, width=ancho, anchor=alineacion, stretch=(clave == "nombre"))

    tabla.bind("<<TreeviewSelect>>", al_seleccionar)
    tabla.bind("<Double-1>", al_doble_clic)
    tabla.bind("<Button-3>", mostrar_menu)  # en macOS agregar también <Button-2>
    filtro.bind("<KeyRelease>", recargar)
    ventana.bind("<Control-o>", elegir_carpeta)
    ventana.bind("<F5>", recargar)
    ventana.bind("<Alt-Up>", subir)

    aplicar_estilo()
    abrir_carpeta(estado["carpeta"])
    ventana.mainloop()


if __name__ == "__main__":
    main()
```

Casos de prueba para la revisión: abrir la carpeta del sistema operativo que requiere permisos de administrador; seleccionar los dos archivos de `muestras/` con distinta codificación; ordenar por tamaño una carpeta con archivos de 900 B y de 2 KB (con orden de texto, "900 B" quedaría después de "2.0 KB"); cancelar el diálogo de carpeta.

### Ejercicios de clase con solución

#### Ejercicio 7 · Contador con teclado, rueda y menú contextual (Sesión 9, 30 min)

Enunciado: una etiqueta grande con un número. Flecha arriba o `+` suma 1; flecha abajo o `-` resta 1; la rueda del ratón sobre la etiqueta suma o resta; clic derecho abre un menú con "Reiniciar" y "Copiar valor".

```python
"""Solución E7: eventos de teclado, rueda y menú contextual."""
import sys
import tkinter as tk

import customtkinter as ctk

ventana = ctk.CTk()
ventana.title("Contador")
ventana.geometry("300x220")

valor = ctk.IntVar(value=0)
etiqueta = ctk.CTkLabel(ventana, textvariable=valor, font=ctk.CTkFont(size=64, weight="bold"))
etiqueta.pack(expand=True)
ctk.CTkLabel(ventana, text="↑/↓, +/-, rueda o clic derecho").pack(pady=(0, 16))


def cambiar(delta: int) -> None:
    """Suma delta al contador."""
    valor.set(valor.get() + delta)


def al_girar_rueda(event: tk.Event) -> None:
    """En Windows delta es ±120 por paso; en macOS, valores pequeños. Basta el signo."""
    cambiar(1 if event.delta > 0 else -1)


def copiar_valor() -> None:
    """Coloca el número en el portapapeles."""
    ventana.clipboard_clear()
    ventana.clipboard_append(str(valor.get()))


menu = tk.Menu(ventana, tearoff=0)
menu.add_command(label="Reiniciar", command=lambda: valor.set(0))
menu.add_command(label="Copiar valor", command=copiar_valor)


def mostrar_menu(event: tk.Event) -> None:
    """Abre el menú contextual donde está el puntero."""
    try:
        menu.tk_popup(event.x_root, event.y_root)
    finally:
        menu.grab_release()


for secuencia in ("<Up>", "<plus>", "<KP_Add>"):
    ventana.bind(secuencia, lambda _e: cambiar(1))
for secuencia in ("<Down>", "<minus>", "<KP_Subtract>"):
    ventana.bind(secuencia, lambda _e: cambiar(-1))

if sys.platform.startswith("linux"):
    etiqueta.bind("<Button-4>", lambda _e: cambiar(1))
    etiqueta.bind("<Button-5>", lambda _e: cambiar(-1))
else:
    etiqueta.bind("<MouseWheel>", al_girar_rueda)

etiqueta.bind("<Button-2>" if sys.platform == "darwin" else "<Button-3>", mostrar_menu)

ventana.mainloop()
```

El punto de discusión: `<plus>` y `<KP_Add>` son teclas distintas (fila principal y teclado numérico). Los nombres de tecla se descubren con un callback de prueba que imprime `event.keysym`.

#### Ejercicio 8 · Bloc de notas con abrir, guardar y confirmación al salir (Sesión 10, 45 min)

Enunciado: un `CTkTextbox` con botones y atajos para Nuevo (Ctrl+N), Abrir (Ctrl+O) y Guardar (Ctrl+S). El título de la ventana muestra el nombre del archivo y un asterisco si hay cambios sin guardar. Si hay cambios, Nuevo, Abrir y cerrar la ventana preguntan si se desea guardar (Sí, No, Cancelar).

```python
"""Solución E8: bloc de notas con diálogos de archivo y confirmación."""
import tkinter as tk
from pathlib import Path
from tkinter import filedialog, messagebox

import customtkinter as ctk

TIPOS = [("Texto", "*.txt"), ("Todos", "*.*")]

ventana = ctk.CTk()
ventana.geometry("640x480")

barra = ctk.CTkFrame(ventana, fg_color="transparent")
barra.pack(fill="x", padx=12, pady=(12, 0))
editor = ctk.CTkTextbox(ventana, wrap="word", undo=True)
editor.pack(fill="both", expand=True, padx=12, pady=12)

archivo_actual: Path | None = None
texto_guardado = ""


def contenido() -> str:
    """\"end-1c\" excluye el salto de línea que Text agrega siempre al final."""
    return editor.get("1.0", "end-1c")


def hay_cambios() -> bool:
    """Compara con lo último guardado o abierto."""
    return contenido() != texto_guardado


def actualizar_titulo(_event: tk.Event | None = None) -> None:
    """Nombre del archivo y asterisco si hay cambios."""
    nombre = archivo_actual.name if archivo_actual else "Sin título"
    ventana.title(f"{'*' if hay_cambios() else ''}{nombre} · Bloc de notas")


def guardar(_event: tk.Event | None = None) -> str:
    """Guarda; si el archivo es nuevo, pide la ruta. Devuelve \"break\" para los atajos."""
    global archivo_actual, texto_guardado
    if archivo_actual is None:
        ruta = filedialog.asksaveasfilename(parent=ventana, defaultextension=".txt", filetypes=TIPOS)
        if not ruta:
            return "break"
        archivo_actual = Path(ruta)
    try:
        archivo_actual.write_text(contenido(), encoding="utf-8")
    except OSError as error:
        messagebox.showerror("No se pudo guardar", str(error), parent=ventana)
        return "break"
    texto_guardado = contenido()
    actualizar_titulo()
    return "break"


def puede_descartar() -> bool:
    """True si se puede continuar: sin cambios, guardados o descartados."""
    if not hay_cambios():
        return True
    respuesta = messagebox.askyesnocancel("Cambios sin guardar", "¿Guardar los cambios?", parent=ventana)
    if respuesta is None:
        return False
    if respuesta:
        guardar()
        return not hay_cambios()  # False si el usuario canceló el diálogo de guardar
    return True


def cargar_texto(texto: str, ruta: Path | None) -> None:
    """Reemplaza el contenido del editor y marca el estado como guardado."""
    global archivo_actual, texto_guardado
    editor.delete("1.0", "end")
    editor.insert("1.0", texto)
    archivo_actual, texto_guardado = ruta, texto
    actualizar_titulo()


def nuevo(_event: tk.Event | None = None) -> str:
    """Ctrl+N."""
    if puede_descartar():
        cargar_texto("", None)
    return "break"


def abrir(_event: tk.Event | None = None) -> str:
    """Ctrl+O. Intenta UTF-8 y, si falla, Windows-1252."""
    if not puede_descartar():
        return "break"
    ruta = filedialog.askopenfilename(parent=ventana, filetypes=TIPOS)
    if not ruta:
        return "break"
    datos = Path(ruta).read_bytes()
    try:
        texto = datos.decode("utf-8")
    except UnicodeDecodeError:
        texto = datos.decode("cp1252", errors="replace")
    cargar_texto(texto, Path(ruta))
    return "break"


def al_cerrar() -> None:
    """Botón X de la ventana."""
    if puede_descartar():
        ventana.destroy()


for texto, accion in (("Nuevo", nuevo), ("Abrir", abrir), ("Guardar", guardar)):
    ctk.CTkButton(barra, text=texto, width=90, command=accion).pack(side="left", padx=(0, 8))

# Se enlazan en el editor para poder devolver "break": Text ya usa Ctrl+O (inserta una línea).
for secuencia, accion in (("<Control-n>", nuevo), ("<Control-o>", abrir), ("<Control-s>", guardar)):
    editor.bind(secuencia, accion)
editor.bind("<KeyRelease>", actualizar_titulo)
ventana.protocol("WM_DELETE_WINDOW", al_cerrar)

actualizar_titulo()
editor.focus_set()
ventana.mainloop()
```

Lo que suele fallar: sin `return "break"`, Ctrl+O abre el diálogo y además inserta un salto de línea, porque el widget de texto trae ese atajo de fábrica. Es un buen caso para explicar el orden de propagación del Tema 3.1.

#### Ejercicio 9 · Tabla de calificaciones editable (Sesión 11, 40 min)

Enunciado: dos campos (nombre y calificación de 0 a 10) y un botón "Agregar" insertan filas en un `Treeview`. Doble clic en una fila la carga en los campos y el botón pasa a decir "Actualizar". La tecla Supr elimina la fila seleccionada previa confirmación. Las calificaciones menores a 6 se muestran en rojo. Una etiqueta muestra el promedio. Clic en los encabezados ordena.

```python
"""Solución E9: tabla de calificaciones con alta, edición y baja."""
import tkinter as tk
from itertools import count
from tkinter import messagebox, ttk

import customtkinter as ctk

MINIMA_APROBATORIA = 6.0

ventana = ctk.CTk()
ventana.title("Calificaciones")
ventana.geometry("460x460")
ventana.columnconfigure(1, weight=1)
ventana.rowconfigure(2, weight=1)

nombre = ctk.CTkEntry(ventana, placeholder_text="Nombre")
nombre.grid(row=0, column=0, columnspan=2, sticky="ew", padx=16, pady=(16, 6))
nota = ctk.CTkEntry(ventana, placeholder_text="Calificación", width=110)
nota.grid(row=1, column=0, sticky="w", padx=16)
boton = ctk.CTkButton(ventana, text="Agregar")
boton.grid(row=1, column=1, sticky="e", padx=16)

estilo = ttk.Style()
estilo.theme_use("clam")
tabla = ttk.Treeview(ventana, columns=("nombre", "nota"), show="headings", selectmode="browse")
tabla.grid(row=2, column=0, columnspan=2, sticky="nsew", padx=16, pady=12)
tabla.tag_configure("reprobado", foreground="#c0392b")
promedio = ctk.CTkLabel(ventana, text="")
promedio.grid(row=3, column=0, columnspan=2, pady=(0, 12))

ids = count(1)                 # genera iid únicos: 1, 2, 3...
en_edicion: str | None = None  # iid de la fila que se está editando


def leer_formulario() -> tuple[str, float] | None:
    """Valida los campos; devuelve None y avisa si hay error."""
    texto_nombre = nombre.get().strip()
    try:
        valor = float(nota.get().replace(",", "."))
    except ValueError:
        valor = -1
    if not texto_nombre or not 0 <= valor <= 10:
        messagebox.showwarning("Datos incompletos", "Escribe un nombre y una calificación de 0 a 10.",
                               parent=ventana)
        return None
    return texto_nombre, valor


def actualizar_promedio() -> None:
    """Recalcula el promedio con los valores de la tabla."""
    notas = [float(tabla.set(iid, "nota")) for iid in tabla.get_children()]
    texto = f"Promedio: {sum(notas) / len(notas):.2f} ({len(notas)} alumnos)" if notas else "Sin registros"
    promedio.configure(text=texto)


def limpiar_formulario() -> None:
    """Vacía los campos y regresa al modo de alta."""
    global en_edicion
    en_edicion = None
    nombre.delete(0, "end")
    nota.delete(0, "end")
    boton.configure(text="Agregar")
    nombre.focus_set()


def guardar() -> None:
    """Agrega o actualiza según el modo."""
    datos = leer_formulario()
    if datos is None:
        return
    valores = (datos[0], f"{datos[1]:.1f}")
    etiquetas = ("reprobado",) if datos[1] < MINIMA_APROBATORIA else ()
    if en_edicion is None:
        tabla.insert("", "end", iid=str(next(ids)), values=valores, tags=etiquetas)
    else:
        tabla.item(en_edicion, values=valores, tags=etiquetas)
    limpiar_formulario()
    actualizar_promedio()


def editar(event: tk.Event) -> None:
    """Doble clic: carga la fila en el formulario."""
    global en_edicion
    iid = tabla.identify_row(event.y)
    if not iid:
        return
    en_edicion = iid
    texto_nombre, texto_nota = tabla.item(iid, "values")
    nombre.delete(0, "end")
    nombre.insert(0, texto_nombre)
    nota.delete(0, "end")
    nota.insert(0, texto_nota)
    boton.configure(text="Actualizar")


def eliminar(_event: tk.Event) -> None:
    """Supr: borra la fila seleccionada tras confirmar."""
    seleccion = tabla.selection()
    if seleccion and messagebox.askyesno("Eliminar", "¿Eliminar el registro?", parent=ventana):
        tabla.delete(seleccion[0])
        if en_edicion == seleccion[0]:
            limpiar_formulario()
        actualizar_promedio()


def ordenar(columna: str, descendente: bool = False) -> None:
    """Ordena y reprograma el encabezado para invertir en el siguiente clic."""
    def clave(iid: str) -> float | str:
        valor = tabla.set(iid, columna)
        return float(valor) if columna == "nota" else valor.lower()

    for posicion, iid in enumerate(sorted(tabla.get_children(), key=clave, reverse=descendente)):
        tabla.move(iid, "", posicion)
    tabla.heading(columna, command=lambda: ordenar(columna, not descendente))


tabla.heading("nombre", text="Nombre", command=lambda: ordenar("nombre"))
tabla.heading("nota", text="Calificación", command=lambda: ordenar("nota"))
tabla.column("nota", width=110, anchor="center", stretch=False)

boton.configure(command=guardar)
nota.bind("<Return>", lambda _e: guardar())
tabla.bind("<Double-1>", editar)
tabla.bind("<Delete>", eliminar)
actualizar_promedio()

ventana.mainloop()
```

La función `ordenar` muestra otra forma de alternar el orden: en lugar de guardar el estado en un diccionario, cada clic vuelve a programar el `command` del encabezado con el valor contrario.

### Errores frecuentes del Módulo 3

| Síntoma | Causa | Solución |
| --- | --- | --- |
| `TypeError: ... takes 0 positional arguments but 1 was given` | Función sin parámetros usada con `.bind()`, que siempre envía `event` | Declarar `_event: tk.Event \| None = None` |
| Todos los botones creados en un ciclo hacen lo mismo | Enlace tardío en `lambda: f(i)` | `lambda n=i: f(n)` o `partial(f, i)` |
| Un atajo hace su acción y además escribe en el campo | El widget de texto tiene su propio comportamiento para esa tecla | Enlazar en el widget y devolver `"break"` |
| `<Control-s>` no responde a veces | Bloq Mayús activo: llega `<Control-S>` | Enlazar ambas secuencias |
| La rueda del ratón no hace nada en Linux | Linux usa `<Button-4>` y `<Button-5>` | Enlazar según `sys.platform` |
| `FileNotFoundError: [Errno 2] No such file or directory: ''` | Se usó la ruta sin comprobar que el diálogo se canceló | `if not ruta: return` |
| `UnicodeDecodeError: 'utf-8' codec can't decode byte ...` | Archivo guardado en Windows-1252 | Intentar `utf-8` y después `cp1252` |
| La tabla queda blanca en modo oscuro | `ttk.Treeview` no sigue el modo de CustomTkinter | `ttk.Style` con tema `clam` y colores por modo |
| `TclError: Item ... already exists` | `iid` repetido en `insert()` | Usar una clave única (id, ruta) o un contador |
| Los números se ordenan como "10, 2, 9" | Se ordenó el texto mostrado | Convertir en la clave de orden o guardar el valor original aparte |
| El diálogo aparece detrás de la ventana | Falta `parent=ventana` | Pasar siempre `parent` |
