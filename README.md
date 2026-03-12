# +101 Text Animation for Godot 4.x
A collection of custom animations for use in `RichTextLabel` nodes.

### How to use these resources
Remember that this is a free resource. If you decide to use it in any of your projects, credits would be appreciated, although you are entirely free not to do so.

### Video Demonstration
https://youtu.be/t_KPnAE53gI
---

## 1. Fade In (Smooth Appearance)

### 🖼️ Preview:
![1](https://github.com/user-attachments/assets/89ea952a-54c7-4d28-80e6-6ee40fb850cd)

### 💻 Code:
```gdscript
@tool
class_name RichTextFadeIn
extends RichTextEffect

# usage example [fade_in speed=1.0 delay=0.05 wait=0.0]Appearing text[/fade_in]
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the smooth fade-in appearance. |
| **delay** | `0.1` | Wait time between each letter. |
| **wait** | `0.0` | Global wait seconds before starting. |

> **Usage example:**
> `[fade_in speed=1.0 delay=0.05 wait=0.2]Example text.[/fade_in]`

---

## 2. Typewriter

### 🖼️ Preview:
![2](https://github.com/user-attachments/assets/1ed074f3-63ca-41ee-9477-ddff1e8b3b09)

### 💻 Code:
```gdscript
@tool
class_name RichTextTypewriter
extends RichTextEffect

# usage example [type speed=20.0 wait=0.0]This text writes itself...[/type]
var bbcode = "type"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 15.0)
	var wait = char_fx.env.get("wait", 0.0)
	
	var time = char_fx.elapsed_time - wait
	var finish_time = char_fx.relative_index / speed
	
	char_fx.color.a = 0.0 if time < finish_time else 1.0
	return true
```

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `15.0` | Amount of letters shown per second. |
| **wait** | `0.0` | Seconds the text will wait before starting. |

> **Usage example:**
> `[type speed=20.0 wait=0.5]Example text.[/type]`

---

## 3. Decode (Symbol Deciphering)

### 🖼️ Preview:
![3](https://github.com/user-attachments/assets/6dd7506e-947e-42ba-a4a5-8c13a443fa74)

### 💻 Code:
```gdscript
@tool
class_name RichTextDecode
extends RichTextEffect

# usage example [decode speed=10.0 wait=0.5]Example text[/decode]
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `5.0` | Character reveal speed. |
| **wait** | `0.0` | Delay before starting the decryption. |

> **Usage example:**
> `[decode speed=10.0 wait=0.5]Example text.[/decode]`

---

## 4. Slide Up

### 🖼️ Preview:
![4](https://github.com/user-attachments/assets/0d368e15-8347-4d72-a3b4-9768a754aa10)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the transition. |
| **delay** | `0.05` | Time between each letter. |
| **dist** | `20.0` | Distance from which the text slides up. |

> **Usage example:**
> `[slide_up speed=2.0 delay=0.05 dist=20.0]Example text.[/slide_up]`

---

## 5. Bounce (Elastic Bounce)

### 🖼️ Preview:
![5](https://github.com/user-attachments/assets/0dd4c149-950b-4e4b-bd3c-bb59b59e71d9)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the bounce. |
| **delay** | `0.08` | Wait time between letters. |
| **dist** | `50.0` | Maximum height of the initial jump. |

> **Usage example:**
> `[bounce speed=2.0 delay=0.08 dist=50.0]Example text.[/bounce]`

---

## 6. Rotate In (Scale Rotation)

### 🖼️ Preview:
![6](https://github.com/user-attachments/assets/7d39607b-bec0-44df-a44c-2b8e72920183)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the rotation and scale growth. |
| **delay** | `0.05` | Stagger time between characters. |

> **Usage example:**
> `[rotate_in speed=2.0 delay=0.05]Example text.[/rotate_in]`

---

## 7. Slide Down

### 🖼️ Preview:
![7](https://github.com/user-attachments/assets/4ca6df90-957e-4f2c-a449-0b931f9d4eba)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the descent. |
| **delay** | `0.05` | Time between each letter. |
| **dist** | `30.0` | Distance from which the text slides down. |

> **Usage example:**
> `[slide_down speed=2.0 delay=0.05 dist=30.0]Example text.[/slide_down]`

---

## 8. Slide Left (Slide from the Left)

### 🖼️ Preview:
![8](https://github.com/user-attachments/assets/bec1595f-d6c9-41d3-9ca8-b414c7a3002e)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the slide. |
| **delay** | `0.05` | Stagger time between characters. |
| **dist** | `40.0` | Horizontal distance of the effect. |

> **Usage example:**
> `[slide_left speed=2.0 delay=0.05 dist=40.0]Example text.[/slide_left]`

---

## 9. Slide Right (Slide from the Right)

### 🖼️ Preview:
![9](https://github.com/user-attachments/assets/2fdd0dfd-c39a-4fb9-b132-edd6188e4640)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the slide. |
| **delay** | `0.05` | Stagger time between characters. |
| **dist** | `40.0` | Horizontal distance of the effect. |

> **Usage example:**
> `[slide_right speed=2.0 delay=0.05 dist=40.0]Example text.[/slide_right]`

---

## 10. Drop In (Heavy Bounce Drop)

### 🖼️ Preview:
![10](https://github.com/user-attachments/assets/26398767-e164-48d1-95f2-914898a5d6b0)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `1.5` | Speed of the fall and bounce. |
| **delay** | `0.06` | Time between each letter. |
| **dist** | `100.0` | Height from which it drops. |

> **Usage example:**
> `[drop_in speed=1.5 delay=0.06 dist=100.0]Example text.[/drop_in]`

---

## 11. Swing (Pendulum Swing)

An entry effect where characters swing like a pendulum until they settle.

### 🖼️ Preview:
![11](https://github.com/user-attachments/assets/d5ae0b83-e516-4868-86fb-5005c6a55692)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `1.2` | Swing speed. |
| **delay** | `0.05` | Stagger delay between letters. |
| **angle** | `15.0` | Initial maximum swing angle. |

> **Usage example:**
> `[swing speed=1.2 delay=0.05 angle=15.0]Example text.[/swing]`

---

## 12. Pulse In (Entry Pulse)

Letters appear with a scaling pulse effect that settles into their final size.

### 🖼️ Preview:
![12](https://github.com/user-attachments/assets/c9aa3d93-2eaa-456f-93a6-a231411e1bbc)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Pulse speed. |
| **delay** | `0.05` | Time delay between letters. |
| **scale** | `1.2` | Maximum pulse scale. |

> **Usage example:**
> `[pulse speed=2.0 delay=0.05 scale=1.2]Example text.[/pulse]`

---

## 13. Scale In (Appearance with Scale)

Characters grow from zero to their original size while fading in.

### 🖼️ Preview:
![13](https://github.com/user-attachments/assets/bc057bb2-c77e-42b1-9ac4-f0e8f708bd21)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `4.0` | Speed at which characters scale from zero. |
| **delay** | `0.05` | Wait time between each letter's scaling. |
| **wait** | `0.0` | Seconds the text waits before starting. |

> **Usage example:**
> `[scale_in speed=4.0 delay=0.05 wait=0.0]Example text[/scale_in]`

---

## 14. Flash (Flashing Entry)

An entry effect where characters flash on and off before remaining fully visible.

### 🖼️ Preview:
![14](https://github.com/user-attachments/assets/0fc76db2-a538-4991-8e53-a90fc7df670b)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Initial flash frequency. |
| **delay** | `0.05` | Stagger delay between characters. |

> **Usage example:**
> `[flash speed=2.0 delay=0.05]Example text[/flash]`

---

## 15. Shake X (Horizontal Shake Entry)

Characters appear while shaking horizontally, with the intensity fading out over time.

### 🖼️ Preview:
![15](https://github.com/user-attachments/assets/7f0f8723-ca12-4d10-a851-de676349cbcb)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Shake frequency. |
| **strength** | `10.0` | Horizontal shake intensity. |

> **Usage example:**
> `[shake_x speed=3.0 strength=10.0]Example text[/shake_x]`

---

## 16. Shake Y (Vertical Shake Entry)

Characters appear while shaking vertically, with the intensity fading out over time.

### 🖼️ Preview:
![16](https://github.com/user-attachments/assets/ed6eea12-c64c-4994-8b82-fd569590c838)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Shake frequency. |
| **strength** | `10.0` | Vertical shake intensity. |

> **Usage example:**
> `[shake_y speed=3.0 strength=10.0]Example text[/shake_y]`

---

## 17. Tada (Celebration Effect)

A celebratory entry effect that combines rapid scaling and rotating pulses.

### 🖼️ Preview:
![17](https://github.com/user-attachments/assets/9787470e-9995-4490-b196-124c045d0031)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the full animation. |
| **delay** | `0.05` | Stagger delay between characters. |

> **Usage example:**
> `[tada speed=2.0 delay=0.05]Example text[/tada]`

---

## 18. Reveal (Lateral Sweep)

Characters are revealed through a lateral sweep effect with a slight horizontal slide.

### 🖼️ Preview:
![18](https://github.com/user-attachments/assets/4c73261a-7873-4525-b69a-e53249c23396)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Sweep speed. |
| **delay** | `0.05` | Time delay between letters. |
| **wait** | `0.0` | Initial wait time. |

> **Usage example:**
> `[reveal speed=2.0 delay=0.05 wait=0.0]Example text[/reveal]`

---

## 19. Wave Jump (Wave Bouncing)

Characters jump following a continuous wave pattern.

### 🖼️ Preview:
![19](https://github.com/user-attachments/assets/ae09d903-b82e-4d19-bb21-cb6486f787a1)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Jump speed. |
| **height** | `10.0` | Maximum jump height. |
| **delay** | `0.1` | Wave curvature/offset. |

> **Usage example:**
> `[wave_jump speed=3.0 height=10.0 delay=0.1]Example text[/wave_jump]`

---

## 20. Wave Rotate (Wavy Rotation)

Characters rotate back and forth following a continuous wave pattern.

### 🖼️ Preview:
![20](https://github.com/user-attachments/assets/e4b47e2f-3f15-47af-aad3-ed9de12a2c43)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Swing speed. |
| **angle** | `45.0` | Maximum rotation angle. |
| **delay** | `0.2` | Delay between characters. |

> **Usage example:**
> `[wave_rot speed=3.0 angle=45.0 delay=0.2]Example text[/wave_rot]`

---

## 21. Roller 3D (X-Axis Rotation)

An animation that simulates a 3D rotation of the letters along the X-axis, including shading effects.

### 🖼️ Preview:
![21](https://github.com/user-attachments/assets/6e36c6cc-a529-4465-bc2e-523abb7a2942)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Rotation speed. |
| **height** | `1.0` | Vertical scale multiplier. |
| **delay** | `0.1` | Time offset between letters. |

> **Usage Example:**
> `[roller speed=3.0 height=1.0 delay=0.1]Example text[/roller]`

---

## 22. Slide Swap (Infinite Vertical Swap)

Letters slide vertically in a continuous loop, fading in and out at the edges.

### 🖼️ Preview:
![22](https://github.com/user-attachments/assets/23d11673-d097-4809-b036-f648e2f4bb8b)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the swap cycle. |
| **dist** | `30.0` | Vertical displacement distance. |
| **delay** | `0.1` | Wait time between characters. |

> **Usage Example:**
> `[slide_swap speed=2.0 dist=30.0 delay=0.1]Example text[/slide_swap]`

---

## 23. Jello Loop (Jelly Vibration)

A continuous squash and stretch loop that gives the text a gelatinous look.

### 🖼️ Preview:
![23](https://github.com/user-attachments/assets/72a32d25-2a53-44c4-82fb-9e6dbf596260)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the jelly oscillation. |
| **delay** | `0.1` | Vibration offset between letters. |
| **strength** | `0.2` | Deformation intensity (Squash & Stretch). |

> **Usage Example:**
> `[jello_loop speed=2.0 delay=0.1 strength=0.2]Example text[/jello_loop]`

---

## 24. Rubber (Entry Squash and Stretch)

An entry effect where each letter appears with an elastic squash and stretch bounce.

### 🖼️ Preview:
![24](https://github.com/user-attachments/assets/a712e073-fa46-4d7b-a3a3-9bbcb9f8ab2b)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Elastic bounce speed. |
| **delay** | `0.05` | Delay between each letter's appearance. |

> **Usage Example:**
> `[rubber speed=2.0 delay=0.05]Example text[/rubber]`

---

## 25. Wave Ocean (Single Wave)

A single wave pass effect that moves across the text from left to right.

### 🖼️ Preview:
![25](https://github.com/user-attachments/assets/f3534b09-b810-496c-9bce-fab52a48b28f)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Speed of the wave pass. |
| **delay** | `0.08` | Offset used to create the curve effect. |
| **dist** | `15.0` | Maximum height of the wave. |

> **Usage Example:**
> `[wave_ocean speed=3.0 delay=0.08 dist=15.0]Example text[/wave_ocean]`

---

## 26. Stretch (Horizontal Stretch)

Each character stretches horizontally in a wave-like pattern.

### 🖼️ Preview:
![26](https://github.com/user-attachments/assets/de1beb87-8542-439f-b2d2-d3c7446311a9)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `4.0` | Stretching speed. |
| **max_stretch** | `1.5` | Maximum horizontal scale (1.5 = 150%). |
| **delay** | `0.05` | Delay between each letter. |

> **Usage Example:**
> `[stretch speed=4.0 delay=0.05 max_stretch=1.5]Example text[/stretch]`

---

## 27. Alpha Scroll (Opacity Reveal)

A smooth reveal effect based on a progressive opacity change from left to right.

### 🖼️ Preview:
![27](https://github.com/user-attachments/assets/a2c148c9-f592-4af3-ae9b-6ba495b7aa59)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `10.0` | Reveal speed. |
| **delay** | `0.1` | Time between each letter appearing. |

> **Usage Example:**
> `[alpha_scroll speed=10.0 delay=0.1]Example text[/alpha_scroll]`

---

## 28. Glitch Pop (Random Glow Substitution)

Characters are occasionally replaced by random glitch symbols with a bright HDR glow.

### 🖼️ Preview:
![28](https://github.com/user-attachments/assets/f46b66a4-5242-4bfe-9cd4-95437ca19ffe)

### 💻 Code:
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
			char_fx.color = Color(10.0, 10.0, 10.0, 1.0) # HDR Glow
	return true
```

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.5` | Frequency of the glitches. |
| **f_size** | `16` | Font size used for glyph calculation. |

> **Usage Example:**
> `[glitch_pop speed=2.5 f_size=16]Example text[/glitch_pop]`

---

## 29. Caesar (Cipher with Reveal Pop)

Text appears encrypted in cyan and deciphers itself letter by letter with a scale "pop" effect.

### 🖼️ Preview:
![29](https://github.com/user-attachments/assets/65661f2b-3e16-48d6-9c8d-c07e49e7b9bc)

### 💻 Code:
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
		char_fx.color.v += pop_curve # Increase brightness
		
	if should_encrypt:
		var ts = TextServerManager.get_primary_interface()
		if char_fx.glyph_index != ts.font_get_glyph_index(char_fx.font, 16, 32, 0): # No spaces
			char_fx.glyph_index += shift
			char_fx.color = Color(0.15, 0.81, 0.82, 1.0) # Encrypted cyan color
	return true
```

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **delay** | `2.0` | Seconds before the deciphering starts. |
| **reveal_speed**| `5.0` | Speed at which letters are revealed. |
| **shift** | `3` | Cipher character shift offset. |
| **pop_scale** | `1.4` | Scale of the "pop" effect upon reveal. |

> **Usage Example:**
> `[caesar delay=2.0 reveal_speed=5.0 shift=3]Encrypted Text[/caesar]`

---

## 30. Nervous (Erratic Agitation)

The text shakes erratically in all directions, perfect for representing nervous or scared characters.

### 🖼️ Preview:
![30](https://github.com/user-attachments/assets/956efa5a-a9fd-4e7f-933d-f73b52a98c6c)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `20.0` | Agitation speed. |
| **strength** | `3.0` | Maximum shake distance. |

> **Usage Example:**
> `[nervous speed=20.0 strength=3.0]Nervous or scared text[/nervous]`

---

## 31. Squeeze In (Compression Reveal)

Characters appear by scaling up vertically from a compressed state while fading in.

### 🖼️ Preview:
![31](https://github.com/user-attachments/assets/fa9cdc2b-53f5-4322-9fd2-e17328d36a0e)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Animation speed. |
| **delay** | `0.1` | Stagger delay between each letter. |

> **Usage Example:**
> `[squeeze speed=2.0 delay=0.1]Example text[/squeeze]`

---

## 32. Color Shift Intro (Rainbow Reveal)

Characters are revealed through a multi-stage color transition, passing through several hues before reaching the final color.

### 🖼️ Preview:
![32](https://github.com/user-attachments/assets/0456bbb5-6750-48af-b403-c0b48372a6d4)

### 💻 Code:
```gdscript
@tool
class_name RichTextColorShiftIntro
extends RichTextEffect

var bbcode = "c_shift_in"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.0)
	var char_delay = char_fx.env.get("delay", 0.05)
	
	var c1 = Color(char_fx.env.get("c1", "#ffffff")) # Initial White
	var c2 = Color(char_fx.env.get("c2", "#ff0055")) # First shift
	var c3 = Color(char_fx.env.get("c3", "#00ccff")) # Second shift
	var c_end = Color(char_fx.env.get("c_end", "#ffffff")) # Final color
	
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `1.0` | Speed of the color transition. |
| **delay** | `0.05` | Chromatic offset between letters. |
| **c1** | `#ffffff` | Initial letter color. |
| **c2** | `#ff0055` | First introduction color. |
| **c3** | `#00ccff` | Second introduction color. |
| **c_end** | `#ffffff` | The final color the text will have. |

> **Usage Example:**
> `[c_shift_in c1=#05614b c2=#020e0e c3=#01de82 c_end=#ffffff speed=1.5]Example text[/c_shift_in]`

---

## 33. Color Shift Loop (Constant Color Loop)

A continuous color cycle that travels across the text, transitioning smoothly between three defined colors.

### 🖼️ Preview:
![33](https://github.com/user-attachments/assets/dc9fd745-bb2b-4a0e-ab11-442240cace3b)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Speed of the color cycle. |
| **freq** | `0.5` | Color distance between adjacent letters. |
| **c1** | `#ffffff` | First loop color. |
| **c2** | `#ff0055` | Second loop color. |
| **c3** | `#00ccff` | Third loop color. |

> **Usage Example:**
> `[c_shift_loop c1=#ff0000 c2=#ffaa00 c3=#ffff00 freq=0.1]HOT TEXT[/c_shift_loop]`

---

## 34. Zoom In Fast (Reveal from 0)

Characters pop in quickly by scaling up from zero to their original size.

### 🖼️ Preview:
![34](https://github.com/user-attachments/assets/06051c89-8cd9-4912-8f86-18d5066210ec)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Zoom speed. |
| **delay** | `0.05` | Wait time between each letter. |

> **Usage Example:**
> `[zoom_in speed=3.0 delay=0.05]Example text[/zoom_in]`

---

## 35. Zoom Out Slow (Reveal from 2.0)

Characters appear by scaling down from a large size (2x) to their original size.

### 🖼️ Preview:
![35](https://github.com/user-attachments/assets/c0728403-8da7-47dd-8580-25e9c3ec6ef7)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Inverse zoom speed. |
| **delay** | `0.05` | Wait time between each letter. |

> **Usage Example:**
> `[zoom_out speed=2.0 delay=0.05]Example text[/zoom_out]`

---

## 36. Roll In (Entry with Rotation)

Letters roll into their position from the left while spinning.

### 🖼️ Preview:
![36](https://github.com/user-attachments/assets/1c30e3be-c638-4a3a-9fb7-a49c2d700a92)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rotation speed. |
| **delay** | `0.1` | Wait time between each letter. |

> **Usage Example:**
> `[roll speed=2.0 delay=0.1]Example text[/roll]`

---

## 37. Cyber Glitch (Entry with Vibration)

Characters appear while vibrating erratically, creating a digital "glitch" aesthetic.

### 🖼️ Preview:
![37](https://github.com/user-attachments/assets/506e1797-5176-44af-8bee-ba3a25ba48c8)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.5` | Initial glitch speed. |
| **delay** | `0.1` | Wait time between each letter. |

> **Usage Example:**
> `[cyber speed=2.5 delay=0.1]Example text[/cyber]`

---

## 38. Random Reveal (Disordered Appearance)

Characters appear in a completely random order across the text.

### 🖼️ Preview:
![38](https://github.com/user-attachments/assets/93c71c1d-d286-4a0d-9bee-0ac7b2f4abc9)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `1.5` | Random reveal speed. |

> **Usage Example:**
> `[r_reveal speed=1.5]Example text[/r_reveal]`

---

## 39. Binary Decode (Binary Decoder)

Characters are first displayed as random "1" and "0" binary digits in green before revealing the actual text.

### 🖼️ Preview:
![39](https://github.com/user-attachments/assets/a198a061-d7f2-4415-8d5c-2e11a1fb41e9)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `1.0` | Speed of the decoding process. |
| **f_size** | `16` | Font size used for binary glyph calculation. |

> **Usage Example:**
> `[binary speed=1.0 f_size=16]Example text[/binary]`

---

## 40. Fall Down (Falling Reveal)

Characters fall into position from a vertical offset above.

### 🖼️ Preview:
![40](https://github.com/user-attachments/assets/72935a67-8e75-41b3-9361-45d18fafac7e)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Falling speed. |

> **Usage Example:**
> `[fall speed=2.0]Example text[/fall]`

---

## 41. Rise Up (Rising Reveal)

Characters rise into position from a vertical offset below.

### 🖼️ Preview:
![41](https://github.com/user-attachments/assets/34d0ff55-7c0b-49ce-96f8-08df48a0836c)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rising speed. |

> **Usage Example:**
> `[rise speed=2.0]Example text[/rise]`

---

## 42. Pop In (Bounce Entry)

Characters pop in with a quick bounce effect, briefly overshooting their original scale.

### 🖼️ Preview:
![42](https://github.com/user-attachments/assets/740c3ecd-c9a6-4963-bbc0-a890d45ee618)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `3.0` | Bounce entry speed. |

> **Usage Example:**
> `[pop_in speed=3.0]Example text[/pop_in]`

---

## 43. Pop Loop (Constant Bubble Effect)

A continuous, rhythmic scaling oscillation that gives the text a "bubbling" or breathing appearance.

### 🖼️ Preview:
![43](https://github.com/user-attachments/assets/e6f3846d-2552-4f13-b66b-a706175acd2e)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `4.0` | Bubble oscillation speed. |

> **Usage Example:**
> `[pop_loop speed=4.0]Example text[/pop_loop]`

---

## 44. Slit Vertical (Vertical Slit Reveal)

Characters are revealed through a vertical "opening" or slit effect.

### 🖼️ Preview:
![44](https://github.com/user-attachments/assets/41ba2f19-c208-47cc-8275-86b57a2c99fb)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Vertical opening speed. |

> **Usage Example:**
> `[slit_v speed=2.0]Example text[/slit_v]`

---

## 45. Slit Horizontal (Horizontal Slit Reveal)

Characters are revealed through a horizontal "opening" or slit effect.

### 🖼️ Preview:
![45](https://github.com/user-attachments/assets/2a532ea2-b2f1-4415-b272-8afa222ba19e)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Horizontal opening speed. |

> **Usage Example:**
> `[slit_h speed=2.0]Example text[/slit_h]`

---

## 46. Roll Top (Rolling from Above)

Letters roll into position from a vertical offset above, spinning forward.

### 🖼️ Preview:
![46](https://github.com/user-attachments/assets/b50cabbf-9c90-4971-bef0-70cf984b3f25)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rolling from above speed. |

> **Usage Example:**
> `[roll_top speed=2.0]Example text[/roll_top]`

---

## 47. Roll Bottom (Rolling from Below)

Letters roll into position from a vertical offset below, spinning backward.

### 🖼️ Preview:
![47](https://github.com/user-attachments/assets/15d630a9-2a6b-489c-a0b2-be8a0f27853c)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rolling from below speed. |

> **Usage Example:**
> `[roll_bottom speed=2.0]Example text[/roll_bottom]`

---

## 48. Bounce Left (Bounce from Left)

Letters bounce into position from an offset on the left.

### 🖼️ Preview:
![48](https://github.com/user-attachments/assets/de356825-24fa-4746-902a-9f64b87e7e83)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Bounce from left speed. |

> **Usage Example:**
> `[bounce_l speed=2.0]Example text[/bounce_l]`

---

## 49. Bounce Right (Bounce from Right)

Letters bounce into position from an offset on the right.

### 🖼️ Preview:
![49](https://github.com/user-attachments/assets/ba81fd7e-7be3-42fb-ba4c-c4f1fec0a692)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Bounce from right speed. |

> **Usage Example:**
> `[bounce_r speed=2.0]Example text[/bounce_r]`

---

## 50. Rotate Y (Vertical Axis / Pseudo-3D)

Simulates a 3D-style vertical rotation (around the Y-axis) as characters are revealed.

### 🖼️ Preview:
![50](https://github.com/user-attachments/assets/11e002b9-c30d-4553-84e7-1c1a946173ed)

### 💻 Code:
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

### 📊 Configuration Parameters

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **speed** | `2.0` | Rotation speed on the Y-axis. |

> **Usage Example:**
> `[rot_y speed=2.0]Example text[/rot_y]`
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
---

## 57. Fly Up (Vuelo hacia arriba)

Efecto de entrada donde las letras vuelan desde la parte inferior de la pantalla con un desvanecimiento suave.

### 🖼️ Muestra:
![57](https://github.com/user-attachments/assets/e809de8d-7da1-4022-adba-ee651cc4c9b0)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFlyUp
extends RichTextEffect

var bbcode = "fly_up"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = clamp(char_fx.elapsed_time * 1.5 - (char_fx.relative_index * 0.03), 0.0, 1.0)
	char_fx.offset.y = lerp(200.0, 0.0, t)
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[fly_up]Texto de ejemplo.[/fly_up]`

---

## 58. Wobble Jelly (Rebote de Gelatina)

Las letras entran con un movimiento elástico y vibrante, simulando la física de una gelatina.

### 🖼️ Muestra:
![58](https://github.com/user-attachments/assets/eb36f97a-6399-45a5-a22d-1dc1a348f82a)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWobbleJelly
extends RichTextEffect

var bbcode = "wobble_j"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = clamp(char_fx.elapsed_time - (char_fx.relative_index * 0.05), 0.0, 1.0)
	if t > 0.0 and t < 1.0:
		char_fx.offset.x = sin(t * 15.0) * 10.0 * (1.0 - t)
		char_fx.rotation = deg_to_rad(sin(t * 10.0) * 5.0 * (1.0 - t))
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[wobble_j]Texto de ejemplo.[/wobble_j]`

---

## 59. Glitch Out (Salida Glitch)

Efecto de desintegración digital. Las letras vibran aleatoriamente y cambian de color antes de desaparecer.

### 🖼️ Muestra:
![59](https://github.com/user-attachments/assets/4d20cd07-0151-451c-a7eb-9fe3231f1581)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextGlitchOut
extends RichTextEffect

var bbcode = "g_out"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.5)
	var delay = char_fx.env.get("delay", 0.1)
	
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * delay), 0.0, 1.0)
	
	if time > 0.0:
		char_fx.offset += Vector2(randf_range(-5.0, 5.0), randf_range(-5.0, 5.0)) * time
		var s = 1.0 - time
		char_fx.transform = char_fx.transform.scaled_local(Vector2(s, s))
		char_fx.color.a = 1.0 - time
		char_fx.color = char_fx.color.lerp(Color.RED if randf() > 0.5 else Color.CYAN, time)
	
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.5` | Velocidad de la desintegración. |
| **delay** | `0.1` | Tiempo de espera entre caracteres. |

> **Ejemplo de uso:**
| `[g_out speed=1.5 delay=0.1]Texto de ejemplo.[/g_out]`

---

## 60. Spotlight Sweep (Barrido de Foco)

Un haz de luz recorre el texto horizontalmente, iluminando las letras a su paso.

### 🖼️ Muestra:
![60](https://github.com/user-attachments/assets/faff693a-6c64-4a7a-917d-10d7d3987f4a)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSpotlight
extends RichTextEffect

var bbcode = "spotlight"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var cycle = fmod(char_fx.elapsed_time * 0.5, 2.0)
	var dist = abs(char_fx.relative_index * 0.1 - cycle)
	var lum = clamp(1.0 - dist * 2.0, 0.3, 1.0)
	char_fx.color *= lum
	if lum > 0.8: char_fx.color = char_fx.color.lerp(Color.WHITE, lum - 0.8)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[spotlight]Texto de ejemplo.[/spotlight]`

---

## 61. Terminal Type (Efecto Terminal)

Simula una consola antigua. Las letras aparecen una a una con un parpadeo de brillo al surgir.

### 🖼️ Muestra:
![61](https://github.com/user-attachments/assets/7b858d03-75f7-4345-879f-4060da80dfa9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextTerminal
extends RichTextEffect

var bbcode = "term"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 10.0)
	var time = char_fx.elapsed_time * speed
	char_fx.color.a = 1.0 if time > char_fx.relative_index else 0.0
	if int(time) == char_fx.relative_index:
		char_fx.color = Color.WHITE
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `10.0` | Cantidad de letras por segundo. |

> **Ejemplo de uso:**
> `[term speed=15.0]Texto de ejemplo.[/term]`

---

## 62. RGB Glitch Shift (Desfase RGB)

Entrada con distorsión cromática que separa los canales rojo y azul brevemente.

### 🖼️ Muestra:
![62](https://github.com/user-attachments/assets/dc9648d1-3cc0-4bab-b3d0-4259bcb93302)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextRGBGlitch
extends RichTextEffect

var bbcode = "rgb_g"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = clamp(char_fx.elapsed_time * 2.5 - (char_fx.relative_index * 0.1), 0.0, 1.0)
	if t < 1.0:
		var shift = sin(char_fx.elapsed_time * 20.0) * 2.0
		char_fx.offset.x += shift
		char_fx.color = Color(1, 0, 0, t) if shift > 0 else Color(0, 0, 1, t)
	else:
		char_fx.color.a = 1.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[rgb_g]Texto de ejemplo.[/rgb_g]`

---

## 63. Water Drop (Gota de Agua)

Las letras caen desde arriba y realizan un efecto de "squash and stretch" al impactar.

### 🖼️ Muestra:
![63](https://github.com/user-attachments/assets/1ae06051-1df7-449b-a324-987ad1b4f3d7)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWaterDrop
extends RichTextEffect

var bbcode = "water"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = clamp(char_fx.elapsed_time * 1.6 - (char_fx.relative_index * 0.05), 0.0, 1.0)
	char_fx.offset.y = lerp(-100.0, 0.0, t)
	var squash = 1.0
	if t > 0.5 and t < 0.8: squash = 1.5
	char_fx.transform = char_fx.transform.scaled_local(Vector2(squash, 2.0 - squash))
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[water]Texto de ejemplo.[/water]`

---

## 64. Falling Leaves (Hojas Cayendo)

Las letras caen suavemente con una rotación oscilatoria, imitando la caída de una hoja.

### 🖼️ Muestra:
![64](https://github.com/user-attachments/assets/b09669b2-e3c3-4ab5-86c6-c6b13a9211f6)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextLeaves
extends RichTextEffect

var bbcode = "leaves"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = (char_fx.elapsed_time - (char_fx.relative_index * 0.1)) * 0.6
	char_fx.offset.y = t * 100.0 - 50.0
	char_fx.offset.x = sin(t * 5.0) * 20.0
	char_fx.rotation = t * 2.0
	char_fx.color.a = clamp(1.0 - t, 0.0, 1.0) if t > 0.8 else clamp(t * 2.0, 0.0, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[leaves]Texto de ejemplo.[/leaves]`

---

## 65. Squeeze Expand (Estiramiento)

Entrada mediante un estiramiento vertical que se asienta en su tamaño original.

### 🖼️ Muestra:
![65](https://github.com/user-attachments/assets/08b982ff-cc6f-4b3e-a0fa-53a91db8dcde)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSqueezeExpand
extends RichTextEffect

var bbcode = "sq_ex"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = clamp(char_fx.elapsed_time * 1.2 - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var scale_y = lerp(0.1, 1.2, t) if t < 0.5 else lerp(1.2, 1.0, (t-0.5)*2.0)
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0, scale_y))
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[sq_ex]Texto de ejemplo.[/sq_ex]`

---

## 66. Sink Hole (Caída al Vacío)

Efecto de salida donde las letras caen pesadamente y rotan mientras desaparecen.

### 🖼️ Muestra:
![66](https://github.com/user-attachments/assets/bab15daf-5aed-4960-82db-cde1a06ce2f5)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSinkHole
extends RichTextEffect

var bbcode = "sink"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var delay = char_fx.env.get("delay", 0.05)
	var time = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * delay), 0.0, 1.0)
	if time > 0.0:
		char_fx.offset.y += pow(time * 10.0, 2.0)
		char_fx.rotation = deg_to_rad(time * 45.0)
		char_fx.color.a = 1.0 - time
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la caída. |
| **delay** | `0.05` | Retraso entre cada letra. |

> **Ejemplo de uso:**
> `[sink speed=2.0 delay=0.05]Texto de ejemplo.[/sink]`
---

## 67. Split Join (Unión Dividida)

Las letras entran desde los laterales de forma alterna (una por izquierda, otra por derecha) hasta unirse en el centro.

### 🖼️ Muestra:
![67](https://github.com/user-attachments/assets/f9566b08-8168-4a8e-bfc5-b72331ed29c9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSplitJoin
extends RichTextEffect

var bbcode = "split_join"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var t = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.05), 0.0, 1.0)
	var inv = 1.0 - t
	var side = 1.0 if char_fx.relative_index % 2 == 0 else -1.0
	char_fx.offset.x = side * (200.0 * inv)
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad con la que se unen las letras. |

> **Ejemplo de uso:**
> `[split_join speed=2.0]Texto de ejemplo.[/split_join]`

---

## 68. Wheel In Left (Rueda desde Izquierda)

Efecto de entrada donde cada letra aparece rodando desde el lado izquierdo hasta su posición final.

### 🖼️ Muestra:
![68](https://github.com/user-attachments/assets/2a7a59c7-fa4a-4146-8f30-613880444a46)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWheelL
extends RichTextEffect

var bbcode = "wheel_l"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var t = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.1), 0.0, 1.0)
	var inv = 1.0 - t
	var angle = inv * -PI * 2.0
	
	char_fx.transform.x = Vector2(cos(angle), sin(angle))
	char_fx.transform.y = Vector2(-sin(angle), cos(angle))
	char_fx.offset.x = inv * -100.0
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de rotación y traslación. |

> **Ejemplo de uso:**
> `[wheel_l speed=2.0]Texto de ejemplo.[/wheel_l]`

---

## 69. Wheel In Right (Rueda desde Derecha)

Efecto de entrada donde cada letra aparece rodando desde el lado derecho hasta su posición final.

### 🖼️ Muestra:
![69](https://github.com/user-attachments/assets/c292004a-bd42-4c0c-a24d-bc79f7eb0fa8)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextWheelR
extends RichTextEffect

var bbcode = "wheel_r"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var t = clamp(char_fx.elapsed_time * speed - (char_fx.relative_index * 0.1), 0.0, 1.0)
	var inv = 1.0 - t
	var angle = inv * PI * 2.0
	
	char_fx.transform.x = Vector2(cos(angle), sin(angle))
	char_fx.transform.y = Vector2(-sin(angle), cos(angle))
	char_fx.offset.x = inv * 100.0
	char_fx.color.a = t
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de rotación y traslación. |

> **Ejemplo de uso:**
> `[wheel_r speed=2.0]Texto de ejemplo.[/wheel_r]`

---

## 70. Sequential Flip (Giro en Cadena)

Las letras realizan un giro de 360 grados sobre su propio eje de forma secuencial.

### 🖼️ Muestra:
![70](https://github.com/user-attachments/assets/6a1f5ab4-2997-4131-9010-f837135c4048)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSequentialFlip
extends RichTextEffect

var bbcode = "seq_flip"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var total_time = char_fx.elapsed_time * speed
	var char_trigger_time = char_fx.relative_index * 0.5
	var local_t = clamp(fmod(total_time - char_trigger_time, 10.0), 0.0, 1.0) 
	
	if local_t > 0.0 and local_t < 1.0:
		var angle = local_t * PI * 2.0
		char_fx.transform.x = Vector2(cos(angle), sin(angle))
		char_fx.transform.y = Vector2(-sin(angle), cos(angle))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del giro. |

> **Ejemplo de uso:**
> `[seq_flip speed=3.0]Texto de ejemplo.[/seq_flip]`

---

## 71. Double Clash Sweep (Choque de Barrido)

Dos olas de luz viajan desde extremos opuestos y combinan su color al encontrarse en cada letra.

### 🖼️ Muestra:
![71](https://github.com/user-attachments/assets/d7739dbc-f547-484d-8d05-b477ed21fc0c)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextDoubleClash
extends RichTextEffect

var bbcode = "clash"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var delay = char_fx.env.get("delay", 0.15)
	var color_1 = char_fx.env.get("c1", Color.CYAN)
	var color_2 = char_fx.env.get("c2", Color.RED)
	
	var total_chars = char_fx.glyph_count
	var cycle_duration = (total_chars * delay) + 2.0
	var time = fmod(char_fx.elapsed_time, cycle_duration)
	
	var local_t1 = time - (char_fx.relative_index * delay)
	var local_t2 = time - ((total_chars - 1 - char_fx.relative_index) * delay)
	
	var i1 = exp(-pow((local_t1 * speed - 0.5) * 4.0, 2.0)) if local_t1 > 0.0 else 0.0
	var i2 = exp(-pow((local_t2 * speed - 0.5) * 4.0, 2.0)) if local_t2 > 0.0 else 0.0
	
	if i1 > 0.1 and i2 > 0.1:
		var clash = (i1 + i2) * 0.5
		char_fx.color = char_fx.color.lerp(Color.WHITE, clash).lerp(color_1.lerp(color_2, 0.5), clash * 0.8)
		char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0 + clash * 0.3, 1.0 + clash * 0.3))
	elif i1 > 0.1:
		char_fx.color = char_fx.color.lerp(color_1, i1)
	elif i2 > 0.1:
		char_fx.color = char_fx.color.lerp(color_2, i2)
		
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad de las olas de luz. |
| **delay** | `0.15` | Tiempo entre cada letra. |
| **c1** | `Color.CYAN` | Color del barrido desde la izquierda. |
| **c2** | `Color.RED` | Color del barrido desde la derecha. |

> **Ejemplo de uso:**
> `[clash speed=3.0 c1=00ffff c2=ff0000]Texto de ejemplo.[/clash]`

---

## 72. Perspective Tilt (Inclinación 3D)

El texto oscila lateralmente con un efecto de deformación que simula profundidad 3D.

### 🖼️ Muestra:
![72](https://github.com/user-attachments/assets/cee46103-5b8d-4963-917b-bed8259b771e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextPerspectiveTilt
extends RichTextEffect

var bbcode = "tilt"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var angle_max = char_fx.env.get("angle", 0.5)
	var skew = sin(char_fx.elapsed_time * speed) * angle_max
	char_fx.transform.x.y = skew 
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad de la oscilación. |
| **angle** | `0.5` | Máximo ángulo de inclinación. |

> **Ejemplo de uso:**
> `[tilt speed=2.0 angle=0.4]Texto de ejemplo.[/tilt]`

---

## 73. Loading Dot (Carga)

Las letras saltan de forma cíclica una por una, imitando un indicador de carga.

### 🖼️ Muestra:
![73](https://github.com/user-attachments/assets/edabf83c-34ab-43b3-8d74-e51699ce3c98)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextLoading
extends RichTextEffect

var bbcode = "load"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 5.0)
	var height = char_fx.env.get("height", 8.0)
	var t = fmod(char_fx.elapsed_time * speed - char_fx.relative_index, char_fx.glyph_count * 1.5)
	
	if t > 0.0 and t < PI:
		char_fx.offset.y -= sin(t) * height
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `5.0` | Rapidez del ciclo de salto. |
| **height** | `8.0` | Altura máxima del salto. |

> **Ejemplo de uso:**
> `[load speed=5.0 height=10.0]Texto de ejemplo.[/load]`

---

## 74. Breath Glow (Respiración)

El texto pulsa suavemente cambiando su brillo y opacidad, como si estuviera respirando.

### 🖼️ Muestra:
![74](https://github.com/user-attachments/assets/00025835-dcea-4376-aacc-5b79b96e3752)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextBreath
extends RichTextEffect

var bbcode = "breath"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var pulse = (sin(char_fx.elapsed_time * speed) + 1.0) / 2.0
	char_fx.color = char_fx.color.lerp(Color.WHITE, pulse * 0.7)
	char_fx.color.a = lerp(0.4, 1.0, pulse)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del ciclo de respiración. |

> **Ejemplo de uso:**
> `[breath speed=2.0]Texto de ejemplo.[/breath]`

---

## 75. Heartbeat (Latido)

Simula el ritmo de un corazón (lub-dub) aplicando un pulso doble a la escala del texto.

### 🖼️ Muestra:
![75](https://github.com/user-attachments/assets/1f67f0d1-d788-4acd-856c-b57ac6978c93)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextHeartbeat
extends RichTextEffect

var bbcode = "heart"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 1.0)
	var t = fmod(char_fx.elapsed_time * speed, 2.0)
	var s = 1.0
	
	if t < 0.2: s = 1.2
	elif t < 0.4: s = 1.0
	elif t < 0.6: s = 1.15
	else: s = 1.0
	
	char_fx.transform = char_fx.transform.scaled_local(Vector2(s, s))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `1.0` | Frecuencia de los latidos. |

> **Ejemplo de uso:**
> `[heart speed=1.5]Texto de ejemplo.[/heart]`

---

## 76. Static Noise (Estática)

Vibración caótica y cambios de opacidad aleatorios que simulan la estática de un televisor antiguo.

### 🖼️ Muestra:
![76](https://github.com/user-attachments/assets/901016c1-6495-4448-a79a-be3f80689cda)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextStatic
extends RichTextEffect

var bbcode = "static"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	if randf() > 0.8:
		char_fx.offset += Vector2(randf_range(-1, 1), randf_range(-1, 1))
		char_fx.color.a = randf_range(0.6, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[static]Texto de ejemplo.[/static]`

---

## 77. Snake (Serpiente)

El texto se desplaza siguiendo una trayectoria sinuosa en forma de "S", imitando el movimiento de una serpiente.

### 🖼️ Muestra:
![77](https://github.com/user-attachments/assets/95f54edd-f1fe-4696-b2e4-ba107134b100)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSnake
extends RichTextEffect

var bbcode = "snake"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var amp = char_fx.env.get("amp", 10.0)
	var t = char_fx.elapsed_time * speed + char_fx.relative_index * 0.4
	
	char_fx.offset.x += cos(t) * amp
	char_fx.offset.y += sin(t * 0.5) * amp
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad del desplazamiento. |
| **amp** | `10.0` | Amplitud del movimiento (qué tanto se aleja del centro). |

> **Ejemplo de uso:**
> `[snake speed=3.0 amp=10.0]Texto de ejemplo.[/snake]`

---

## 78. Mirror (Reflejo en Agua)

Crea una distorsión de inclinación y balanceo vertical que simula el reflejo sobre una superficie líquida.

### 🖼️ Muestra:
![78](https://github.com/user-attachments/assets/426531b3-a62d-4632-a2f9-ddcbaed35b93)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextMirror
extends RichTextEffect

var bbcode = "mirror"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 4.0)
	var t = char_fx.elapsed_time * speed + char_fx.relative_index
	
	char_fx.transform.x.y = sin(t) * 0.2
	char_fx.offset.y += cos(t) * 2.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `4.0` | Velocidad de la oscilación del reflejo. |

> **Ejemplo de uso:**
> `[mirror speed=4.0]Texto de ejemplo.[/mirror]`

---

## 79. Stairs (Escalón)

Las letras suben y bajan de posición en "escalones" discretos, creando un movimiento rítmico y mecánico.

### 🖼️ Muestra:
![79](https://github.com/user-attachments/assets/b48e9905-49f7-4668-9e26-efca12e5f1c9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextStairs
extends RichTextEffect

var bbcode = "stairs"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = char_fx.elapsed_time * 2.0 + char_fx.relative_index * 0.2
	var step = floor(sin(t) * 3.0)
	
	char_fx.offset.y += step * 5.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[stairs]Texto de ejemplo.[/stairs]`

---

## 80. Magnet (Atracción Magnética)

Las letras se separan y vuelven a juntarse hacia el centro del texto, como si un imán las jalara y soltara rítmicamente.

### 🖼️ Muestra:
![80](https://github.com/user-attachments/assets/6d932646-654b-4372-b2b1-fc3d98b330bb)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextMagnet
extends RichTextEffect

var bbcode = "magnet"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 2.0)
	var center = char_fx.glyph_count / 2.0
	var dist = char_fx.relative_index - center
	var pull = sin(char_fx.elapsed_time * speed) * dist * 5.0
	
	char_fx.offset.x += pull
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `2.0` | Velocidad del ciclo de atracción/repulsión. |

> **Ejemplo de uso:**
> `[magnet speed=2.0]Texto de ejemplo.[/magnet]`

---

## 81. Glitch Block (Bloque de Error)

Mueve caracteres de forma lateral muy rápida y aumenta su brillo esporádicamente, simulando un fallo de señal.

### 🖼️ Muestra:
![81](https://github.com/user-attachments/assets/69897c23-20a6-4e98-be03-0218a343ed5d)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextGlitchBlock
extends RichTextEffect

var bbcode = "g_block"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	if fmod(char_fx.elapsed_time, 0.5) < 0.1:
		char_fx.offset.x += randf_range(-10, 10)
		char_fx.color.v += 2.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[g_block]Texto de ejemplo.[/g_block]`

---

## 82. Orbit (Órbita)

Cada letra gira individualmente en círculos pequeños alrededor de su posición original.

### 🖼️ Muestra:
![82](https://github.com/user-attachments/assets/53b28829-aaf0-4b75-863e-9bfbe2011040)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextOrbit
extends RichTextEffect

var bbcode = "orbit"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = char_fx.elapsed_time * 4.0 + char_fx.relative_index * 0.5
	char_fx.offset += Vector2(cos(t), sin(t)) * 8.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[orbit]Texto de ejemplo.[/orbit]`

---

## 83. Ghost Trail (Rastro Fantasma)

Las letras parpadean y dejan de estar fijas, simulando una estela o rastro transparente en movimiento.

### 🖼️ Muestra:
![83](https://github.com/user-attachments/assets/1e597318-9f24-4706-aaed-d3452b0fa0c0)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextGhostTrail
extends RichTextEffect

var bbcode = "trail"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = sin(char_fx.elapsed_time * 10.0 + char_fx.relative_index)
	char_fx.color.a = 0.5 + t * 0.5
	if t > 0.8:
		char_fx.offset.x += 4.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[trail]Texto de ejemplo.[/trail]`

---

## 84. Earthquake (Terremoto)

Sacudidas violentas en todas direcciones que aumentan y disminuyen de intensidad rítmicamente.

### 🖼️ Muestra:
![84](https://github.com/user-attachments/assets/30c1ad5f-82b6-4a17-8819-2bb09b9bb904)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextQuake
extends RichTextEffect

var bbcode = "quake"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var intensity = (sin(char_fx.elapsed_time * 0.5) + 1.0) * 5.0
	char_fx.offset += Vector2(randf_range(-1,1), randf_range(-1,1)) * intensity
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[quake]Texto de ejemplo.[/quake]`

---

## 85. Liquid (Calor Líquido)

Deforma suavemente el eje de las letras de forma sinusoidal, similar a la distorsión del aire por calor intenso.

### 🖼️ Muestra:
![85](https://github.com/user-attachments/assets/8316f8b1-e8e4-4ea4-9eb5-1e8696d214cb)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextLiquid
extends RichTextEffect

var bbcode = "liquid"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var speed = char_fx.env.get("speed", 3.0)
	var t = char_fx.elapsed_time * speed + char_fx.relative_index * 0.3
	
	char_fx.transform.x.y = sin(t) * 0.3
	char_fx.transform.y.x = cos(t) * 0.2
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **speed** | `3.0` | Velocidad de la distorsión líquida. |

> **Ejemplo de uso:**
> `[liquid speed=3.0]Texto de ejemplo.[/liquid]`

---

## 86. Drifting (Derrape)

Las letras se desplazan suavemente hacia la derecha mientras se desvanecen y reaparecen instantáneamente a la izquierda.

### 🖼️ Muestra:
![86](https://github.com/user-attachments/assets/fba871df-ad99-4f9a-a563-65b1f7f217e9)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextDrift
extends RichTextEffect

var bbcode = "drift"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = fmod(char_fx.elapsed_time + char_fx.relative_index * 0.1, 2.0)
	char_fx.offset.x = lerp(0.0, 30.0, t / 2.0)
	char_fx.color.a = 1.0 - (t / 2.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[drift]Texto de ejemplo.[/drift]`

---

## 87. Yoshi’s Island (Mareado)

Inspirado en el efecto "Touch Fuzzy, Get Dizzy". Las letras se balancean y cambian de tamaño de forma errática.

### 🖼️ Muestra:
![87](https://github.com/user-attachments/assets/c53c67d6-d4c1-4fa1-8abd-e2910d3d3948)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextYoshi
extends RichTextEffect

var bbcode = "yoshi"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = char_fx.elapsed_time * 3.0
	char_fx.offset += Vector2(sin(t), cos(t * 1.5)) * 10.0
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0 + sin(t)*0.2, 1.0 + cos(t)*0.2))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[yoshi]Texto de ejemplo.[/yoshi]`

---

## 88. Pokémon Damage (Daño)

Simula la sacudida horizontal rápida que ocurre cuando un Pokémon recibe un golpe en los juegos clásicos.

### 🖼️ Muestra:
![88](https://github.com/user-attachments/assets/54dc1264-be72-4631-ae3b-b3c7e71e12a8)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextPkmn
extends RichTextEffect

var bbcode = "pkmn"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = fmod(char_fx.elapsed_time, 0.4)
	if t < 0.2: char_fx.offset.x += 15.0
	else: char_fx.offset.x -= 15.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[pkmn]Texto de ejemplo.[/pkmn]`

---

## 89. Zelda Item (Tesoro)

Las letras brillan en color dorado y se elevan suavemente, como cuando Link obtiene un objeto importante.

### 🖼️ Muestra:
![89](https://github.com/user-attachments/assets/b97bf6df-015f-430f-bb30-470e18c05df5)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextZelda
extends RichTextEffect

var bbcode = "zelda"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var light = abs(sin(char_fx.elapsed_time * 4.0 + char_fx.relative_index))
	char_fx.color = char_fx.color.lerp(Color.GOLD, light)
	char_fx.offset.y -= light * 5.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[zelda]Texto de ejemplo.[/zelda]`

---

## 90. Persona 5 (Estilo P5)

Inclinación fija con una vibración caótica de los caracteres, imitando la interfaz dinámica de Persona 5.

### 🖼️ Muestra:
![90](https://github.com/user-attachments/assets/62cbc5c0-dedd-4231-b6a3-69f2a0e9d102)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextP5
extends RichTextEffect

var bbcode = "p5"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	char_fx.transform.x.y = 0.3
	if fmod(char_fx.elapsed_time, 0.2) > 0.1:
		char_fx.offset += Vector2(randf_range(-2,2), randf_range(-2,2))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[p5]Texto de ejemplo.[/p5]`

---

## 91. Undertale Sans (Voz de Sans)

Las letras saltan de forma rítmica y desfasada, imitando el estilo de diálogo de Sans el esqueleto.

### 🖼️ Muestra:
![91](https://github.com/user-attachments/assets/1f3bbdf1-eb22-42d6-a537-6b01781f77c7)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextSans
extends RichTextEffect

var bbcode = "sans"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var step = int(char_fx.elapsed_time * 10.0)
	if (step + char_fx.relative_index) % 4 == 0:
		char_fx.offset.y -= 4.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[sans]Texto de ejemplo.[/sans]`

---

## 92. Minecraft Enchant (Runas)

Los caracteres cambian aleatoriamente a símbolos extraños y se tornan púrpuras, como en la mesa de encantamientos.

### 🖼️ Muestra:
![92](https://github.com/user-attachments/assets/12768a22-47a9-424b-9997-5a60d85e991b)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextEnchant
extends RichTextEffect

var bbcode = "enchant"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	if randf() > 0.95:
		char_fx.glyph_index += randi_range(1, 20)
		char_fx.color = Color.PURPLE
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[enchant]Texto de ejemplo.[/enchant]`

---

## 93. MGS Alert (¡Alert!)

El texto salta violentamente y parpadea en rojo brillante, imitando el ícono de alerta de Metal Gear Solid.

### 🖼️ Muestra:
![93](https://github.com/user-attachments/assets/cb8b7c5b-edbf-483a-bf79-bad2381fd84a)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextAlert
extends RichTextEffect

var bbcode = "alert"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var s = sin(char_fx.elapsed_time * 20.0)
	char_fx.offset.y -= abs(s) * 10.0
	char_fx.color = Color.RED if s > 0 else Color.WHITE
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[alert]Texto de ejemplo.[/alert]`

---

## 94. Mario Fireball (Fuego)

Las letras rebotan siguiendo una trayectoria parabólica y cambian entre colores naranja y amarillo.

### 🖼️ Muestra:
![94](https://github.com/user-attachments/assets/71fe6f66-3dd0-408b-bc03-a51dcaee4af7)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFireball
extends RichTextEffect

var bbcode = "mfire"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = fmod(char_fx.elapsed_time * 3.0 + char_fx.relative_index * 0.5, PI)
	char_fx.offset.y -= sin(t) * 20.0
	char_fx.color = Color.ORANGE_RED.lerp(Color.YELLOW, sin(t))
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[mfire]Texto de ejemplo.[/mfire]`

---

## 95. Final Fantasy Cursor

Un movimiento horizontal rítmico que emula el puntero de selección de los Final Fantasy clásicos.

### 🖼️ Muestra:
![95](https://github.com/user-attachments/assets/eea485eb-d5b8-4885-af74-f5e89dcd9d6e)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextFF
extends RichTextEffect

var bbcode = "ff"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	char_fx.offset.x += sin(char_fx.elapsed_time * 5.0) * 10.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[ff]Texto de ejemplo.[/ff]`

---

## 96. Splatoon Ink (Tinta)

Las letras se deforman elásticamente y "gotean" hacia abajo, simulando la viscosidad de la tinta.

### 🖼️ Muestra:
![96](https://github.com/user-attachments/assets/da439293-adbb-4896-8670-a18c8e156196)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextInk
extends RichTextEffect

var bbcode = "ink"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = char_fx.elapsed_time + char_fx.relative_index * 0.2
	char_fx.transform = char_fx.transform.scaled_local(Vector2(1.0 + sin(t)*0.1, 1.0 - sin(t)*0.1))
	char_fx.offset.y += abs(sin(t)) * 5.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[ink]Texto de ejemplo.[/ink]`

---

## 97. Black Hole (Agujero Negro)

Efecto de gravedad donde las letras son succionadas hacia el centro del texto y pierden opacidad.

### 🖼️ Muestra:
![97](https://github.com/user-attachments/assets/e88f5a3c-c5f3-4ba8-ade0-2c9286a2bdcf)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextVoid
extends RichTextEffect

var bbcode = "void"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var center = char_fx.glyph_count / 2.0
	var diff = center - char_fx.relative_index
	char_fx.offset.x += diff * sin(char_fx.elapsed_time) * 5.0
	char_fx.color.a = clamp(1.0 - abs(diff/center), 0.2, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[void]Texto de ejemplo.[/void]`

---

## 98. DNA Helix (Hélice)

Las letras se cruzan en un movimiento vertical entrelazado que simula la estructura de una cadena de ADN.

### 🖼️ Muestra:
![98](https://github.com/user-attachments/assets/aab8609f-44f2-46c1-9c19-9158addddcec)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextDNA
extends RichTextEffect

var bbcode = "dna"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = char_fx.elapsed_time * 3.0 + char_fx.relative_index * 0.5
	char_fx.offset.y = sin(t) * 15.0
	char_fx.color.a = clamp(cos(t), 0.3, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[dna]Texto de ejemplo.[/dna]`

---

## 99. Ghost Writer (Escritor Fantasma)

Las letras aparecen y desaparecen con un desvanecimiento suave y un ligero movimiento vertical.

### 🖼️ Muestra:
![99](https://github.com/user-attachments/assets/ac5a77c3-909a-4d1d-a263-931962587983)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextGhostW
extends RichTextEffect

var bbcode = "gwrite"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = sin(char_fx.elapsed_time * 2.0 + char_fx.relative_index)
	char_fx.color.a = smoothstep(-0.5, 0.5, t)
	char_fx.offset.y -= (1.0 - char_fx.color.a) * 10.0
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[gwrite]Texto de ejemplo.[/gwrite]`

---

## 100. Supernova (Explosión)

Un estallido de brillo blanco que se transforma en un ciclo de colores vibrantes a gran escala.

### 🖼️ Muestra:
![100](https://github.com/user-attachments/assets/718205c8-a8a4-4f96-a71d-bb495cd7dd07)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextNova
extends RichTextEffect

var bbcode = "nova"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var p = fmod(char_fx.elapsed_time, 2.0)
	if p < 0.5:
		char_fx.color = Color.WHITE * 5.0
		char_fx.transform = char_fx.transform.scaled_local(Vector2(1.5, 1.5))
	else:
		char_fx.color = Color.from_hsv(p, 0.8, 1.0)
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[nova]Texto de ejemplo.[/nova]`

---

## 101. Matrix Neo (Código)

Lluvia de caracteres con parpadeo verde binario que termina estabilizándose en el texto final.

### 🖼️ Muestra:
![101](https://github.com/user-attachments/assets/d153783e-1379-40e9-9df4-859ac3f3c655)

### 💻 Codigo:
```gdscript
@tool
class_name RichTextNeo
extends RichTextEffect

var bbcode = "neo"

func _process_custom_fx(char_fx: CharFXTransform) -> bool:
	var t = fmod(char_fx.elapsed_time, 3.0)
	if t < 2.0:
		char_fx.color = Color(0, randf(), 0, 1)
		char_fx.offset.y += randf_range(-2, 2)
	else:
		char_fx.color = Color.GREEN
	return true
```

### 📊 Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :---: | :--- |
| **N/A** | `N/A` | No requiere parámetros adicionales. |

> **Ejemplo de uso:**
> `[neo]Texto de ejemplo.[/neo]`
