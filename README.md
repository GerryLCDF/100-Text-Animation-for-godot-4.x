# +101-Text-Animation-for-godot-4.x
Una recopilación de animaciones personalizadas para utilizar en nodos `RichTextLabel`.

####como utilizar cualquiera de estos recursos ,recuerda que esto es solo un recurso si lo llegas a utilizar en alguno de tus proyectos no me molesataria que me dieras creditos ,pero siempre eres libre de no hacerlo ,
---

## 1. Fade In (Aparición Suave)

### 🖼️ Muestra:
![1](https://github.com/user-attachments/assets/89ea952a-54c7-4d28-80e6-6ee40fb850cd)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFadeIn
extends RichTextEffect

# ejemplo de uso [fade_in speed=1.0 delay=0.05 wait=0.0]Texto que aparece[/fade_in]
var bbcode = "fade_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.1)
	var start_wait = char_fx.env.get("wait", 0.0)
	
	var my_start_time = start_wait + (char_fx.relative_index * delay)
	var alpha = (char_fx.elapsed_time - my_start_time) * speed
	
	char_fx.color.a = clamp(alpha, 0.0, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la aparición suave. |
| **delay** | `0.1` | Tiempo de espera entre cada letra. |
| **wait** | `0.0` | Segundos de espera globales antes de iniciar. |

> **Ejemplo de uso:**
> `[fade_in speed=1.0 delay=0.05 wait=0.2]Texto de ejemplo.[/fade_in]`

---

## 2. Typewriter (Máquina de Escribir)

### 🖼️ Muestra:
![2](https://github.com/user-attachments/assets/1ed074f3-63ca-41ee-9477-ddff1e8b3b09)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextTypewriter
extends RichTextEffect

# ejemplo de uso [type speed=20.0 wait=0.0]Este texto se escribe solo...[/type]
var bbcode = "type"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 15.0)
	var wait = char_fx.env.get("wait", 0.0)
	
	var time = char_fx.elapsed_time - wait
	var finish_time = char_fx.relative_index / speed
	
	char_fx.color.a = 0.0 if time < finish_time else 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `15.0` | Cantidad de letras que se muestran por segundo. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar. |

> **Ejemplo de uso:**
> `[type speed=20.0 wait=0.5]Texto de ejemplo.[/type]`

---

## 3. Decode (Descifrado de Símbolos)

### 🖼️ Muestra:
![3](https://github.com/user-attachments/assets/6dd7506e-947e-42ba-a4a5-8c13a443fa74)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextDecode
extends RichTextEffect

# ejemplo de uso [decode speed=10.0 wait=0.5]Texto de ejemplo[/decode]
var bbcode = "decode"
const SYMBOLS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 5.0)
	var wait = char_fx.env.get("wait", 0.0)
	var time = char_fx.elapsed_time - wait
	var reveal_time = char_fx.relative_index / speed
	
	if time < reveal_time:
		var noise = int(char_fx.elapsed_time * 25.0) + char_fx.relative_index
		var symbol_idx = noise % SYMBOLS.length()
		var char_unicode = SYMBOLS.unicode_at(symbol_idx)
		char_fx.glyph_index = TextServerManager.get_primary_interface().font_get_glyph_index(char_fx.font, 1, char_unicode, 0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `5.0` | Velocidad de revelado de los caracteres. |
| **wait** | `0.0` | Retraso antes de iniciar el descifrado. |

> **Ejemplo de uso:**
> `[decode speed=10.0 wait=0.5]Texto de ejemplo.[/decode]`

---

## 4. Slide Up (Deslizar hacia Arriba)

### 🖼️ Muestra:
![4](https://github.com/user-attachments/assets/0d368e15-8347-4d72-a3b4-9768a754aa10)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlideUp
extends RichTextEffect

var bbcode = "slide_up"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var distance = char_fx.env.get("dist", 20.0)
	var wait = char_fx.env.get("wait", 0.0)
	
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
| **delay** | `0.05` | Tiempo entre cada letra. |
| **dist** | `20.0` | Distancia desde la que sube el texto. |

> **Ejemplo de uso:**
> `[slide_up speed=2.0 delay=0.05 dist=20.0]Texto de ejemplo.[/slide_up]`

---

## 5. Bounce (Rebote Elástico)

### 🖼️ Muestra:
![5](https://github.com/user-attachments/assets/0dd4c149-950b-4e4b-bd3c-bb59b59e71d9)

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
| **speed** | `2.0` | Velocidad del rebote. |
| **delay** | `0.08` | Tiempo de espera entre letras. |
| **dist** | `50.0` | Altura máxima del salto inicial. |

> **Ejemplo de uso:**
> `[bounce speed=2.0 delay=0.08 dist=50.0]Texto de ejemplo.[/bounce]`

---

## 6. Rotate In (Giro con Escala)

### 🖼️ Muestra:
![6](https://github.com/user-attachments/assets/7d39607b-bec0-44df-a44c-2b8e72920183)

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

## 7. Slide Down (Deslizar hacia Abajo)

### 🖼️ Muestra:
![7](https://github.com/user-attachments/assets/4ca6df90-957e-4f2c-a449-0b931f9d4eba)

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
| **speed** | `2.0` | Velocidad de caída. |
| **delay** | `0.05` | Tiempo entre cada letra. |
| **dist** | `30.0` | Distancia desde la que baja el texto. |

> **Ejemplo de uso:**
> `[slide_down speed=2.0 delay=0.05 dist=30.0]Texto de ejemplo.[/slide_down]`

---

## 8. Slide Left (Deslizar desde la Izquierda)

### 🖼️ Muestra:
![8](https://github.com/user-attachments/assets/bec1595f-d6c9-41d3-9ca8-b414c7a3002e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlideLeft
extends RichTextEffect

var bbcode = "slide_left"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var dist = char_fx.env.get("dist", 40.0)
	var progress = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform.origin.x -= (1.0 - progress) * dist
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del deslizamiento. |
| **delay** | `0.05` | Escalonamiento entre caracteres. |
| **dist** | `40.0` | Distancia horizontal del efecto. |

> **Ejemplo de uso:**
> `[slide_left speed=2.0 delay=0.05 dist=40.0]Texto de ejemplo.[/slide_left]`

---

## 9. Slide Right (Deslizar desde la Derecha)

### 🖼️ Muestra:
![9](https://github.com/user-attachments/assets/2fdd0dfd-c39a-4fb9-b132-edd6188e4640)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlideRight
extends RichTextEffect

var bbcode = "slide_right"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var dist = char_fx.env.get("dist", 40.0)
	var progress = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform.origin.x += (1.0 - progress) * dist
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del deslizamiento. |
| **delay** | `0.05` | Escalonamiento entre caracteres. |
| **dist** | `40.0` | Distancia horizontal del efecto. |

> **Ejemplo de uso:**
> `[slide_right speed=2.0 delay=0.05 dist=40.0]Texto de ejemplo.[/slide_right]`

---

## 10. Drop In (Caída con Rebote Pesado)

### 🖼️ Muestra:
![10](https://github.com/user-attachments/assets/26398767-e164-48d1-95f2-914898a5d6b0)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextDropIn
extends RichTextEffect

var bbcode = "drop_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.5)
	var delay = char_fx.env.get("delay", 0.06)
	var dist = char_fx.env.get("dist", 100.0)
	
	var t = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	char_fx.color.a = clamp(t * 2.0, 0.0, 1.0)
	
	var y_offset = 0.0
	if t < 1.0:
		if t < 0.6: y_offset = lerp(dist, -dist * 0.2, t / 0.6)
		elif t < 0.8: y_offset = lerp(-dist * 0.2, dist * 0.1, (t - 0.6) / 0.2)
		else: y_offset = lerp(dist * 0.1, 0.0, (t - 0.8) / 0.2)
	
	char_fx.transform.origin.y -= y_offset
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.5` | Velocidad de caída y rebote. |
| **delay** | `0.06` | Tiempo entre cada letra. |
| **dist** | `100.0` | Altura desde la que cae. |

> **Ejemplo de uso:**
> `[drop_in speed=1.5 delay=0.06 dist=100.0]Texto de ejemplo.[/drop_in]`

---

## 11. Swing (Balanceo de Péndulo)

### 🖼️ Muestra:
![11](https://github.com/user-attachments/assets/d5ae0b83-e516-4868-86fb-5005c6a55692)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSwing
extends RichTextEffect

var bbcode = "swing"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.2)
	var delay = char_fx.env.get("delay", 0.05)
	var max_angle = char_fx.env.get("angle", 15.0)
	
	var t = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	char_fx.color.a = clamp(t * 5.0, 0.0, 1.0)
	
	if t < 1.0:
		var rot = cos(t * PI * 3.0) * exp(-t * 2.0) * deg_to_rad(max_angle)
		char_fx.transform = char_fx.transform.rotated_local(rot)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.2` | Velocidad del balanceo. |
| **delay** | `0.05` | Escalonamiento entre letras. |
| **angle** | `15.0` | Ángulo máximo de balanceo inicial. |

> **Ejemplo de uso:**
> `[swing speed=1.2 delay=0.05 angle=15.0]Texto de ejemplo.[/swing]`

---

## 12. Pulse In (Pulso de Entrada)

### 🖼️ Muestra:
![12](https://github.com/user-attachments/assets/c9aa3d93-2eaa-456f-93a6-a231411e1bbc)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextPulse
extends RichTextEffect

var bbcode = "pulse"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var max_scale = char_fx.env.get("scale", 1.2)
	
	var t = clamp((char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	char_fx.color.a = clamp(t * 2.0, 0.0, 1.0)
	
	var s = 1.0
	if t < 1.0:
		s = 1.0 + (sin(t * PI) * (max_scale - 1.0))
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(s, s))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del pulso. |
| **delay** | `0.05` | Tiempo entre cada letra. |
| **scale** | `1.2` | Tamaño máximo del pulso. |

> **Ejemplo de uso:**
> `[pulse speed=2.0 delay=0.05 scale=1.2]Texto de ejemplo.[/pulse]`

---

## 13. Scale In (Aparición con Escala)

### 🖼️ Muestra:
![13](https://github.com/user-attachments/assets/bc057bb2-c77e-42b1-9ac4-f0e8f708bd21)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextScaleIn extends RichTextEffect

var bbcode = "scale_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 4.0)
	var delay = char_fx.env.get("delay", 0.05)
	var wait = char_fx.env.get("wait", 0.0)
	
	var my_start_time = wait + (char_fx.relative_index * delay)
	var progress = clamp((char_fx.elapsed_time - my_start_time) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform = char_fx.transform.scaled_local(Vector2(progress, progress))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `4.0` | Velocidad con la que el texto crece desde cero. |
| **delay** | `0.05` | Tiempo de espera entre la escala de cada letra. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar. |

> **Ejemplo de uso:**
> `[scale_in speed=4.0 delay=0.05 wait=0.0]Texto de ejemplo[/scale_in]`

---

## 14. Flash (Entrada con Parpadeo)

### 🖼️ Muestra:
![14](https://github.com/user-attachments/assets/0fc76db2-a538-4991-8e53-a90fc7df670b)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFlash extends RichTextEffect

var bbcode = "flash"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < 1.0:
		var flash_val = abs(cos(t * PI * 2.0))
		char_fx.color.a = 1.0 if flash_val > 0.5 else 0.0
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rapidez del parpadeo inicial. |
| **delay** | `0.05` | Escalonamiento entre caracteres. |

> **Ejemplo de uso:**
> `[flash speed=2.0 delay=0.05]Texto de ejemplo[/flash]`

---

## 15. Shake X (Temblor Horizontal de Entrada)

### 🖼️ Muestra:
![15](https://github.com/user-attachments/assets/7f0f8723-ca12-4d10-a851-de676349cbcb)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextShakeX extends RichTextEffect

var bbcode = "shake_x"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var delay = char_fx.env.get("delay", 0.05)
	var strength = char_fx.env.get("strength", 10.0)
	
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < 1.0:
		char_fx.color.a = 1.0
		char_fx.transform.origin.x += sin(t * PI * 5.0) * strength * (1.0 - t)
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del temblor. |
| **strength** | `10.0` | Fuerza del movimiento horizontal. |

> **Ejemplo de uso:**
> `[shake_x speed=3.0 strength=10.0]Texto de ejemplo[/shake_x]`

---

## 16. Shake Y (Temblor Vertical de Entrada)

### 🖼️ Muestra:
![16](https://github.com/user-attachments/assets/ed6eea12-c64c-4994-8b82-fd569590c838)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextShakeY extends RichTextEffect

var bbcode = "shake_y"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var delay = char_fx.env.get("delay", 0.05)
	var strength = char_fx.env.get("strength", 10.0)
	
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < 1.0:
		char_fx.color.a = 1.0
		char_fx.transform.origin.y += sin(t * PI * 5.0) * strength * (1.0 - t)
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del temblor. |
| **strength** | `10.0` | Fuerza del movimiento vertical. |

> **Ejemplo de uso:**
> `[shake_y speed=3.0 strength=10.0]Texto de ejemplo[/shake_y]`

---

## 17. Tada (Efecto Celebración)

### 🖼️ Muestra:
![17](https://github.com/user-attachments/assets/9787470e-9995-4490-b196-124c045d0031)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextTada extends RichTextEffect

var bbcode = "tada"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < 1.0:
		char_fx.color.a = 1.0
		var scale = 1.0 + (sin(t * PI * 4.0) * 0.1)
		var rot = sin(t * PI * 8.0) * deg_to_rad(3.0)
		char_fx.transform = char_fx.transform.scaled_local(Vector2(scale, scale)).rotated_local(rot)
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la animación completa. |
| **delay** | `0.05` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[tada speed=2.0 delay=0.05]Texto de ejemplo[/tada]`

---

## 18. Reveal (Barrido Lateral)

### 🖼️ Muestra:
![18](https://github.com/user-attachments/assets/4c73261a-7873-4525-b69a-e53249c23396)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextReveal extends RichTextEffect

var bbcode = "reveal"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var wait = char_fx.env.get("wait", 0.0)
	
	var t = char_fx.elapsed_time - wait
	var progress = clamp((t - (char_fx.relative_index * delay)) * speed, 0.0, 1.0)
	
	char_fx.color.a = progress
	char_fx.transform.origin.x += (1.0 - progress) * 5.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del barrido. |
| **delay** | `0.05` | Tiempo entre letras. |
| **wait** | `0.0` | Tiempo de espera inicial. |

> **Ejemplo de uso:**
> `[reveal speed=2.0 delay=0.05 wait=0.0]Texto de ejemplo[/reveal]`

---

## 19. Wave Jump (Salto de Onda)

### 🖼️ Muestra:
![19](https://github.com/user-attachments/assets/ae09d903-b82e-4d19-bb21-cb6486f787a1)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWaveJump extends RichTextEffect

var bbcode = "wave_jump"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var height = char_fx.env.get("height", 10.0)
	var delay = char_fx.env.get("delay", 0.1)
	
	var time = (char_fx.elapsed_time * speed) - (char_fx.relative_index * delay * speed)
	char_fx.transform.origin.y -= max(0.0, sin(time)) * height
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del salto. |
| **height** | `10.0` | Altura máxima del salto. |
| **delay** | `0.1` | Curvatura de la onda. |

> **Ejemplo de uso:**
> `[wave_jump speed=3.0 height=10.0 delay=0.1]Texto de ejemplo[/wave_jump]`

---

## 20. Wave Rotate (Rotación Ondulada)

### 🖼️ Muestra:
![20](https://github.com/user-attachments/assets/e4b47e2f-3f15-47af-aad3-ed9de12a2c43)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWaveRotate extends RichTextEffect

var bbcode = "wave_rot"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var angle = char_fx.env.get("angle", 45.0)
	var delay = char_fx.env.get("delay", 0.2)
	
	var time = (char_fx.elapsed_time * speed) - (char_fx.relative_index * delay * speed)
	char_fx.transform = char_fx.transform.rotated_local(sin(time) * deg_to_rad(angle))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del balanceo. |
| **angle** | `45.0` | Ángulo máximo de rotación. |
| **delay** | `0.2` | Retraso entre caracteres. |

> **Ejemplo de uso:**
> `[wave_rot speed=3.0 angle=45.0 delay=0.2]Texto de ejemplo[/wave_rot]`

---

## 21. Roller 3D (Giro en Eje X)

### 🖼️ Muestra:
![21](https://github.com/user-attachments/assets/6e36c6cc-a529-4465-bc2e-523abb7a2942)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRoller3d extends RichTextEffect

var bbcode = "roller"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var height = char_fx.env.get("height", 1.0)
	var delay = char_fx.env.get("delay", 0.1)
	
	var time = (char_fx.elapsed_time * speed) - (char_fx.relative_index * delay * speed)
	var scale_y = cos(time)
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0, abs(scale_y) * height))
	
	var brightness = remap(abs(scale_y), 0.0, 1.0, 0.5, 1.0)
	char_fx.color.r *= brightness
	char_fx.color.g *= brightness
	char_fx.color.b *= brightness
	
	if scale_y < 0.0: char_fx.color.a *= 0.5
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad de rotación. |
| **height** | `1.0` | Multiplicador de escala vertical. |
| **delay** | `0.1` | Desfase entre cada letra. |

> **Ejemplo de uso:**
> `[roller speed=3.0 height=1.0 delay=0.1]Texto de ejemplo[/roller]`

---

## 22. Slide Swap (Intercambio Vertical Infinito)

### 🖼️ Muestra:
![22](https://github.com/user-attachments/assets/23d11673-d097-4809-b036-f648e2f4bb8b)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlideSwap extends RichTextEffect

var bbcode = "slide_swap"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var dist = char_fx.env.get("dist", 30.0)
	var delay = char_fx.env.get("delay", 0.1)
	
	var time = (char_fx.elapsed_time * speed) - (char_fx.relative_index * delay)
	var progress = fmod(time, 1.0)
	if progress < 0.0: progress += 1.0
	
	char_fx.transform.origin.y += lerp(dist, -dist, progress)
	char_fx.color.a *= sin(progress * PI)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del ciclo de intercambio. |
| **dist** | `30.0` | Distancia del desplazamiento vertical. |
| **delay** | `0.1` | Tiempo de espera entre caracteres. |

> **Ejemplo de uso:**
> `[slide_swap speed=2.0 dist=30.0 delay=0.1]Texto de ejemplo[/slide_swap]`

---

## 23. Jello Loop (Vibración Gelatinosa)

### 🖼️ Muestra:
![23](https://github.com/user-attachments/assets/72a32d25-2a53-44c4-82fb-9e6dbf596260)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextJelloLoop extends RichTextEffect

var bbcode = "jello_loop"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.1)
	var strength = char_fx.env.get("strength", 0.2)
	
	var t = (char_fx.elapsed_time * speed) - (char_fx.relative_index * delay)
	var s_x = 1.0 + (sin(t * PI) * strength)
	var s_y = 1.0 - (sin(t * PI) * strength)
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(s_x, s_y))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la oscilación gelatinosa. |
| **delay** | `0.1` | Desfase de la vibración entre letras. |
| **strength** | `0.2` | Intensidad de la deformación (Squash & Stretch). |

> **Ejemplo de uso:**
> `[jello_loop speed=2.0 delay=0.1 strength=0.2]Texto de ejemplo[/jello_loop]`

---

## 24. Rubber (Squash and Stretch de Entrada)

### 🖼️ Muestra:
![24](https://github.com/user-attachments/assets/a712e073-fa46-4d7b-a3a3-9bbcb9f8ab2b)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRubber extends RichTextEffect

var bbcode = "rubber"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < 1.5:
		char_fx.color.a = 1.0
		var bounce = sin(t * PI * 3.0) * exp(-t * 3.0)
		char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0 + bounce * 0.5, 1.0 - bounce * 0.5))
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del rebote elástico. |
| **delay** | `0.05` | Retraso entre la aparición de cada letra. |

> **Ejemplo de uso:**
> `[rubber speed=2.0 delay=0.05]Texto de ejemplo[/rubber]`

---

## 25. Wave Ocean (Ola Única)

### 🖼️ Muestra:
![25](https://github.com/user-attachments/assets/f3534b09-b810-496c-9bce-fab52a48b28f)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWaveOcean extends RichTextEffect

var bbcode = "wave_ocean"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var delay = char_fx.env.get("delay", 0.08)
	var dist = char_fx.env.get("dist", 15.0)
	
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < PI:
		char_fx.color.a = 1.0
		char_fx.transform.origin.y -= sin(t) * dist
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Rapidez del paso de la ola. |
| **delay** | `0.08` | Desfase para crear el efecto de curva. |
| **dist** | `15.0` | Altura máxima de la ola. |

> **Ejemplo de uso:**
> `[wave_ocean speed=3.0 delay=0.08 dist=15.0]Texto de ejemplo[/wave_ocean]`

---

## 26. Stretch (Estiramiento Horizontal)

### 🖼️ Muestra:
![26](https://github.com/user-attachments/assets/de1beb87-8542-439f-b2d2-d3c7446311a9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextStretch extends RichTextEffect

var bbcode = "stretch"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 4.0)
	var delay = char_fx.env.get("delay", 0.05)
	var max_stretch = char_fx.env.get("max_stretch", 1.5)
	
	var t = (char_fx.elapsed_time - (char_fx.relative_index * delay)) * speed
	if t < 0.0: 
		char_fx.color.a = 0.0
	elif t < PI:
		char_fx.color.a = 1.0
		char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0 + (sin(t) * (max_stretch - 1.0)), 1.0))
	else: 
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `4.0` | Velocidad del estiramiento. |
| **max_stretch** | `1.5` | Escala máxima horizontal (1.5 = 150%). |
| **delay** | `0.05` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[stretch speed=4.0 delay=0.05 max_stretch=1.5]Texto de ejemplo[/stretch]`

---

## 27. Alpha Scroll (Revelado por Opacidad)

### 🖼️ Muestra:
![27](https://github.com/user-attachments/assets/a2c148c9-f592-4af3-ae9b-6ba495b7aa59)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextAlphaScroll extends RichTextEffect

var bbcode = "alpha_scroll"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 10.0)
	var delay = char_fx.env.get("delay", 0.1)
	
	var time = char_fx.elapsed_time * speed - (char_fx.relative_index * delay)
	char_fx.color.a = clamp(time, 0.0, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `10.0` | Velocidad del revelado. |
| **delay** | `0.1` | Tiempo entre la aparición de cada letra. |

> **Ejemplo de uso:**
> `[alpha_scroll speed=10.0 delay=0.1]Texto de ejemplo[/alpha_scroll]`

---

## 28. Glitch Pop (Sustitución Aleatoria Brilla)

### 🖼️ Muestra:
![28](https://github.com/user-attachments/assets/f46b66a4-5242-4bfe-9cd4-95437ca19ffe)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextGlitchPop extends RichTextEffect

var bbcode = "glitch_pop"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.5)
	var f_size = char_fx.env.get("f_size", 16)
	var glitch_chars = ["!", "X", "#", "$", "?", "0", "%", "&"]
	
	var step = int(char_fx.elapsed_time * speed)
	var victim_idx = (step * 149) % char_fx.glyph_count
	
	if char_fx.relative_index == victim_idx:
		var ts = TextServerManager.get_primary_interface()
		var sub_step = int(char_fx.elapsed_time * 15.0)
		var target_char = glitch_chars[sub_step % glitch_chars.size()]
		var new_glyph = ts.font_get_glyph_index(char_fx.font, f_size, target_char.unicode_at(0), 0)
		
		if new_glyph != -1:
			char_fx.glyph_index = new_glyph
			char_fx.color = Color(10.0, 10.0, 10.0, 1.0) # Brillo HDR
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.5` | Frecuencia de los fallos (glitches). |
| **f_size** | `16` | Tamaño de fuente para el cálculo del glifo. |

> **Ejemplo de uso:**
> `[glitch_pop speed=2.5 f_size=16]Texto de ejemplo[/glitch_pop]`

---

## 29. Caesar (Cifrado con Pop de Revelado)

### 🖼️ Muestra:
![29](https://github.com/user-attachments/assets/65661f2b-3e16-48d6-9c8d-c07e49e7b9bc)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextCaesar extends RichTextEffect

var bbcode = "caesar"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var delay = char_fx.env.get("delay", 2.0)
	var reveal_speed = char_fx.env.get("reveal_speed", 5.0)
	var shift = char_fx.env.get("shift", 3)
	var reverse = char_fx.env.get("reverse", false)
	var pop_scale = char_fx.env.get("pop_scale", 1.4)
	
	var reveal_time = delay + (char_fx.relative_index / reveal_speed)
	var diff = char_fx.elapsed_time - reveal_time
	var should_encrypt = (char_fx.elapsed_time < reveal_time) if not reverse else (char_fx.elapsed_time > reveal_time)
	
	if diff > 0.0 and diff < 0.3:
		var pop_curve = sin((diff / 0.3) * PI)
		var current_scale = 1.0 + (pop_curve * (pop_scale - 1.0))
		char_fx.transform = char_fx.transform.scaled_local(Vector2(current_scale, current_scale))
		char_fx.color.v += pop_curve # Aumenta brillo
		
	if should_encrypt:
		var ts = TextServerManager.get_primary_interface()
		if char_fx.glyph_index != ts.font_get_glyph_index(char_fx.font, 16, 32, 0): # No espacios
			char_fx.glyph_index += shift
			char_fx.color = Color(0.15, 0.81, 0.82, 1.0) # Color cian cifrado
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **delay** | `2.0` | Segundos antes de empezar a descifrar. |
| **reveal_speed**| `5.0` | Velocidad de revelado de letras. |
| **shift** | `3` | Desfase de caracteres del cifrado. |
| **pop_scale** | `1.4` | Escala del efecto "pop" al revelarse. |

> **Ejemplo de uso:**
> `[caesar delay=2.0 reveal_speed=5.0 shift=3]Texto Cifrado[/caesar]`

---

## 30. Nervous (Agitación Errática)

### 🖼️ Muestra:
![30](https://github.com/user-attachments/assets/956efa5a-a9fd-4e7f-933d-f73b52a98c6c)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextNervous extends RichTextEffect

var bbcode = "nervous"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 20.0)
	var strength = char_fx.env.get("strength", 3.0)
	
	var time = char_fx.elapsed_time * speed
	var char_seed = char_fx.relative_index * 1337.0
	
	var shake_x = sin(time + char_seed) * cos(time * 1.7 + char_seed) * strength
	var shake_y = cos(time * 0.9 + char_seed) * sin(time * 2.1 + char_seed) * strength
	
	char_fx.offset.x += shake_x
	char_fx.offset.y += shake_y
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `20.0` | Rapidez de la agitación. |
| **strength** | `3.0` | Distancia máxima del temblor. |

> **Ejemplo de uso:**
> `[nervous speed=20.0 strength=3.0]Texto nervioso o asustado[/nervous]`

---

## 31. Squeeze In (Revelado por Compresión)

### 🖼️ Muestra:
![31](https://github.com/user-attachments/assets/fa9cdc2b-53f5-4322-9fd2-e17328d36a0e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSqueeze extends RichTextEffect

var bbcode = "squeeze"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var char_delay = char_fx.env.get("delay", 0.1)
	
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * char_delay), 0.0, 1.0)
	var scale_y = 1.0
	
	if time < 1.0:
		scale_y = lerp(1.0, 0.5, time * 2.0) if time < 0.5 else lerp(0.5, 1.0, (time - 0.5) * 2.0)
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0, scale_y))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la animación. |
| **delay** | `0.1` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[squeeze speed=2.0 delay=0.1]Texto de ejemplo[/squeeze]`

---

## 32. Color Shift Intro (Revelado Arcoíris)

### 🖼️ Muestra:
![32](https://github.com/user-attachments/assets/0456bbb5-6750-48af-b403-c0b48372a6d4)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextColorShiftIntro
extends RichTextEffect

var bbcode = "c_shift_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.0)
	var char_delay = char_fx.env.get("delay", 0.05)
	
	var c1 = Color(char_fx.env.get("c1", "#ffffff")) # Blanco inicial
	var c2 = Color(char_fx.env.get("c2", "#ff0055")) # Primer cambio
	var c3 = Color(char_fx.env.get("c3", "#00ccff")) # Segundo cambio
	var c_end = Color(char_fx.env.get("c_end", "#ffffff")) # Color final
	
	var time = char_fx.elapsed_time * speed - (char_fx.relative_index * char_delay)
	
	if time <= 0:
		char_fx.color.a = 0
		return true
	if time < 0.33:
		char_fx.color = c1.lerp(c2, time / 0.33)
	elif time < 0.66:
		char_fx.color = c2.lerp(c3, (time - 0.33) / 0.33)
	else:
		char_fx.color = c3.lerp(c_end, clamp((time - 0.66) / 0.34, 0, 1))
	char_fx.color.a = clamp(time * 5.0, 0, 1)
	
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.0` | Velocidad de la transición de color. |
| **delay** | `0.05` | Desfase cromático entre letras. |
| **c1** | `#ffffff` | color inciar de las letras. |
| **c2** | `#ff0055` | primer color de la introduccion. |
| **c3** | `#00ccff` | Segundo color de la introduccion. |
| **c_end** | `#ffffff` | el color que tendra al final el texto. |


> **Ejemplo de uso:**
> `[c_shift_in c1=#05614b c2=#020e0e c3=#01de82 c_end=#ffffff speed=1.5]Texto de ejemplo[/c_shift_in]`

---

## 33. Color Shift Loop (Bucle de Color Constante)

### 🖼️ Muestra:
![33](https://github.com/user-attachments/assets/dc9fd745-bb2b-4a0e-ab11-442240cace3b)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextColorShiftLoop
extends RichTextEffect

var bbcode = "c_shift_loop"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var freq = char_fx.env.get("freq", 0.2) 
	var c1 = Color(char_fx.env.get("c1", "#ffffff")) # Color A
	var c2 = Color(char_fx.env.get("c2", "#ff0055")) # Color B
	var c3 = Color(char_fx.env.get("c3", "#00ccff")) # Color C
	var shift = (char_fx.elapsed_time * speed) - (char_fx.relative_index * freq)
	var cycle = fposmod(shift, 3.0) 
	
	if cycle < 1.0:
		char_fx.color = c1.lerp(c2, cycle)
	elif cycle < 2.0:
		char_fx.color = c2.lerp(c3, cycle - 1.0)
	else:
		char_fx.color = c3.lerp(c1, cycle - 2.0)
		
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del ciclo de color. |
| **freq** | `0.5` | Distancia de color entre letras contiguas. |
| **c1** | `#ffffff` | Primer color del bucle |
| **c2** | `#ff0055` | Segundo color del bulce. |
| **c3** | `#00ccff` | Tercer color del bucle. |

> **Ejemplo de uso:**
> `[c_shift_loop c1=#ff0000 c2=#ffaa00 c3=#ffff00 freq=0.1]TEXTO CALIENTE[/c_shift_loop]`

---

## 34. Zoom In Fast (Revelado desde 0)

### 🖼️ Muestra:
![34](https://github.com/user-attachments/assets/06051c89-8cd9-4912-8f86-18d5066210ec)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextZoomIn extends RichTextEffect

var bbcode = "zoom_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var char_delay = char_fx.env.get("delay", 0.05)
	
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * char_delay), 0.0, 1.0)
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(time, time))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del zoom. |
| **delay** | `0.05` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[zoom_in speed=3.0 delay=0.05]Texto de ejemplo[/zoom_in]`

---

## 35. Zoom Out Slow (Revelado desde 2.0)

### 🖼️ Muestra:
![35](https://github.com/user-attachments/assets/c0728403-8da7-47dd-8580-25e9c3ec6ef7)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextZoomOut extends RichTextEffect

var bbcode = "zoom_out"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var char_delay = char_fx.env.get("delay", 0.05)
	
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * char_delay), 0.0, 1.0)
	var scale = lerp(2.0, 1.0, time)
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(scale, scale))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del zoom inverso. |
| **delay** | `0.05` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[zoom_out speed=2.0 delay=0.05]Texto de ejemplo[/zoom_out]`

---

## 36. Roll In (Entrada con Rotación)

### 🖼️ Muestra:
![36](https://github.com/user-attachments/assets/1c30e3be-c638-4a3a-9fb7-a49c2d700a92)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRollIn extends RichTextEffect

var bbcode = "roll"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var char_delay = char_fx.env.get("delay", 0.1)
	
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * char_delay), 0.0, 1.0)
	var inv = 1.0 - time
	var angle = deg_to_rad(inv * -120.0)
	
	char_fx.transform.x = Vector2(cos(angle), sin(angle))
	char_fx.transform.y = Vector2(-sin(angle), cos(angle))
	char_fx.offset.x = inv * -50.0
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de rotación. |
| **delay** | `0.1` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[roll speed=2.0 delay=0.1]Texto de ejemplo[/roll]`

---

## 37. Cyber Glitch (Entrada con Vibración)

### 🖼️ Muestra:
![37](https://github.com/user-attachments/assets/506e1797-5176-44af-8bee-ba3a25ba48c8)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextCyberGlitch extends RichTextEffect

var bbcode = "cyber"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.5)
	var char_delay = char_fx.env.get("delay", 0.1)
	
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * char_delay), 0.0, 1.0)
	
	if time < 1.0 and time > 0.0:
		char_fx.offset.x += randf_range(-2.0, 2.0)
		char_fx.offset.y += randf_range(-2.0, 2.0)
	
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.5` | Velocidad del glitch inicial. |
| **delay** | `0.1` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[cyber speed=2.5 delay=0.1]Texto de ejemplo[/cyber]`

---

## 38. Random Reveal (Aparición Desordenada)

### 🖼️ Muestra:
![38](https://github.com/user-attachments/assets/93c71c1d-d286-4a0d-9bee-0ac7b2f4abc9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRandomReveal extends RichTextEffect

var bbcode = "r_reveal"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.5)
	
	var reveal_seed = fmod(abs(sin(char_fx.relative_index * 91.34) * 43758.54), 1.0)
	var threshold = char_fx.elapsed_time * speed
	
	char_fx.color.a = 1.0 if threshold > reveal_seed else 0.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.5` | Velocidad de revelado aleatorio. |

> **Ejemplo de uso:**
> `[r_reveal speed=1.5]Texto de ejemplo[/r_reveal]`

---

## 39. Binary Decode (Decodificador Binario)

### 🖼️ Muestra:
![39](https://github.com/user-attachments/assets/a198a061-d7f2-4415-8d5c-2e11a1fb41e9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextBinaryDecode extends RichTextEffect

var bbcode = "binary"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.0)
	var f_size = char_fx.env.get("f_size", 16)
	
	var start_time = char_fx.relative_index * 0.1
	var end_time = start_time + 0.8 
	var time = char_fx.elapsed_time * speed
	
	if time < start_time:
		char_fx.color.a = 0
	elif time < end_time:
		var ts = TextServerManager.get_primary_interface()
		var binary_char = "1" if randf() > 0.5 else "0"
		char_fx.glyph_index = ts.font_get_glyph_index(char_fx.font, f_size, binary_char.unicode_at(0), 0)
		char_fx.color = Color(0.0, 1.0, 0.0, 1.0) 
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.0` | Velocidad del proceso de decodificación. |
| **f_size** | `16` | Tamaño de fuente para el cálculo del glifo binario. |

> **Ejemplo de uso:**
> `[binary speed=1.0 f_size=16]Texto de ejemplo[/binary]`

---

## 40. Fall Down (Caída desde arriba)

### 🖼️ Muestra:
![40](https://github.com/user-attachments/assets/72935a67-8e75-41b3-9361-45d18fafac7e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFallDown extends RichTextEffect

var bbcode = "fall"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.1), 0.0, 1.0)
	
	char_fx.offset.y = lerp(-50.0, 0.0, time)
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de caída. |

> **Ejemplo de uso:**
> `[fall speed=2.0]Texto de ejemplo[/fall]`

---

## 41. Rise Up (Subida desde abajo)

### 🖼️ Muestra:
![41](https://github.com/user-attachments/assets/34d0ff55-7c0b-49ce-96f8-08df48a0836c)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRiseUp extends RichTextEffect
var bbcode = "rise"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.1), 0.0, 1.0)
	char_fx.offset.y = lerp(50.0, 0.0, time)
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la subida. |

> **Ejemplo de uso:**
> `[rise speed=2.0]Texto de ejemplo[/rise]`

---

## 42. Pop In (Entrada con Rebote)

### 🖼️ Muestra:
![42](https://github.com/user-attachments/assets/740c3ecd-c9a6-4963-bbc0-a890d45ee618)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextPopIn extends RichTextEffect
var bbcode = "pop_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var s = 0.0
	if time < 0.8: s = lerp(0.5, 1.2, time / 0.8)
	else: s = lerp(1.2, 1.0, (time - 0.8) / 0.2)
	char_fx.transform = char_fx.transform.scaled_local(Vector2(s, s))
	char_fx.color.a = clamp(time * 2.0, 0.0, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del efecto de rebote al entrar. |

> **Ejemplo de uso:**
> `[pop_in speed=3.0]Texto de ejemplo[/pop_in]`

---

## 43. Pop Loop (Efecto Burbuja Constante)

### 🖼️ Muestra:
![43](https://github.com/user-attachments/assets/e6f3846d-2552-4f13-b66b-a706175acd2e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextPopLoop extends RichTextEffect
var bbcode = "pop_loop"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 4.0)
	var pulse = sin(char_fx.elapsed_time * speed + char_fx.relative_index * 0.5) * 0.2
	var s = 1.0 + pulse
	char_fx.transform = char_fx.transform.scaled_local(Vector2(s, s))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `4.0` | Velocidad de la oscilación de la burbuja. |

> **Ejemplo de uso:**
> `[pop_loop speed=4.0]Texto de ejemplo[/pop_loop]`

---

## 44. Slit Vertical (Aparición de Corte V)

### 🖼️ Muestra:
![44](https://github.com/user-attachments/assets/41ba2f19-c208-47cc-8275-86b57a2c99fb)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlitV extends RichTextEffect
var bbcode = "slit_v"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0, time))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de apertura vertical. |

> **Ejemplo de uso:**
> `[slit_v speed=2.0]Texto de ejemplo[/slit_v]`

---

## 45. Slit Horizontal (Aparición de Corte H)

### 🖼️ Muestra:
![45](https://github.com/user-attachments/assets/2a532ea2-b2f1-4415-b272-8afa222ba19e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSlitH extends RichTextEffect
var bbcode = "slit_h"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	char_fx.transform = char_fx.transform.scaled_local(Vector2(time, 1.0))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de apertura horizontal. |

> **Ejemplo de uso:**
> `[slit_h speed=2.0]Texto de ejemplo[/slit_h]`

---

## 46. Roll Top (Rodando desde arriba)

### 🖼️ Muestra:
![46](https://github.com/user-attachments/assets/b50cabbf-9c90-4971-bef0-70cf984b3f25)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRollTop extends RichTextEffect
var bbcode = "roll_top"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var inv = 1.0 - time
	var angle = deg_to_rad(inv * -120.0)
	char_fx.transform.x = Vector2(cos(angle), sin(angle))
	char_fx.transform.y = Vector2(-sin(angle), cos(angle))
	char_fx.offset.y = inv * -50.0
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la rotación desde arriba. |

> **Ejemplo de uso:**
> `[roll_top speed=2.0]Texto de ejemplo[/roll_top]`

---

## 47. Roll Bottom (Rodando desde abajo)

### 🖼️ Muestra:
![47](https://github.com/user-attachments/assets/15d630a9-2a6b-489c-a0b2-be8a0f27853c)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRollBottom extends RichTextEffect
var bbcode = "roll_bottom"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var inv = 1.0 - time
	var angle = deg_to_rad(inv * 120.0)
	char_fx.transform.x = Vector2(cos(angle), sin(angle))
	char_fx.transform.y = Vector2(-sin(angle), cos(angle))
	char_fx.offset.y = inv * 50.0
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la rotación desde abajo. |

> **Ejemplo de uso:**
> `[roll_bottom speed=2.0]Texto de ejemplo[/roll_bottom]`

---

## 48. Bounce Left (Rebote desde Izquierda)

### 🖼️ Muestra:
![48](https://github.com/user-attachments/assets/de356825-24fa-4746-902a-9f64b87e7e83)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextBounceLeft extends RichTextEffect
var bbcode = "bounce_l"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var t = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var x = 0.0
	if t < 0.6: x = lerp(-50.0, 10.0, t / 0.6)
	elif t < 0.8: x = lerp(10.0, -5.0, (t - 0.6) / 0.2)
	else: x = lerp(-5.0, 0.0, (t - 0.8) / 0.2)
	char_fx.offset.x = x
	char_fx.color.a = clamp(t * 2.0, 0.0, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del rebote desde la izquierda. |

> **Ejemplo de uso:**
> `[bounce_l speed=2.0]Texto de ejemplo[/bounce_l]`

---

## 49. Bounce Right (Rebote desde Derecha)

### 🖼️ Muestra:
![49](https://github.com/user-attachments/assets/ba81fd7e-7be3-42fb-ba4c-c4f1fec0a692)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextBounceRight extends RichTextEffect
var bbcode = "bounce_r"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var t = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var x = 0.0
	if t < 0.6: x = lerp(50.0, -10.0, t / 0.6)
	elif t < 0.8: x = lerp(-10.0, 5.0, (t - 0.6) / 0.2)
	else: x = lerp(5.0, 0.0, (t - 0.8) / 0.2)
	char_fx.offset.x = x
	char_fx.color.a = clamp(t * 2.0, 0.0, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del rebote desde la derecha. |

> **Ejemplo de uso:**
> `[bounce_r speed=2.0]Texto de ejemplo[/bounce_r]`

---

## 50. Rotate Y (Eje Vertical / 3D Falso)

### 🖼️ Muestra:
![50](https://github.com/user-attachments/assets/11e002b9-c30d-4553-84e7-1c1a946173ed)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRotY extends RichTextEffect
var bbcode = "rot_y"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var s_x = cos(lerp(PI/2.0, 0.0, time))
	char_fx.transform = char_fx.transform.scaled_local(Vector2(s_x, 1.0))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de rotación en el eje Y. |

> **Ejemplo de uso:**
> `[rot_y speed=2.0]Texto de ejemplo[/rot_y]`

---

## 51. Rotate X (Eje Horizontal / 3D Falso)

### 🖼️ Muestra:
![51](https://github.com/user-attachments/assets/73ea0861-bd24-4bac-b735-0623f399ea0c)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRotX extends RichTextEffect
var bbcode = "rot_x"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var s_y = cos(lerp(PI/2.0, 0.0, time))
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0, s_y))
	char_fx.color.a = time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de rotación en el eje X. |

> **Ejemplo de uso:**
> `[rot_x speed=2.0]Texto de ejemplo[/rot_x]`
```
