# 100-Text-Animation-for-godot-4.x
Una recopilación de animaciones personalizadas para utilizar en nodos `RichTextLabel`.

---

## 1. Fade In (Desvanecimiento)
Este efecto permite que los caracteres aparezcan progresivamente, ideal para diálogos o introducciones con estilo.




```gdscript
@tool
class_name RichTextFadeIn
extends RichTextEffect

# Identificador de uso: [fade_in]
var bbcode = "fade_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	# Parámetros extraídos desde el BBCode
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.1)
	var start_wait = char_fx.env.get("wait", 0.0)
	
	# Calculamos el tiempo de inicio para este carácter específico (basado en su índice)
	var my_start_time = start_wait + (char_fx.relative_index * delay)
	
	# Calculamos el progreso del canal alfa (de 0.0 a 1.0)
	var alpha = (char_fx.elapsed_time - my_start_time) * speed
	
	# Aplicamos el valor final limitándolo entre 0 (invisible) y 1 (opaco)
	char_fx.color.a = clamp(alpha, 0.0, 1.0)
	
	return true
```

### 📊 Parámetros de Configuración
Puedes ajustar el comportamiento del efecto directamente desde el BBCode:

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Controla la suavidad. Un valor más bajo hace que el fade sea más lento. |
| **delay** | `0.1` | El "escalonamiento". Tiempo de espera entre la aparición de cada letra. |
| **wait** | `0.0` | Segundos que esperará el texto antes de empezar a mostrar la primera letra. |

> **Ejemplo de uso:** > `[fade_in speed=1.0 delay=0.05 wait=0.2]Texto que aparece suavemente[/fade_in]`


### 🛠️ Implementación del Código
Para usar este efecto, asegúrate de tener el siguiente script en tu proyecto:

[![Ver Archivo](https://img.shields.io/badge/Ver_Código-GitHub-blue?style=for-the-badge&logo=github)](./RichTextFadeIn.gd)
[![Descargar Script](https://img.shields.io/badge/Descargar-Script-green?style=for-the-badge&logo=godotengine)](./RichTextFadeIn.gd)

---
