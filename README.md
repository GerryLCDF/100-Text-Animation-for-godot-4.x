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
	# Basado en cuánto tiempo ha pasado desde que "le tocaba" empezar
	var alpha = (char_fx.elapsed_time - my_start_time) * speed
	
	# Limitamos el valor entre 0 (invisible) y 1 (opaco)
	char_fx.color.a = clamp(alpha, 0.0, 1.0)
	
	return true
```

### 📊 Parámetros de Configuración
Puedes ajustar el comportamiento del efecto directamente desde el BBCode:

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Un valor más bajo hace que el fade sea más lento. |
| **delay** | `0.1` | Tiempo de espera entre la aparición de cada letra. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar a mostrar la primera letra. |

> **Ejemplo de uso:** > `[fade_in speed=1.0 delay=0.05 wait=0.2]Texto que aparece suavemente[/fade_in]`

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
Puedes ajustar el comportamiento del efecto directamente desde el BBCode:

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `15.0` | Cantidad de letras que se muestran por segundo. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar a escribir la primera letra. |


> **Ejemplo de uso:** > `[fade_in speed=1.0 delay=0.05 wait=0.2]Texto que aparece suavemente[/fade_in]`

---

