---
title: Guía de estilo de texto y diseño de la interfaz de usuario de Windows Admin Center
description: Interfaz de usuario de Windows Admin Center texto y diseño de guía de estilo SDK
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: be41267d6584002ebf87e5fe828a41575d305e1b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445912"
---
# <a name="windows-admin-center-ui-text-and-design-style-guide"></a>Guía de estilo de texto y diseño de la interfaz de usuario de Windows Admin Center

>Se aplica a: Windows Admin Center

En este tema se describe el método general para escribir el texto de la interfaz de usuario (IU) de Windows Admin Center, así como algunas convenciones específicas y soluciones que estamos aportando.

Windows Admin Center y las extensiones deben seguir los [principios de voz de Microsoft](https://docs.microsoft.com/style-guide/brand-voice-above-all-simple-human) para que la experiencia sea descriptiva y fácil de usar. Esta guía de estilo se basa en estos principios de voz, así como en la [Guía de estilo de escritura de Microsoft](https://docs.microsoft.com/style-guide/welcome/), por tanto asegúrate de desproteger estos recursos para obtener información sobre elementos como [accesibilidad](https://docs.microsoft.com/style-guide/accessibility/accessibility-guidelines-requirements), [acrónimos](https://docs.microsoft.com/style-guide/acronyms) y la [elección de palabras](https://docs.microsoft.com/style-guide/word-choice/) como [por favor](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/p/please)y [lo sentimos](https://docs.microsoft.com/style-guide/a-z-word-list-term-collections/s/sorry).

## <a name="buttons"></a>Botones

- Los botones deben ser una palabra siempre que sea posible, especialmente si tienes previsto localizar tu herramienta. Dos o tres son correcto, pero evite ya. Si tiene cuatro palabras o más, sería mejor usar un control de vínculos.
- Las etiquetas de los botones deben ser concisas, específicas y autoexplicativas. En lugar de un botón genérico "Enviar", usa un verbo que corresponda a la acción del usuario, como "Crear", "Eliminar", "Agregar", "Formatear", etc.
- Si un botón sigue a una pregunta, su etiqueta debe corresponder con claridad a la pregunta (normalmente "Sí" o "No").

## <a name="capitalization"></a>Uso de mayúsculas

Seguimos el estilo de Microsoft para el [uso de mayúsculas](https://docs.microsoft.com/style-guide/capitalization): uso de mayúsculas con estilo de oración para casi todo.

| Elemento de la interfaz de usuario              |Uso de mayúsculas|Observaciones|
|-------------------------|--------------|--------|
|Distintivos (como VISTA PREVIA) |Todo en mayúsculas      ||
|Todo lo demás          |Estilo de frase|Sin embargo, hay algunas excepciones donde nos encontramos con propiedades de objetos de WMI o PowerShell que se encuentran fuera de nuestro control.|

## <a name="colons"></a>Dos puntos

Utilice signos de dos puntos para presentar las listas. Por ejemplo:

    Choose one of the following:
    Cats
    Dogs
    Quokkas

No use signos de dos puntos en el texto de la interfaz de usuario cuando una etiqueta está en otra línea de lo que las etiquetas o cuando hay una distinción clara entre la etiqueta y lo es el etiquetado.

Utilice signos de dos puntos en el texto de la interfaz de usuario cuando es una etiqueta en la misma línea que el texto las etiquetas y necesita impedir que se ejecuten conjuntamente los dos elementos.

## <a name="confirmation-messages"></a>Mensajes de confirmación

Los cuadros de diálogo de confirmación son útiles cuando continúe podría tener resultados inesperados, como la pérdida de datos. Deben contener información útil, pueden analizarse con un resultado claro, especialmente para los eventos que no se puede deshacer. 

- Asegúrese de que es necesaria una confirmación. Si no hay ninguna nueva información de la oferta (por ejemplo, "¿está seguro?"), a continuación, un mensaje de confirmación no es posible que sea necesario.  
- Compruebe que el cliente desea continuar con la acción.
- Asegúrese de que la instrucción principal (encabezado) y el texto explicativo (cuerpo) no son redundantes.
- En el encabezado, defina los posibles resultados como una pregunta o una instrucción sobre lo que sucederá junto. ¿Por ejemplo, "Borrar todos los datos en esta unidad? o "Está a punto de borrar todos los datos".
- Agregar detalles en el cuerpo. Si hay una variable, como el nombre del elemento que está sobre el cambio, incluirlo aquí.
- Incluir una pregunta sencilla (ya sea en el encabezado o en el cuerpo) que rodea a una opción clara entre los dos botones de acción.
- Para una compleja, use Sí/No botones, lo que recomendamos leer cuidado. Para una opción más sencilla, use los botones que son específicas de la acción, como eliminar todos o Cancelar.

## <a name="first-run-experiences"></a>Experiencias de primera ejecución

Cuando un usuario visita por primera vez una página, tienes la oportunidad de ayudarle a empezar a trabajar con la herramienta. Esto podría ser:

- Una cadena de texto en una página en blanco con instrucciones breves sobre cómo empezar a trabajar, por ejemplo, "Selecciona 'Agregar' para agregar una aplicación".
- Un vínculo al control que permite al usuario comenzar, por ejemplo, "Agregar una aplicación para empezar".
- Una animación o un vídeo pequeño o breve que muestre al usuario cómo empezar a trabajar

Estas son algunas sugerencias de nuestra guía de estilo de Windows:

### <a name="1-be-helpful"></a>1. Se útil

- Evita lenguaje y estilo de marketing.
- Cuando hagas una demostración o sugieras algo, asegúrate de que el resultado final sea claro; mostrar únicamente al cliente cómo hacer algo no es eficaz si no saben por qué lo están haciendo.
- No presentes sugerencias si el cliente no las necesita.

### <a name="2-show-dont-tell"></a>2. Mostrar, no nos

Mantén un texto lo más simple posible (piensa en animaciones o vídeos pequeños).

### <a name="3-dont-overwhelm"></a>3. No se sobrecargará

- Limita los elementos emergentes y sugerencias a 4 por sesión de uso combinado, incluidas las notificaciones del sistema y las notificaciones del shell.
- Asegúrate de que la sincronización de los elementos emergentes sea útil.
- No impidas que el cliente haga algo.
- Asegúrate de que los elementos emergentes se cierren fácilmente.

### <a name="4-keep-it-contextual"></a>4. Mantener contextuales

- Los momentos didácticos son más eficaces cuando se presentan en el momento adecuado.
- Si creas tutoriales o presentaciones, la información debe ser concreta.
- Evita adornos de marketing, céntrate en consejos y sugerencias específicos.
- Proporciona una forma para que los clientes vuelvan al tutorial más adelante, si es relevante (las personas a menudo no retienen la información la primera vez, pero las instrucciones de configuración podrían no ser relevantes en alguna ocasión).
- La mensajería con estado vacío es un lugar natural para aprender y/o deleitar, haz que sean simples e informativos.

### <a name="5-minimize-painful-setup"></a>5. Minimizar complicado programa de instalación

Cuando necesites que el cliente realice otra acción para experimentar el valor completo (iniciar sesión en un servicio en línea, etc.), hazlo de la forma más sencilla posible.

- La mensajería debe ser breve y directa.
- Evita enviarlos a otra ubicación. Si es posible, proporciona un medio para conectarse desde donde estén.
- Si es posible, posibilita la opción de hacerlo más adelante y, a continuación, recuérdales la posibilidad de hacerlo más adelante.
- Si los sacas de su experiencia, proporciona una forma de volver de forma rápida y sencilla.

## <a name="help-links"></a>Vínculos de ayuda

Estas son algunas sugerencias de nuestra guía de estilo de Windows:

### <a name="when-should-we-provide-a-help-link"></a>¿Debemos cuando se proporciona un vínculo de ayuda?

Casi nunca. Proporcionar un vínculo de ayuda solo cuando:

- Hay una pregunta obvia e importante que los clientes suelen tener mientras están en la interfaz de usuario de la respuesta a la que le ayudará a tener éxito en la tarea de la interfaz de usuario. 
- No hay espacio suficiente en la interfaz de usuario para proporcionar la cantidad de información necesaria para los usuarios a realizar correctamente la tarea de la interfaz de usuario. 

### <a name="where-should-help-links-appear"></a>¿Donde debe vínculos de ayuda aparecen? 

- Vínculos de texto deben aparecer lo más cerca del elemento de interfaz de usuario que se dirige la Ayuda como sea posible. 
- Si se debe proporcionar un vínculo de texto que se aplica a una pantalla completa de la interfaz de usuario, colóquelo en la parte inferior izquierda de la pantalla. 
- Si proporciona un vínculo a través de un botón de ayuda (?), la información sobre herramientas debe ser "Help".

### <a name="what-url-should-we-use"></a>¿Qué dirección URL debemos usar?

Nunca un vínculo directo a una dirección web, en su lugar, use un servicio de redireccionamiento.

Los desarrolladores de Microsoft deben usar un vínculo redireccionable excepto cuando es un vínculo de ayuda que los usuarios podrían tener que escribir manualmente, en cuyo caso, utilice un vínculo aka.ms (siempre que el destino de la dirección URL es un sitio Web que reconoce automáticamente la configuración regional del explorador, como Docs.microsoft.com)

### <a name="text-guidelines"></a>Directrices de texto 

- Use oraciones completas.
- No incluya puntuación, excepto los signos de interrogación de cierre. 
- No es necesario usar el mismo texto como el título de la tarea; Use el texto que tenga sentido en el contexto de la interfaz de usuario, pero asegúrese de que hay una conexión lógica entre los dos. Por ejemplo: 
- Vínculo de ayuda: ¿Qué es el riesgo de permitir que las excepciones? 
- Título del tema de ayuda: "Permitir que un programa se comunique a través de Firewall de Windows"
- Sea lo más específico posible sobre el contenido del tema de ayuda. 
    - Nuestro estilo
        - ¿Cómo ayuda Windows Firewall proteger mi equipo?
        - ¿Por qué aspectos destacados pueden mejorar una imagen
    - No es nuestro estilo
        - Para obtener más información acerca de Firewall de Windows
        - Más información sobre la administración del color
        - Obtén más información
- Utilice la frase completa para el texto del vínculo, no solo las palabras clave. 
    - Nuestro estilo 
        - [¿Qué es el riesgo de permitir que las excepciones?]()
    - No es nuestro estilo
        - ¿Cuáles son los [riesgo de permitir que las excepciones]()? 

## <a name="error-messages"></a>Mensajes de error

Aquí tiene algunas instrucciones adaptado de la Guía de estilo de Windows:

Escribir un mensaje buena es un equilibrio entre proporcionar suficiente explicación pero no eran demasiado técnicos; entre los que se va a ocasionales y personal, pero no resultan molestas u ofensivas.

### <a name="general-guidelines"></a>Directrices generales

Use un mensaje por caso de error.

#### <a name="headings"></a>Encabezados

- Sea breve y concisa explicar cuál es el problema o **lo ideal es que lo que debe hacer**. <br>Algunas superficies de la interfaz de usuario pueden tener encabezados que truncar en lugar de encapsular cuando sea demasiado largos, así que esté atento para estos.
- Usar la solución en el encabezado si es un simple paso.
- Asegúrese de que el encabezado se relaciona directamente con el botón en caso de que el lector omite el texto del cuerpo.
- Evite el uso de "Se produjo un problema" en los encabezados, a menos que no tenga ninguna otra alternativa. Ser más específico sobre el problema.
- Evite el uso de variables (como nombres de archivo, carpeta y aplicación) en los encabezados. Colóquelos en el cuerpo.

#### <a name="body"></a>Cuerpo

- Si el encabezado suficientemente explica el problema o la solución, no es necesario el texto de cuerpo.
- No repetir el título en el mensaje con una redacción ligeramente diferente.
- Comunicarse de forma clara y concisa cuál es la solución.
- Centrarse en proporcionar a los hechos en primer lugar.
- No culpe a los usuarios para el error.
- Si hay un código de error asociado con el error y si piensa incluyendo el código de error puede ser útil que el cliente o el soporte técnico de Microsoft para investigar el problema, incluirlo directamente debajo del texto de cuerpo y escribirlo como sigue:

    Código de error: ###

    Si el cliente tiene toda la información necesaria para resolver el error sin el código, no es necesario incluirlo.

#### <a name="buttons"></a>Botones

- Escribir el texto del botón para que sea una respuesta específica a la instrucción principal. Si no es posible, use "Cerrar" para el texto del botón despido (en lugar de "Correcto" o "Listo").
- Si tiene más de un botón, realice el botón situado más a la acción que se anima al usuario tomar. Realice el botón situado más a la acción más conservador, por ejemplo, "Cancelar".

#### <a name="help-links"></a>Vínculos de ayuda

Solo tenga en cuenta los vínculos de ayuda para los mensajes de error que no pueden realizar específicos y procesables.

## <a name="null-state-text"></a>Texto de estado null

Aquí es algo de Ayuda de la Guía de estilo Windows.

Estado null se produce cuando los datos del cliente o el contenido no está presente de una aplicación o característica, cuando se devuelve ningún resultado después de una búsqueda, o cuando sea necesario falta información de un formulario, como información de facturación para una transacción.

### <a name="guidelines"></a>Instrucciones

- Si es posible, utilice situaciones del estado null como una oportunidad para informar a las personas sobre cómo usar la característica (por ejemplo, cómo agregar música, donde para buscar la fotografías, etcetera.)  
  - Si tiene un título en la interfaz de usuario, se explica la acción para "corregir" el estado null (por ejemplo, "Agregar música") 
  - Diviértase con el texto. Este espacio puede ser una oportunidad para que se proporcionan los usuarios, ya que probablemente no se verán varias veces. 
  - Evitar "Es nada aquí." Es triste y ha sido sobreutilizado. 
  - Evitar preguntas como "No ha conectado la impresora?" Correcto para utilizar una vez, pero este formato tiende a obtener sobreutilizados y preguntas suponer una presión/carga adicional en el cliente. También puede parecer condescendiente. 
  - Diversos en texto de estado null es una buena opción. 

### <a name="examples"></a>Ejemplos

- "Agregar a alguien como favorito y podrá verlos aquí".
- "¿Tiene los logros o clips juego está especialmente me siento orgulloso de? Agréguelos a la presentación."
- "Nadie en una entidad aún. Inicie uno!"
- "Cuando alguien le agrega como un amigo, verá aquí."
- "Cuando cosas como desbloquear logros, grabar clips de juego y agregar a amigos, lo verá todas aquí."
- "Sus amigos favoritos aparecerán aquí, para que pueda ver cuándo están conectados y lo que les hasta."

## <a name="punctuation"></a>Puntuación

- Sin puntuación final (puntos, signos de interrogación) en encabezados u oraciones incompletas. Una excepción es un cuadro de diálogo de confirmación donde el encabezado formula la pregunta
- Usa las instrucciones de la Guía de estilo de Microsoft sobre [puntos](https://docs.microsoft.com/style-guide/punctuation/periods) y [signos de interrogación](https://docs.microsoft.com/style-guide/punctuation/question-marks).

## <a name="status-messages"></a>Mensajes de estado

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
|En curso         |Empieza por el verbo de la acción que estés realizando y termina con puntos suspensivos para indicar una operación en curso. Por ejemplo:<br> *Crear el volumen "Datos del cliente"...*|
|Correcto             |Empieza con lo que el software acaba de hacer y termina con "correctamente". Por ejemplo:<br> *Se creó correctamente el volumen "Datos del cliente".*|
|Error             |Empieza por "No se ha podido" y termina con lo que no ha podido hacer el software. Por ejemplo:<br> *No se pudo crear el volumen "Datos del cliente".*|

## <a name="tooltips"></a>Información sobre herramientas

Buena información sobre herramientas brevemente describe controles sin etiqueta o proporciona un poco de información adicional para los controles con la etiqueta, cuando esto es útil. Puede también ayudan a los clientes navegar por la interfaz de usuario, ya que ofrece adicionales: no redundantes, información acerca de las etiquetas de control, iconos, vínculos, etcetera.

Información sobre herramientas debe usarse con moderación o no admitirlo. Pueden ser una interrupción en el cliente, por lo que no incluyen información sobre herramientas que simplemente se repite una etiqueta o Estados lo obvio. Siempre debe agregar información valiosa.

|    Contexto                                 |    Cómo escribir la información sobre herramientas    |
|    -----------------------                 |    -------------------------    |
|Cuando un control o elemento de interfaz de usuario es sin etiqueta...|Utilice una frase simple y descriptivo. Por ejemplo:<br> Resaltado de lápiz |
|Cuando un elemento de interfaz de usuario está etiquetado, pero su propósito necesita aclaración...|<ul><li>Describa brevemente lo que puede hacer con este elemento de interfaz de usuario. </li><li>Use el formulario de verbo imperativo. Por ejemplo, "Buscar texto en este archivo" (no "busca texto en este archivo").</li><li>No incluya puntuación final, a menos que haya varias oraciones completas.</li> </ul>|
|Cuando una etiqueta de texto está truncada o el probable truncar en algunos idiomas...|<ul><li>Proporcione la etiqueta sin truncar en la información sobre herramientas.</li><li>Opcional: En la siguiente línea, proporcione una descripción esclarecedora, pero solo si es necesario.</li><li>No se proporciona información sobre herramientas si no se proporciona la información sin truncar en otro lugar en la página o el flujo.</li></ul>|
|Si hay un método abreviado de teclado...|<ul><li>Opcional: Proporcione el método abreviado de teclado entre paréntesis después de la etiqueta o la frase descriptiva, p. ej. "Print" (Ctrl + P)"o"Buscar texto en este archivo (Ctrl + F)"</li><li>Está bien para agregar un método abreviado de teclado útiles para una información sobre herramientas esclarecedora, pero evitar agregar información sobre herramientas solo para mostrar un método abreviado de teclado. </li></ul>|