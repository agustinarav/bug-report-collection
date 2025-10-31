## Hallazgo: Exposición parcial de datos en flujo de compra como invitado en e-commerce internacional (SANITIZADO)

## Resumen
Durante pruebas exploratorias en un e-commerce internacional (empresa anonimizada), se detectó que al ingresar un correo de prueba durante el flujo de compra como invitado, la aplicación:
- reconoce la existencia de la cuenta como usuario registrado,
- muestra una interfaz que sugiere que hay una sesión activa (ej. botón “No soy yo / Cerrar sesión”),
- autocompleta parcialmente datos de envío (campos enmascarados).

Esto implica un riesgo de **enumeración de usuarios** y exposición parcial de datos personales sin autenticación.

> **Nota:** Todos los nombres, URLs y datos sensibles han sido anonimizados en este documento para evitar exposición.

---

## Estado
- **Proyecto:** E-commerce internacional (anonimizado)  
- **Fecha del hallazgo:** 31-10-2025  
- **Tester**: Agustina Ravanedo**
- **Platforma**: Google Chrome
- **OS:** Windows 10
- **Tipo:** No funcional - Seguridad
- **Reproducible:** Sí
- **Prioridad:** Alta
- **Fuente:** Testing exploratorio

---

## Pasos para reproducir (sanitizados)
1. Agregar un producto al carrito como invitado en el sitio.  
2. Clickear en el ícono de carrito de compras
3. Clickear "Ir a pagar" 
4. Introducir un correo que en pruebas se sabe está registrado en el sistema.  
5. Observar que la interfaz:
   - Muestra mensaje de cuenta asociada y que se completará el formulario automáticamente
   - Muestra opción “No soy yo / Cerrar sesión” sin iniciar sesión,
   - Autocompleta parcialmente los campos de dirección (enmascarados).  

> No se incluyen URLs ni endpoints reales para evitar divulgación.

---

## Comportamiento observado
- El sistema reconoce la cuenta y accede a datos asociados sin que el usuario se haya autenticado.
- El flujo de la página puede quedar bloqueado o con carga indefinida después de esto.

## Comportamiento esperado
- El sistema **no debería confirmar** si el correo existe.  
- No debería mostrar ni autocompletar datos de usuarios sin autenticación.  
- Mensaje seguro recomendado:  
  > “Si ya tienes una cuenta, inicia sesión para continuar. Si existe una cuenta asociada, recibirás instrucciones por correo.”

---

## Impacto
- Posible **enumeración de usuarios** (confirmar qué correos están registrados).  
- Exposición de información personal parcial (aunque esté enmascarada).  
- Riesgo de **ingeniería social o phishing** si la información se combina con otras fuentes.

---

## Recomendaciones
1. Evitar mensajes que confirmen la existencia de la cuenta; usar mensajes genéricos.  
2. No recuperar ni mostrar información de perfil sin comprobar sesión autenticada.  
3. Revisar lógica de sesión/token y control de acceso en endpoints que manejan datos de usuario.  
4. **Implementar rate-limit** en intentos de verificación de correo para mitigar enumeración: limitar la cantidad de intentos por IP o usuario en un tiempo determinado.  
5. Realizar test de seguridad adicionales y revisar logs para detectar intentos de abuso.

---

## Evidencia (sanitizada)
- Capturas originales y logs almacenados en privado para referencia personal.  






