# 🧩 Activity #6 – Expresiones regulares

En esta actividad aprenderás a trabajar con **expresiones regulares (regex)** aplicadas a configuraciones de Apache y validaciones en general.  
Estas regex permiten filtrar, validar, redirigir y manipular patrones dentro de URLs, rutas o cadenas.

---

## 📌 Objetivos de la práctica

- Comprender y aplicar expresiones regulares  
- Usar expresiones en Apache (RedirectMatch, patrones de archivos, etc.)  
- Resolver los ejercicios planteados en el PDF  
- Añadir capturas como evidencia  

---

# 🛠️ Paso previo

Se recomienda leer antes:

🔗 http://www.rexegg.com/regex-quickstart.html  
🔗 https://regexr.com  
🔗 http://iie.fing.edu.uy/~vagonbar/unixbas/expreg.htm  

Además, debes realizar los ejercicios de la web:  
👉 http://regexone.com

(Coloca evidencias aquí si tu profesor las pide)

---

# 🧩 Ejercicios y soluciones

A continuación están las expresiones regulares solicitadas en la práctica.

---

## 1️⃣ Directorios dentro de `/www/` cuyo nombre consista en **tres dígitos**

```
^\/www\/(.+\/)?[0-9]{3}$
```

### 📸 *Captura 1 (opcional)*
`![cap1](ruta.png)`

---

## 2️⃣ Coincidir archivos: `*.gif`, `*.jpeg`, `*.jpg`, `*.png`

```
.+\.(gif|jpe?g|png)$
```

---

## 3️⃣ Redireccionar todos los `.gif` a ficheros `.jpg` en **otro servidor**

Directiva Apache:

```
RedirectMatch "(.+)\.gif$" "http://other.example.com/$1.jpg"
```

### 📸 *Captura 2 – Configuración en Apache (opcional)*
`![cap2](ruta.png)`

---

## 4️⃣ Números enteros o decimales

```
\d*\.?\d+
```

---

## 5️⃣ Números de teléfono en formato americano:  
📞 `123-123-1234`

```
\d{3}-?\d{3}-?\d{4}
```

---

## 6️⃣ Palabras (solo letras)

```
[a-zA-Z]+
```

---

## 7️⃣ Códigos hexadecimales de color (24 o 32 bits)

```
(#|0x)?(?:[0-9A-F]{2}){3,4}
```

---

## 8️⃣ Palabras de **4 letras**

```
\w{4}
```

---

## 9️⃣ Número entero sin signo

```
\d+
```

---

## 🔟 Número entero con signo

```
[-+]?\d+
```

---

## 1️⃣1️⃣ Números reales

```
[-+]?(([0-9]*\.[0-9]+)|([0-9]+))
```

---

## 1️⃣2️⃣ Números reales con exponente científico

Ejemplo: `4.5e10`, `-2.3E-4`

```
[-+]?[0-9]*\.?[0-9]+([eE][-+]?[0-9]+)?
```

---

## 1️⃣3️⃣ Emails

```
[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}
```

---

## 1️⃣4️⃣ Números del **0 al 255** (válido para IPs)

```
^([01][0-9][0-9]|2[0-4][0-9]|25[0-5])$
```

---
