# Auditoría visual y de experiencia de usuario — Serendipia

Fecha: 11 de noviembre de 2026

## Alcance y método

Se revisó la estructura de la página de inicio en Astro, sus componentes, estilos y destinos de navegación. Se ejecutó la web localmente y se observó en un navegador con ventanas configuradas a **1440 × 900**, **768 × 1024** y **390 × 844 píxeles**. La barra de desplazamiento del navegador reduce ligeramente el ancho útil en algunos casos. Las observaciones son de esta ejecución local, no de una web publicada.

Se probó el desplazamiento del hero, la apertura del menú móvil, la opción SEO de ese menú, los controles de los carruseles móviles de servicios, estándar europeo y noticias, la apertura de una pregunta, el avance de un testimonio, el enlace «visitar el blog» y el CTA «Déjanos tus datos». También se contrastaron destinos y estructura en el código. Se utilizó la skill `frontend-design` de Anthropic como guía para valorar coherencia, jerarquía, claridad y movimiento, respetando la identidad ya definida.

**Límites:** no se hizo una prueba con personas usuarias, dispositivos físicos, lector de pantalla, conexiones lentas ni una auditoría automatizada completa de accesibilidad. No se midieron todos los contrastes ni el rendimiento real en redes móviles. No se enviaron mensajes ni datos a servicios externos y no se verificó la titularidad de las redes o testimonios. La barra oscura de herramientas de Astro visible en algunas capturas pertenece al entorno de desarrollo y no se considera parte del diseño publicado.

**Clasificación:** «comprobado» significa observado en el navegador o verificable directamente en el código; «opinión de diseño» indica una recomendación profesional que convendría contrastar con usuarios y objetivos comerciales. Las prioridades son propuestas, no resultados de una prueba de conversión.

## Resumen de la impresión general

Serendipia tiene una dirección visual reconocible: el ajedrez introduce la idea de estrategia; el morado, el dorado, el vidrio y el movimiento aportan personalidad. La página ofrece un recorrido amplio por servicios, diferenciadores, resultados, dudas y contenidos. Las imágenes, el caso de éxito y las tarjetas aportan materia prima para una presentación convincente.

La oportunidad principal está en **facilitar que la persona entienda qué hacer y pueda hacerlo**. El mensaje y la navegación aparecen después del video; varios enlaces tienen destinos incompletos; las llamadas a dejar datos o agendar terminan en un footer sin formulario ni agenda. En tablet, el abanico de servicios requiere desplazamiento horizontal. La cantidad y duración de entradas animadas también puede competir con la lectura. Estos aspectos se pueden pulir conservando el concepto visual y el video con scroll.

## Qué funciona bien y recomendaría conservar

- **Hero de ajedrez:** la pieza y el tablero forman una metáfora estratégica propia, con una presencia visual memorable.
- **Paleta de marca:** el morado profundo y el dorado ofrecen una combinación distintiva; las tarjetas añaden variedad sin necesitar un rediseño genérico.
- **Caso de éxito:** el dato de alrededor de 800 leads y una voz identificada hacen más concreta la promesa comercial.
- **Diversidad visual:** el abanico de servicios, la cuadrícula tipo bento y el mosaico de noticias evitan que toda la página tenga el mismo patrón.
- **Componentes accesibles de base:** las FAQ usan `details`/`summary`; las imágenes decorativas se identifican como tales; las flechas de carrusel tienen nombres accesibles y existe tratamiento para movimiento reducido.
- **Móvil:** el menú inferior y los carruseles disponen de botones visibles; en la prueba los controles de los tres carruseles avanzaron y el menú de Soluciones se abrió.

## Evaluación por sección y oportunidades de mejora

### Hero, navegación y video — `Hero.astro`, `Navbar.astro`, `MobileTabMenu.astro`, `style.css`

1. **Mensaje y navegación iniciales.** Al cargar, se observa el ajedrez, el logo y un aviso pequeño «Desliza para explorar»; el titular, la propuesta, el CTA y la navegación aparecen al terminar el recorrido del video. El proyecto reserva `300svh` para el video y `100svh` adicionales para el contenido. **Por qué:** una persona que llega por primera vez debe desplazarse antes de saber a qué se dedica la agencia o encontrar un acceso principal. **Propuesta:** conservar la secuencia y el video, pero mostrar desde el inicio una frase breve de orientación y una opción visible de «Ver servicios» o «Saltar introducción»; comprobar si una distancia menor de scroll mantiene el efecto sin demorar el mensaje. **Prioridad:** alta. **Tipo:** comprobado en la ejecución y en código; impacto en conversión por validar.

2. **Peso del video y tolerancia a fallos.** El archivo local `chess_hd_hero_scroll_hq.mp4` pesa 36.806.209 bytes, aproximadamente 35,1 MiB. La revelación normal depende de que el video tenga metadatos y alcance su último fotograma; existe una alternativa para movimiento reducido. **Por qué:** el peso puede afectar a personas con datos limitados y la dependencia del video merece una salida segura si la carga o la decodificación fallan. **Propuesta:** conservar el mismo metraje y probar una versión optimizada, carga por rango y un poster; añadir una vía que muestre el mensaje si el video tarda demasiado o falla. Medir antes y después en red móvil simulada. **Prioridad:** alta. **Tipo:** tamaño y dependencia comprobados en código; impacto real y funcionamiento ante error no medidos.

3. **Enlaces del menú de escritorio.** Los botones enlazan a `#seo`, `#publicidad`, `#diseno`, `#soluciones` y `#informacion`; en el DOM revisado no existen elementos con esos identificadores. **Por qué:** los rótulos sugieren una navegación a contenido que no tiene destino en esta página. **Propuesta:** conectar cada opción con su sección o servicio real y comprobar la navegación desde escritorio y con teclado. **Prioridad:** alta. **Tipo:** comprobado en código y DOM; no se probó individualmente cada enlace con clic.

4. **Soluciones y WhatsApp en móvil.** El desplegable se abre y tiene tres opciones; todas apuntan a `/#services-title`. Al elegir SEO, el carrusel mostró inicialmente «Estudio de mercado», no SEO. El enlace de WhatsApp usa `wa.me` con texto, pero sin número de destinatario. **Por qué:** el destino no coincide con la elección y no hay un destinatario empresarial definido. **Propuesta:** seleccionar la tarjeta correspondiente al elegir una solución y configurar un número de WhatsApp confirmado, con texto que indique que se abrirá la aplicación. **Prioridad:** alta. **Tipo:** selección de SEO comprobada; destinos y ausencia de número comprobados en código. No se envió ningún mensaje.

5. **Legibilidad del glass y de los controles.** El panel difuminado ayuda a separar el texto del video, pero la legibilidad cambia con los detalles luminosos detrás. En móvil, la barra fija ocupa parte del área inferior y el desplegable cubre contenido al abrirse. **Por qué:** fondos variables y espacios reducidos pueden dificultar la lectura o tapar información. **Propuesta:** conservar el glass, reforzar su contraste con un tinte oscuro controlado, comprobarlo en varios fotogramas y reservar espacio suficiente para la barra; probar foco, cierre con Escape y tamaños táctiles en dispositivos reales. **Prioridad:** media. **Tipo:** observación visual y propuesta de diseño; contraste y ergonomía no medidos formalmente.

### Marcas — `brands.astro`

6. **Prueba social de ejemplo.** La banda muestra repetidamente el logo de Google; el código crea 12 entradas usando la misma imagen y nombres alternativos «Marca 1» a «Marca 12». **Por qué:** una fila rotulada como marcas que confían en la agencia puede interpretarse como una relación comercial real; las etiquetas genéricas no identifican las marcas a tecnologías de asistencia. **Propuesta:** sustituir los ejemplos por logos autorizados y nombres reales cuando estén disponibles; si aún no lo están, aclarar que son referencias de diseño o evitar presentar la fila como prueba de clientes. **Prioridad:** alta antes de publicar. **Tipo:** repetición y etiquetas comprobadas; no se ha verificado ninguna relación comercial.

### Servicios — `services.astro`, `style.css`, `Layout.astro`

7. **Abanico en tablet.** A 768 px se ve una barra de desplazamiento horizontal y las tarjetas de los extremos quedan parcialmente fuera de la vista. El contenido de la tarjeta central también se ve muy ajustado verticalmente. **Por qué:** en ese ancho todavía se usa una baraja de anchura mínima de 900 px, mientras que el carrusel móvil comienza por debajo de 768 px. La flecha de la tarjeta del extremo puede quedar fuera de la vista inicial. **Propuesta:** extender el carrusel táctil a ese rango o adaptar el abanico al ancho disponible, con controles siempre visibles; permitir altura suficiente para listas largas. **Prioridad:** alta. **Tipo:** barra y recorte comprobados a 768 px; revisar alturas en más dispositivos antes de definir el punto de corte.

8. **Descubrir el detalle en móvil.** La tarjeta inicialmente presenta el nombre; al tocar la tarjeta de SEO se mostró su descripción y lista. **Por qué:** no hay una indicación textual visible de que se puede tocar para revelar más. **Propuesta:** mantener el diseño de tarjetas e incorporar una pista como «Toca para ver detalles», con un control explícito y estado accesible; verificar teclado y pantallas táctiles. **Prioridad:** media. **Tipo:** interacción comprobada; riesgo de descubrimiento es opinión de diseño.

9. **Movimiento y espera.** En escritorio las tarjetas caen escalonadas y los títulos aparecen letra por letra; durante la entrada se vio temporalmente el espacio aún sin contenido. **Por qué:** varios movimientos simultáneos pueden ralentizar la lectura si se navega rápidamente. **Propuesta:** mantener la caída distintiva, pero reducir la distancia o hacer que títulos y tarjetas se vuelvan legibles antes; evitar que contenido importante dependa exclusivamente de una animación y respetar movimiento reducido. **Prioridad:** media. **Tipo:** secuencia observada; preferencia de ritmo es opinión de diseño.

### Estándar europeo — `eu-quality.astro`

10. **Jerarquía y contenido del bento.** La combinación de fotografía y fondos pastel funciona como una agrupación clara; el mapa refuerza la idea europea. Los cinco párrafos son relativamente largos y no llevan un título breve que facilite comparar. **Por qué:** exige leer cada tarjeta completa para distinguir beneficios. **Propuesta:** conservar el bento y añadir rótulos cortos como «Equipo especializado», «ROI» o «Dirección estratégica», reduciendo repetición en los párrafos y manteniendo sus afirmaciones verificables. **Prioridad:** media. **Tipo:** opinión de diseño.

11. **Espacios en el título y nombre accesible.** En la ejecución se vio «enEcuador» sin separación; el árbol accesible expuso «Estándar Europeo yMetodología Avanzada enEcuador». **Por qué:** afecta la lectura visual y la lectura por tecnologías de asistencia. **Propuesta:** preservar espacios explícitos alrededor del salto de línea y de los spans resaltados, y construir el nombre accesible con esas separaciones. **Prioridad:** alta. **Tipo:** comprobado visualmente y en el árbol accesible.

### Caso de éxito — `opinions.astro`

12. **Prueba de resultados.** La cita de 800 leads en cinco meses aporta una cifra concreta; la imagen del dashboard muestra otras cifras y se presenta como «Dashboard del caso de éxito». **Por qué:** no queda claro cuáles son resultados medidos del caso y cuáles son ilustrativos; no se ha verificado su procedencia. **Propuesta:** mantener la imagen y el testimonio, aclarar si el dashboard es ilustrativo y acompañar los datos reales con periodo, contexto y fuente autorizada; enlazar a un caso detallado propio si existe. **Prioridad:** alta antes de publicación. **Tipo:** contenido y texto alternativo comprobados; veracidad no verificada.

13. **Destino de «Conoce más».** Tanto el nombre del proyecto como el botón conducen al sitio externo de Bachillerato en línea. **Por qué:** alguien que espera leer cómo Serendipia logró el resultado sale de la web sin llegar a un caso de estudio. **Propuesta:** distinguir «Visitar al cliente» de «Ver caso de éxito» y dirigir el segundo a una explicación del trabajo, si se prepara. **Prioridad:** media. **Tipo:** destino comprobado en código; claridad del recorrido es opinión de diseño.

### Propuesta — `proposal.astro`

14. **Promesa del CTA.** «Déjanos tus datos» conduce a `#contacto`, que es el footer; en la prueba no apareció formulario ni campo donde dejarlos. **Por qué:** el resultado no corresponde con el texto del botón. **Propuesta:** conectar el CTA a un formulario o canal real de contacto y explicar el siguiente paso; mientras no exista, usar una etiqueta que describa con precisión el destino disponible. Conservar el cohete y su aterrizaje, procurando que no retrase el acceso al botón. **Prioridad:** alta. **Tipo:** destino y ausencia de formulario comprobados en la ejecución.

15. **Proporción en móvil.** En 390 × 844 el cohete queda debajo del CTA y sobresale del panel; el menú fijo ocupa parte del borde inferior de la pantalla. **Por qué:** la ilustración tiene mucho peso vertical respecto al mensaje y exige más desplazamiento. **Propuesta:** mantener el recurso, ajustar su tamaño y espacio inferior en móviles pequeños y comprobar que no cubra contenido ni controles. **Prioridad:** media. **Tipo:** disposición observada; ajuste recomendado como opinión de diseño.

### Preguntas frecuentes — `faq.astro`

16. **Acordeón funcional.** Al abrir la pregunta sobre SEO se mostró su respuesta y se cerró la primera. **Por qué conservarlo:** reduce la longitud visible sin ocultar el mecanismo a teclado por depender de un control nativo. **Propuesta:** mantenerlo y probar además todas las preguntas con teclado y lector de pantalla. **Prioridad:** baja. **Tipo:** apertura y cierre comprobados; prueba completa de accesibilidad pendiente.

17. **Agenda y lectura.** «Agenda tu cita ahora» apunta también a `#contacto`, donde no se observó agenda. El nombre accesible del título principal apareció como «Preguntasfrecuentes». **Por qué:** el botón promete una acción no disponible y el nombre sin espacio dificulta la lectura asistida. **Propuesta:** conectar una agenda real o aclarar el canal; corregir la separación del texto accesible. En móvil, valorar colocar antes las preguntas o un enlace directo a ellas, ya que la tarjeta de CTA precede a la lista. **Prioridad:** alta para destino y nombre accesible; media para el orden. **Tipo:** destinos y nombre comprobados; orden propuesto es opinión de diseño.

### Quiénes somos y testimonios — `who-we-are.astro`

18. **Tarjetas de testimonios.** La pila de tarjetas y la navegación visual refuerzan la personalidad; al pulsar la flecha se pasó a «Opinión 2 de 3». La segunda y tercera entradas usan rótulos genéricos («Experiencia corporativa» y «Experiencia e-commerce»), sin cliente identificado. **Por qué:** podrían interpretarse como testimonios de clientes aunque su procedencia no esté clara. **Propuesta:** sustituirlos por citas reales autorizadas, con nombre o atribución acordada, o etiquetarlos como ejemplos internos hasta contar con evidencia. **Prioridad:** alta antes de publicar. **Tipo:** contenidos y cambio comprobados; no se verificó su autenticidad.

19. **Orden narrativo.** La presentación de la agencia aparece después de servicios, caso de éxito, propuesta y FAQ. **Por qué:** algunas personas podrían querer entender quién está detrás antes de evaluar la promesa europea. **Propuesta:** probar una referencia breve a la agencia cerca del hero o del estándar europeo, manteniendo esta sección al final como ampliación. **Prioridad:** baja. **Tipo:** opinión de diseño, sujeta a prueba.

### Noticias digitales — `blog.astro`

20. **Enlaces sin destino.** Al pulsar «visitar el blog» en local se mostró `404: Not found` para `/blog`; las seis tarjetas también apuntan a `/blog`, no a artículos individuales. **Por qué:** se ofrece lectura que no está disponible en la ruta revisada. **Propuesta:** crear la portada y las páginas reales o enlazar a destinos existentes; hasta entonces, presentar la sección como contenido en preparación sin enlaces que prometan artículos. **Prioridad:** alta. **Tipo:** 404 comprobado en navegador; enlaces de tarjetas comprobados en código.

21. **Tratamiento editorial.** El mosaico 1–3–2 tiene buena jerarquía y las imágenes comparten paleta, pero todas las noticias compiten visualmente con ilustraciones muy similares. **Por qué:** puede costar distinguir de un vistazo el tema o la actualidad de cada nota. **Propuesta:** conservar el mosaico e incorporar categoría legible, fecha real y variedad visual controlada; no añadir fechas inventadas. En móvil, comprobar que el carrusel permita descubrir las seis entradas. **Prioridad:** media. **Tipo:** opinión de diseño; la navegación móvil 1/6 a 2/6 se comprobó.

### Footer — `footer.astro`

22. **Contacto incompleto.** El footer muestra ubicación general, cobertura y una sesión de 30 minutos, pero no un correo, teléfono, formulario ni enlace de reserva. Las redes tienen direcciones genéricas `.../serendipia` configuradas en el código. **Por qué:** es el destino de varios CTA y no permite completar directamente la acción prometida. **Propuesta:** añadir solo canales y enlaces oficiales confirmados; convertir la sesión en enlace real de reserva si está disponible. Verificar que el menú fijo no tape el último contenido en móvil. **Prioridad:** alta. **Tipo:** contenido y enlaces comprobados en código y pantalla; no se verificó la propiedad de las cuentas.

## Recomendaciones por dispositivo

### Escritorio — 1440 × 900

- Mantener el hero panorámico, el vidrio y el abanico de servicios como momentos visuales principales.
- Hacer visible antes una pista de la actividad de la agencia y un acceso directo sin eliminar el video con scroll.
- Afinar el ritmo de entradas: al desplazarse rápidamente se observaron instantes en que letras o tarjetas aún estaban apareciendo. Probar que la información esencial pueda leerse sin esperar al final de todas las secuencias.
- Conectar el menú superior y las rutas del blog antes de evaluar conversiones.

### Tablet — 768 × 1024

- Priorizar el abanico de servicios: la baraja excede el ancho visible y aparece desplazamiento horizontal. Evaluar un breakpoint que use carrusel o una composición adaptada en este intervalo.
- Comprobar que el botón del hero y el texto no se partan de forma incómoda; en la muestra el botón ocupó dos líneas.
- Revisar el orden y altura de FAQ y caso de éxito, que pasan a una columna y generan recorridos más largos. Mantener la separación entre textos, tarjetas y el cohete.
- Ampliar pruebas a tablet horizontal y anchos intermedios: aquí se observó solo 768 × 1024.

### Móvil — 390 × 844 configurados

- Conservar la barra inferior y los controles de carrusel; conectar cada solución con la tarjeta correcta y configurar el WhatsApp empresarial.
- Señalar que las tarjetas de servicio se pueden tocar para revelar información; ofrecer un estado visible y accesible.
- Revisar que el menú fijo no cubra los controles o el final de tarjetas al hacer scroll, especialmente con el desplegable abierto y en pantallas más pequeñas.
- Reducir la longitud percibida de los párrafos más extensos y probar el peso visual del cohete sin retirarlo.
- Probar dispositivos físicos, conexiones lentas, orientación horizontal y preferencias de movimiento reducido; esta auditoría no los cubrió.

## Lista final de cambios sugeridos por prioridad

### Alta

1. Conectar los CTA de contacto y agenda con acciones reales; configurar el número oficial de WhatsApp.
2. Corregir el destino `/blog` y crear o conectar los artículos antes de ofrecerlos como disponibles.
3. Resolver los cinco anclajes del menú de escritorio y hacer que cada opción móvil seleccione el servicio correspondiente.
4. Adaptar el abanico de servicios al ancho de tablet, con controles accesibles y sin recortes de información.
5. Corregir los espacios visuales y nombres accesibles de los encabezados «Estándar Europeo…» y «Preguntas frecuentes».
6. Sustituir o identificar claramente logos y testimonios de ejemplo; aclarar el carácter ilustrativo del dashboard y verificar cifras antes de publicar.
7. Mantener el video de ajedrez, pero ofrecer acceso temprano al mensaje y una salida si el video falla; medir y optimizar su peso.

### Media

8. Mejorar la indicación de que las tarjetas de servicios revelan detalles y validar su uso táctil y con teclado.
9. Ajustar el ritmo de animaciones y la legibilidad del glass sobre fotogramas variables; mantener movimiento reducido.
10. Simplificar y jerarquizar los textos de las tarjetas del bento y distinguir el caso de estudio del enlace al sitio del cliente.
11. Revisar el espacio del menú móvil, el cohete y los CTA en pantallas pequeñas.
12. Reforzar el carácter editorial del blog con fechas reales, categorías legibles y variedad de imágenes.

### Baja

13. Probar si una introducción breve de la agencia antes de servicios mejora la comprensión, manteniendo la sección completa al final.
14. Complementar esta revisión con pruebas de usuarios, teclado, lectores de pantalla, contraste medido, rendimiento y dispositivos físicos.
