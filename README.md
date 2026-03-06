# 102-Text-Animation-for-godot-4.x
Una recopilación de animaciones personalizadas para utilizar en nodos `RichTextLabel`,

---

## 1. Fade In (Desvanecimiento)

### 🖼️ Muestra:
![Fade In ](https://github.com/user-attachments/assets/dbbaffd6-7251-4d0c-ac87-c6ca96ea7e45)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFadeIn
extends RichTextEffect

# ejemplo de uso [fade_in speed=1.0 delay=0.05]Texto que aparece[/fade_in]
var bbcode = "fade_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	# Parámetros desde el BBCode
	var speed = char_fx.env.get("speed", 2.0)      # que tan rapido sera la aparicion
	var delay = char_fx.env.get("delay", 0.1)     # tiempo a esperar para aplicara al siguienbte caracter
	var start_wait = char_fx.env.get("wait", 0.0) # esperaaa
	
	# Calculamos el tiempo de inicio para este carácter específico
	var my_start_time = start_wait + (char_fx.relative_index * delay)
	
	# Calculamos el progreso del fade (de 0.0 a 1.0)
	var alpha = (char_fx.elapsed_time - my_start_time) * speed
	
	# Limitamos el valor entre 0 (invisible) y 1 (opaco)
	char_fx.color.a = clamp(alpha, 0.0, 1.0)
	
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Un valor más bajo hace que el fade sea más lento. |
| **delay** | `0.1` | Tiempo de espera entre la aparición de cada letra. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar a mostrar la primera letra. |

> **Ejemplo de uso:**
> `[fade_in speed=1.0 delay=0.05 wait=0.2]Texto que aparece suavemente[/fade_in]`

---

## 2. Typewriter (Máquina de escribir)

### 🖼️ Muestra:
![Typewriter](https://github.com/user-attachments/assets/a2882d15-0181-4a03-87a3-86269749fd04)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextTypewriter
extends RichTextEffect

# ejemplo de uso [type speed=20.0]Este texto se escribe solo...[/type]
var bbcode = "type"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 15.0) # cantidad de letras por segundo
	var wait = char_fx.env.get("wait", 0.0)    # tiempo que debe esperar para empezar 
	
	# El tiempo que ha pasado para saber si se debe mostrar el siguiente 
	var time = char_fx.elapsed_time - wait
	var finish_time = char_fx.relative_index / speed
	
	if time < finish_time:
		char_fx.color.a = 0.0
	else:
		char_fx.color.a = 1.0
		
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `15.0` | Cantidad de letras que se muestran por segundo. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar a escribir la primera letra. |

> **Ejemplo de uso:**
> `[type speed=20.0 wait=0.5]Texto de ejemplo.[/type]`

---

## 3. Decode (Decodificación)

### 🖼️ Muestra:
![Decode](https://github.com/user-attachments/assets/6f8b64ba-ded6-412f-b2ea-f3f1a92d483f)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextDecode
extends RichTextEffect

# ejemplo de uso [decode speed=10.0 wait=0.5]Texto ejemplo[/decode]
var bbcode = "decode"

const SYMBOLS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 5.0)
	var wait = char_fx.env.get("wait", 0.0)
	var time = char_fx.elapsed_time - wait
	
	# Revelar el carácter original 
	var reveal_time = char_fx.relative_index / speed
	
	if time < reveal_time:
		# Colocación de un símbolo aleatorio
		var noise = int(char_fx.elapsed_time * 25.0) + char_fx.relative_index
		var symbol_idx = noise % SYMBOLS.length()
		var char_unicode = SYMBOLS.unicode_at(symbol_idx)
		char_fx.glyph_index = TextServerManager.get_primary_interface().font_get_glyph_index(char_fx.font, 1, char_unicode, 0)
		
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `5.0` | Velocidad a la que se revelan los caracteres originales. |
| **wait** | `0.0` | Segundos de espera antes de comenzar el proceso. |

> **Ejemplo de uso:**
> `[decode speed=10.0 wait=0.5]Texto de ejemplo.[/decode]`

---

## 4. Slide Up (Desplazamiento hacia arriba)

### 🖼️ Muestra:
![4](https://github.com/user-attachments/assets/37461ac7-a1a2-4a62-94b0-92c890283c97)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlideUp
extends RichTextEffect

# ejemplo de uso [slide_up speed=2.0 delay=0.05 dist=20.0 wait=0.0]Slide Up Reveal[/slide_up]
var bbcode = "slide_up"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)   # espera entre caracteres
	var distance = char_fx.env.get("dist", 20.0) # distancia en que se desplazara
	var wait = char_fx.env.get("wait", 0.0)      # tiempo de espera para iniciar 
	
	var my_start_time = wait + (char_fx.relative_index * delay)
	var progress = clamp((char_fx.elapsed_time - my_start_time) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform.origin.y += (1.0 - progress) * distance
	
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la transición. |
| **delay** | `0.05` | Tiempo de espera entre la animación de cada letra. |
| **dist** | `20.0` | Distancia en píxeles desde la que sube el texto. |
| **wait** | `0.0` | Segundos de espera globales antes de iniciar. |

> **Ejemplo de uso:**
> `[slide_up speed=3.0 delay=0.1 dist=30.0 wait=0.5]Texto de ejemplo.[/slide_up]`

---

## 5. Bounce (Rebote de entrada)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextBounce
extends RichTextEffect
 
var bbcode = "bounce"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.08)
	var dist = char_fx.env.get("dist", 50.0)
	
	var t = max(0.0, char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	var y = 0.0
	if t < 1.0:
		y = pow(2.0, -10.0 * t) * abs(cos(t * PI * 2.5)) * dist
		char_fx.color.a = clamp(t * 5.0, 0.0, 1.0)
	
	char_fx.transform.origin.y -= y
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad general del rebote. |
| **dist** | `50.0` | Altura máxima del salto inicial. |
| **delay** | `0.08` | Tiempo de espera entre cada letra. |

> **Ejemplo de uso:**
> `[bounce speed=2.0 delay=0.08 dist=50.0]Texto de ejemplo.[/bounce]`

---

## 6. Rotate In (Giro con Escala)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRotateIn
extends RichTextEffect

var bbcode = "rotate_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var progress = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform = char_fx.transform.rotated_local(lerp(-PI, 0.0, progress))
	char_fx.transform = char_fx.transform.scaled_local(Vector2(progress, progress))
	
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rapidez del giro y crecimiento. |
| **delay** | `0.05` | Escalonamiento entre caracteres. |

> **Ejemplo de uso:**
> `[rotate_in speed=2.0 delay=0.05]Texto de ejemplo.[/rotate_in]`

---

## 7. Slide Down (Caída suave)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlideDown
extends RichTextEffect

var bbcode = "slide_down"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var dist = char_fx.env.get("dist", 30.0)
	var progress = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform.origin.y -= (1.0 - progress) * dist
	
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la caída. |
| **dist** | `30.0` | Distancia desde la parte superior. |
| **delay** | `0.05` | Tiempo entre cada letra. |

> **Ejemplo de uso:**
> `[slide_down speed=2.0 delay=0.05 dist=30.0]Texto de ejemplo.[/slide_down]`
```
