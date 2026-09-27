# Manual del Instructor · Curso-Taller de Interfaces Gráficas con Python

Sep 27, 2026 · @Noe Cazarez Camargo

## Presentación del curso

El curso-taller dura 40 horas repartidas en 5 módulos de 8 horas. El participante termina con una aplicación de escritorio empaquetada como ejecutable, construida con Tkinter y CustomTkinter. Este manual está escrito para quien imparte el curso: cada módulo trae distribución por sesiones, guion de clase, código de demostración, ejercicios con solución y rúbrica del entregable.

### Ficha técnica

| Dato | Valor |
| --- | --- |
| Nombre | Curso-Taller de Interfaces Gráficas de Usuario (UI/GUI) con Python |
| Modalidad | Curso-taller: 30 % exposición, 70 % práctica guiada |
| Duración | 40 horas (5 módulos × 8 horas) |
| Sesiones | 20 sesiones de 2 horas (4 por módulo) |
| Requisitos del participante | Python básico: tipos de datos, condicionales, ciclos, funciones y listas/diccionarios. Conocer clases ayuda desde el Módulo 4. |
| Producto final | Aplicación de escritorio con login, panel principal, tareas en segundo plano y ejecutable distribuible |

### Perfil de egreso

Al terminar, el participante puede:

- Diseñar pantallas de escritorio aplicando consistencia, jerarquía visual y retroalimentación al usuario.
- Distribuir widgets con `.pack()`, `.grid()` y `.place()`, y elegir el gestor adecuado para cada contenedor.
- Construir interfaces modernas con CustomTkinter, incluidos temas claro/oscuro y temas JSON propios.
- Programar interfaces orientadas a eventos, con diálogos, tablas `ttk.Treeview` y varias ventanas.
- Ejecutar tareas largas en hilos sin congelar la interfaz.
- Empaquetar la aplicación con PyInstaller, incluyendo imágenes, fuentes y temas.

### Software del laboratorio

| Componente | Versión | Instalación |
| --- | --- | --- |
| Python | 3.10 o superior | python.org; en Windows marcar "Add python.exe to PATH" |
| Tkinter | Incluido con Python | En Linux (Debian/Ubuntu): `sudo apt install python3-tk` |
| CustomTkinter | 5.2 o superior | `pip install customtkinter` (desde el Módulo 2) |
| Pillow | Actual | `pip install pillow` (Módulo 5) |
| PyInstaller | Actual | `pip install pyinstaller` (Módulo 5) |
| Editor | VS Code con extensión Python, o PyCharm | — |

Cada participante trabaja en un entorno virtual propio (`python -m venv .venv`). Antes de la primera sesión conviene confirmar en todos los equipos que `python -m tkinter` abre la ventana de prueba de Tk.

### Metodología

Cada sesión sigue el mismo ciclo: el instructor explica un concepto en 15 a 20 minutos, lo demuestra con código en vivo y los participantes lo reproducen y modifican. La última sesión de cada módulo se dedica al entregable. Los ejercicios son independientes entre sí: ningún módulo depende del código que el grupo escribió en el anterior, lo que permite reincorporar a quien faltó.

### Evaluación

| Componente | Peso |
| --- | --- |
| Entregables de los Módulos 1 a 4 (15 % cada uno) | 60 % |
| Proyecto final (Módulo 5) | 30 % |
| Ejercicios en clase | 10 % |

Cada entregable se califica con la rúbrica incluida al final de su módulo.

## Índice detallado

El curso avanza de la ventana más simple de Tkinter a una aplicación empaquetada. Cada módulo tiene 8 horas y cierra con un entregable evaluable.

| Módulo | Horas | Eje | Entregable |
| --- | --- | --- | --- |
| 1. Fundamentos de UI/UX y anatomía de Tkinter | 8 | Ventana, layouts, widgets básicos | Formularios con validación en tiempo real |
| 2. Modernización visual con CustomTkinter | 8 | Temas, widgets modernos, organización modular | Rediseño de un dashboard administrativo |
| 3. Eventos y diálogos | 8 | `.bind()`, callbacks, diálogos, `ttk.Treeview` | Explorador de archivos con vista tabular |
| 4. Arquitectura UI, hilos y multiventana | 8 | Clases, MVC, `CTkToplevel`, `threading` | Login + panel + proceso asíncrono con `CTkProgressBar` |
| 5. Proyecto final y empaquetado | 8 | Pillow, fuentes, accesibilidad, PyInstaller | Aplicación ejecutable documentada |

### Módulo 1 · Fundamentos de UI/UX y anatomía de Tkinter

1. **1.1 Diseño de interfaces y anatomía de la ventana.** Consistencia, jerarquía visual y retroalimentación. `Tk()`, título, geometría, área cliente y bucle principal (`mainloop`).
2. **1.2 Gestores de geometría.** `.pack()` (orden, `side`, `fill`, `expand`, padding), `.grid()` (filas, columnas, `sticky`, `columnspan`, pesos) y `.place()` (coordenadas absolutas y relativas; cuándo evitarlo).
3. **1.3 Widgets esenciales y variables de control.** `Label`, `Entry`, `Button`; `StringVar`, `IntVar`, `BooleanVar` y `trace_add()`.
4. **1.4 Validación en tiempo real (taller).** `validatecommand` frente a `trace_add()`. Entregable: formularios interactivos con validación.

### Módulo 2 · Modernización visual con CustomTkinter

1. **2.1 De Tkinter a CustomTkinter.** Limitaciones visuales de Tkinter estándar (escalado en pantallas HiDPI, esquinas, modo oscuro). Instalación y equivalencias `tk.Button` → `ctk.CTkButton`.
2. **2.2 Temas y apariencia.** `set_appearance_mode()` (light, dark, system), `set_default_color_theme()` y creación de un tema JSON propio a partir de uno incluido.
3. **2.3 Widgets modernos.** `CTkButton`, `CTkEntry`, `CTkOptionMenu`, `CTkComboBox`, `CTkSwitch`, `CTkSlider`.
4. **2.4 Organización modular.** Agrupación con `CTkFrame`, pestañas con `CTkTabview` y áreas desplazables con `CTkScrollableFrame`. Entregable: rediseño de un dashboard administrativo.

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
