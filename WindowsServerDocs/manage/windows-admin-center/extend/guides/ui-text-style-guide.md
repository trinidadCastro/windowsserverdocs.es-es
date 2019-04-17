---
title: Guía de estilo de texto y diseño de la interfaz de usuario de Windows Admin Center
description: Interfaz de usuario de Windows Admin Center texto y diseño Guía de estilo de SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab5bee55975b803a77db0b6cdb179b76590e1d83
ms.sourcegitcommit: 56cccb09a35b2d3eef9056e2406c3a1762d28682
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "4739985"
---
# Guía de estilo de texto y diseño de la interfaz de usuario de Windows Admin Center

>Se aplica a: Windows Admin Center

En este tema se describe el método general para escribir el texto de la interfaz de usuario (IU) de Windows Admin Center, así como algunas convenciones específicas y soluciones que estamos aportando.

Windows Admin Center y las extensiones deben seguir los [principios de voz de Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) para que la experiencia sea descriptiva y fácil de usar. Esta guía de estilo se basa en estos principios de voz, así como en la [Guía de estilo de escritura de Microsoft](https://docs.microsoft.com/style-guide/welcome/), por tanto asegúrate de desproteger estos recursos para obtener información sobre elementos como [accesibilidad](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [acrónimos](https://docs.microsoft.com/style-guide/acronyms) y la [elección de palabras](https://docs.microsoft.com/style-guide/word-choice/) como [por favor](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please)y [lo sentimos](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## Botones

- Los botones deben ser una palabra siempre que sea posible, especialmente si tienes previsto localizar tu herramienta. Dos o tres son correcto pero intenta evitar ya. Si tienes cuatro palabras o ya, es mejor usar un control de vínculo.
- Las etiquetas de los botones deben ser concisas, específicas y autoexplicativas. En lugar de un botón genérico "Enviar", usa un verbo que corresponda a la acción del usuario, como "Crear", "Eliminar", "Agregar", "Formatear", etc.
- Si un botón sigue a una pregunta, su etiqueta debe corresponder con claridad a la pregunta (normalmente "Sí" o "No").

## Uso de mayúsculas

Seguimos el estilo de Microsoft para el [uso de mayúsculas](https://docs.microsoft.com/style-guide/capitalization): uso de mayúsculas con estilo de oración para casi todo.

| Elemento de la interfaz de usuario              |Uso de mayúsculas|Observaciones|
|-------------------------|--------------|--------|
|Distintivos (como VISTA PREVIA) |Todo en mayúsculas      ||
|Todo lo demás          |Estilo de frase|Sin embargo, hay algunas excepciones donde nos encontramos con propiedades de objetos de WMI o PowerShell que se encuentran fuera de nuestro control.|

## Dos puntos

Utilice dos puntos para presentar listas. Por ejemplo:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

No uses puntos y en el texto de la interfaz de usuario cuando una etiqueta es en otra línea de lo que las etiquetas o cuando se produce una distinción clara entre la etiqueta y lo es etiquetar.

Usa puntos y en el texto de la interfaz de usuario cuando una etiqueta es en la misma línea como el texto las etiquetas y que debes tener los dos elementos se ejecuten juntas.

## Mensajes de confirmación

Los cuadros de diálogo de confirmación son útiles cuando continuar podría tener resultados inesperados, como la pérdida de datos. Deben contener información útil, legible con un resultado claro, especialmente para los eventos que no se puede invertir. 

- Asegúrese de que es necesaria una confirmación. Si no hay ninguna información nuevo para ofrecer (por ejemplo, "¿está seguro?"), a continuación, un mensaje de confirmación puede no ser necesario.  
- Comprueba que el cliente quiera continuar con la acción.
- Asegúrate de que la instrucción principal (encabezado) y explicación texto (cuerpo) no son redundantes.
- En el encabezado, definir los posibles resultados como una pregunta o una instrucción sobre qué sucederá a continuación. ¿Por ejemplo, "Borrar todos los datos de esta unidad? o "Está a punto de borrar todos los datos".
- Agregar detalles en el cuerpo. Si hay una variable, como el nombre del elemento que estás acerca de cambio, incluyen aquí.
- Incluir una pregunta sencilla (ya sea en el encabezado o en el cuerpo) que una opción clara entre los dos botones de acción de marcos.
- Para una selección compleja, usa Sí y No botones, que fomentar la lectura de cuidado. Para una elección más simple, usar los botones que son específicas de la acción, como eliminar todos o Cancelar.

## Experiencias de primera ejecución

Cuando un usuario visita por primera vez una página, tienes la oportunidad de ayudarle a empezar a trabajar con la herramienta. Esto podría ser:

- Una cadena de texto en una página en blanco con instrucciones breves sobre cómo empezar a trabajar, por ejemplo, "Selecciona 'Agregar' para agregar una aplicación".
- Un vínculo al control que permite al usuario comenzar, por ejemplo, "Agregar una aplicación para empezar".
- Una animación o un vídeo pequeño o breve que muestre al usuario cómo empezar a trabajar

Estas son algunas sugerencias de nuestra guía de estilo de Windows:

### 1. Sé útil

- Evita lenguaje y estilo de marketing.
- Cuando hagas una demostración o sugieras algo, asegúrate de que el resultado final sea claro; mostrar únicamente al cliente cómo hacer algo no es eficaz si no saben por qué lo están haciendo.
- No presentes sugerencias si el cliente no las necesita.

### 2. Muestra, no indiques

Mantén un texto lo más simple posible (piensa en animaciones o vídeos pequeños).

### 3. No abrumes

- Limita los elementos emergentes y sugerencias a 4 por sesión de uso combinado, incluidas las notificaciones del sistema y las notificaciones del shell.
- Asegúrate de que la sincronización de los elementos emergentes sea útil.
- No impidas que el cliente haga algo.
- Asegúrate de que los elementos emergentes se cierren fácilmente.

### 4. Consigue que sea contextual

- Los momentos didácticos son más eficaces cuando se presentan en el momento adecuado.
- Si creas tutoriales o presentaciones, la información debe ser concreta.
- Evita adornos de marketing, céntrate en consejos y sugerencias específicos.
- Proporciona una forma para que los clientes vuelvan al tutorial más adelante, si es relevante (las personas a menudo no retienen la información la primera vez, pero las instrucciones de configuración podrían no ser relevantes en alguna ocasión).
- La mensajería con estado vacío es un lugar natural para aprender y/o deleitar, haz que sean simples e informativos.

### 5. Minimiza las configuraciones difíciles

Cuando necesites que el cliente realice otra acción para experimentar el valor completo (iniciar sesión en un servicio en línea, etc.), hazlo de la forma más sencilla posible.

- La mensajería debe ser breve y directa.
- Evita enviarlos a otra ubicación. Si es posible, proporciona un medio para conectarse desde donde estén.
- Si es posible, posibilita la opción de hacerlo más adelante y, a continuación, recuérdales la posibilidad de hacerlo más adelante.
- Si los sacas de su experiencia, proporciona una forma de volver de forma rápida y sencilla.

## Vínculos de ayuda

Estas son algunas sugerencias de nuestra guía de estilo de Windows:

### ¿Deberíamos cuando se proporciona un vínculo de ayuda?

Casi nunca. Proporcionar un vínculo de ayuda solo cuando:

- Hay una pregunta obvia e importante que los clientes suelen tener mientras están en la interfaz de usuario que la respuesta a la que te ayudarán a tener éxito en la tarea de la interfaz de usuario. 
- No hay suficiente espacio en la interfaz de usuario para proporcionar la cantidad de información necesaria para que los usuarios correcto en la tarea de la interfaz de usuario. 

### ¿Donde debe ayudar a vínculos a aparecer? 

- Deberían aparecer vínculos de texto lo más cerca del elemento de interfaz de usuario que se dirige la ayuda a que sea posible. 
- Si debes proporcionar un vínculo de texto que se aplica a una pantalla de la interfaz de usuario completa, colócalo en la parte inferior izquierda de la pantalla. 
- Si proporcionas un vínculo a través de un botón de ayuda (?), la información sobre herramientas debe ser "Ayuda".

### ¿Qué dirección URL debemos utilizar?

Nunca vincular directamente a una dirección web, en su lugar, usa un servicio de redireccionamiento.

Los desarrolladores de Microsoft deben usar un FWLink excepto si se trata de un vínculo de ayuda que los usuarios tendrán que escribir manualmente, en cuyo caso, utilice un vínculo aka.ms (siempre que sea el destino de la dirección URL de un sitio Web que reconoce automáticamente la configuración regional del explorador, por ejemplo, Docs.microsoft.com)

### Directrices de texto 

- Utilice oraciones completas.
- No incluyas puntuación excepto signos de interrogación final. 
- No es necesario usar el mismo texto como el título de la tarea; usar el texto que tenga sentido en el contexto de la interfaz de usuario, pero debes asegurarte de que hay una conexión lógica entre las dos. Por ejemplo: 
- Vínculo a la Ayuda: ¿Cuáles son los riesgos de permitir excepciones? 
- Título del tema de ayuda: "Permitir que un programa se comunique a través de Firewall de Windows"
- Sea tan específico como sea posible sobre el contenido del tema de ayuda. 
    - Nuestros estilos
        - ¿Cómo Firewall de Windows ayuda a proteger mi equipo?
        - ¿Por qué aspectos destacados pueden mejorar una imagen
    - No nuestros estilos
        - Para obtener más información acerca de Firewall de Windows
        - Más información sobre la administración de color
        - Más información
- Usa toda la oración para el texto de vínculo, no solo las palabras clave. 
    - Nuestros estilos 
        - [¿Cuáles son los riesgos de permitir excepciones?]()
    - No nuestros estilos
        - ¿Cuáles son los [riesgos de permitir excepciones]()? 

## Mensajes de error

Aquí es orientativa adaptada a partir de la Guía de estilo de Windows:

Escribir un mensaje buena es un equilibrio entre proporcionar suficiente explicación pero no eran demasiado técnico; entre los que se va a informal y personal, pero no molestos u ofensivas.

### Directrices generales

Usa un mensaje por cada caso de error.

#### Encabezados

- Mantenlo breve y concisa Explique cuál es el problema o **lo ideal qué hacer**. <br>Algunas superficies de interfaz de usuario pueden tener encabezados que truncar en lugar de ajustar cuando estén demasiado largas, por lo tanto, no pierdas un vistazo a estas aplicaciones.
- Usa la solución en el encabezado si es un paso sencillo.
- Asegúrate de que el título esté relacionado directamente en el botón en caso de que el lector omite el texto del cuerpo.
- Evitar el uso de "Se ha producido un problema" en encabezados, a menos que ninguna otra opción. Ser más específica sobre el problema.
- Evita el uso de variables (como nombres de archivo, carpeta y aplicación) en encabezados. Colocarlos en el cuerpo.

#### Body

- Si el encabezado lo suficientemente explica el problema o la solución, no necesitas texto del cuerpo.
- No repitas el título en el mensaje con otras palabras ligeramente distintas.
- Comunicar claramente y concisa cuál es la solución.
- Céntrate en lo que da a los datos en primer lugar.
- No culpe a los usuarios para el error.
- Si hay un código de error asociado con el error y si crees que incluido el código de error puede ser útil el cliente o con el soporte técnico de Microsoft para investigar el problema, incluir directamente debajo del texto de cuerpo y lo escribe siguiente:

    Código de error: ###

    Si el cliente tiene toda la información necesaria para resolver el error sin el código, no es necesario para incluirla.

#### Botones

- Escribir el texto del botón para que sea una respuesta específica a la instrucción principal. Si no es posible, usa "Cerrar" para el texto del botón despido (en lugar de "Correcto" o "Listo").
- Si tienes más de un botón, hacer que el botón situado más a la acción que se recomienda al usuario realizar. Hacer que el botón situado más a la acción más conservador, por ejemplo, "Cancelar".

#### Vínculos de ayuda

Solo considera la posibilidad de vínculos de ayuda para mensajes de error que no puede hacer específico y usables.

## Texto de estado null

Aquí te mostramos algunos ayuda de la Guía de estilo de Windows.

Estado null se produce cuando los datos del cliente o el contenido esté ausente desde una aplicación o una característica, cuando no hay resultados se devuelven después de una búsqueda o cuando sea necesario información no está en un formulario, como información de una transacción de facturación.

### Instrucciones

 - Si es posible, usa las situaciones de estado null como una oportunidad para enseñar a las personas sobre cómo usar la característica (por ejemplo, cómo agregar música, donde a buscar imágenes, etcetera.)  
- Si tienes un título en la interfaz de usuario, se explica la acción para que realice para el estado "null" (por ejemplo, "Agregar música") "corregir" 
- Divertirse con el texto. Este espacio puede ser una oportunidad para proporcionar deleite dado que probablemente no se verán varias veces. 
- Evitar "Es Solitario aquí." Esto es sed y se abusa de ella. 
- Evitar preguntas como "No has conectado la impresora?" Bien usar una vez, pero este formato tiende a obtener abusa de ella, y preguntas Pon carga/presión adicional en el cliente. También puede sentir condescending. 
- Diversidad de texto de estado null es bueno. 

### Ejemplos

- "Agregar otra como favorito, y verás aquí".
- "¿Tiene los logros o clips de juegos que estás especialmente orgullosos del? Agregar a su showcase."
- Del "nadie en una parte aún. Inicie uno!"
- "Cuando alguien agrega como un amigo, podrás ver ellos aquí."
- "Al material como desbloquear logros, grabar clips de juegos y agregar a amigos, verás lo todo aquí."
- "Tus amigos favoritos se mostrarán aquí, para que puedas ver cuando estén en línea y lo que estén hasta."

## Puntuación

- Sin puntuación final (puntos, signos de interrogación) en encabezados u oraciones incompletas. Una excepción es un cuadro de diálogo de confirmación donde el encabezado formula la pregunta
- Usa las instrucciones de la Guía de estilo de Microsoft sobre [puntos](https://docs.microsoft.com/style-guide/punctuation/periods) y [signos de interrogación](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## Mensajes de estado

Los mensajes de estado constan de notificaciones y mensajes emergentes (notificación del sistema).

|Tipo de cadena         | Notas                               |
|------------        |-------------------------------------|
|Notificación del sistema               |Tipo oración con puntuación final: idealmente con una variable de objeto para que los usuarios puedan comprender a qué objeto se aplica el mensaje en caso de que hayan salido del objeto|
|Encabezado de notificación|Tipo de oración sin puntuación final (se trata de un encabezado): ideal con una variable de objeto|
|Detalles de notificación|Oraciones completas, idealmente con un vínculo a la interfaz de usuario que muestre el objeto|

A continuación presentamos algunas recomendaciones detalladas para mensajes de notificación:

|Tipo de cadena         | Notas                               |
|------------        |-------------------------------------|
|Iniciado             |Omítelo siempre que sea posible: normalmente puedes saltar al mensaje en curso para minimizar el número de distracciones.|
|En curso         |Empieza por el verbo de la acción que estés realizando y termina con puntos suspensivos para indicar una operación en curso. A continuación te mostramos un ejemplo:<br> *Creando el volumen "Datos del cliente"...*|
|Correcto             |Empieza con lo que el software acaba de hacer y termina con "correctamente". A continuación te mostramos un ejemplo:<br> *Volumen "Datos de clientes" creado correctamente.*|
|Error             |Empieza por "No se ha podido" y termina con lo que no ha podido hacer el software. A continuación te mostramos un ejemplo:<br> *No se ha podido crear el volumen "Datos de clientes".*|

## Información de herramientas

Una buena información sobre herramientas brevemente describe controles sin etiqueta o proporciona un poco de información adicional para los controles con etiquetas, cuando esto es útil. Puede también ayudan a los clientes navegar por la interfaz de usuario al ofrecer adicionales: no redundante: información acerca de las etiquetas de control, iconos, vínculos, etcetera.

Información sobre herramientas debe usarse con moderación o no en absoluto. Pueden ser una interrupción al cliente, por lo que no se incluye información sobre herramientas que simplemente se repite una etiqueta Estados o lo obvio. Siempre debe agregar información valiosa.

|    Contexto                                 |    Cómo escribir la información sobre herramientas    |
|    -----------------------                 |    -------------------------    |
|Cuando un control o elemento de interfaz de usuario está sin etiqueta …|Usa una frase nominal descriptiva y sencillo. Por ejemplo:<br> Resaltado de lápiz |
|Cuando la etiqueta de un elemento de interfaz de usuario, pero su propósito necesita aclaración …|<ul><li>Describe brevemente lo que puedes hacer con este elemento de interfaz de usuario. </li><li>Usa el formulario de verbo imperativo. Por ejemplo, "Buscar texto en este archivo" (no "busca texto en este archivo").</li><li>No incluyas la puntuación final, a menos que hay varios oraciones completas.</li> </ul>|
|Cuando una etiqueta de texto es probable que truncar en algunos idiomas o truncado …|<ul><li>Proporcionar la etiqueta untruncated en la información sobre herramientas.</li><li>Opcional: En línea otra, proporciona una descripción se clarifican, pero solo si es necesario.</li><li>No proporciona una información sobre herramientas si no se proporciona la información untruncated en otro lugar en la página o el flujo.</li></ul>|
|Si hay disponible un método abreviado de teclado …|<ul><li>Opcional: Proporciona el método abreviado de teclado entre paréntesis después de la etiqueta o frase descriptiva, por ejemplo, "Imprimir (Ctrl + P)" o "Buscar texto en este archivo (CTRL+f)"</li><li>Está bien para agregar un método abreviado de teclado útiles para información sobre herramientas se clarifican, pero evita agregar información sobre herramientas solo para mostrar un método abreviado de teclado. </li></ul>|