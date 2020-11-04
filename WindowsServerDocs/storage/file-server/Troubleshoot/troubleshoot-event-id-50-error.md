---
title: Solución de problemas del mensaje de error 50 de ID. de evento
description: Describe cómo solucionar problemas del mensaje de error de ID. de evento 50
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 930a503f756bf2005e26ac227565b42b627ee11a
ms.sourcegitcommit: 39d55b5a0006aceac6281e8cdb61fc79a209ce1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328135"
---
# <a name="troubleshoot-the-event-id-50-error-message"></a>Solución de problemas del mensaje de error 50 de ID. de evento

##  <a name="symptoms"></a>Síntomas

Cuando se escribe información en el disco físico, se pueden registrar los dos mensajes de eventos siguientes en el registro de eventos del sistema: 

```
Event ID: 50 
Event Type: Warning 
Event Source: Ftdisk 
Description: {Lost Delayed-Write Data} The system was attempting to transfer file data from buffers to \Device\HarddiskVolume4. The write operation failed, and only some of the data may have been written to the file.
Data: 
0000: 00 00 04 00 02 00 56 00 
0008: 00 00 00 00 32 00 04 80 
0010: 00 00 00 00 00 00 00 00 
0018: 00 00 00 00 00 00 00 00 
0020: 00 00 00 00 00 00 00 00 
0028: 11 00 00 80 
```

```
Event ID: 26 
Event Type: Information
Event Source: Application Popup
Description: Windows - Delayed Write Failed : Windows was unable to save all the data for the file \Device\HarddiskVolume4\Program Files\Microsoft SQL Server\MSSQL$INSTANCETWO\LOG\ERRORLOG. The data has been lost. This error may be caused by a failure of your computer hardware or network connection.

Please try to save this file elsewhere.
```

Estos mensajes de ID. de evento significan exactamente lo mismo y se generan por las mismas razones. Para los fines de este artículo, solo se describe el mensaje ID 50.

> [!NOTE] 
> El dispositivo y la ruta de acceso de la descripción y los datos hexadecimales específicos variarán. 

##  <a name="more-information"></a>Más información

Se registra un mensaje con el identificador de evento 50 si se produce un error genérico cuando Windows está intentando escribir información en el disco. Este error se produce cuando Windows intenta confirmar los datos del administrador de caché del sistema de archivos (no de la memoria caché de nivel de hardware) en el disco físico. Este comportamiento forma parte de la administración de memoria de Windows. Por ejemplo, si un programa envía una solicitud de escritura, el administrador de caché almacena la solicitud de escritura y se indica al programa que la escritura se ha completado correctamente. En un momento posterior en el tiempo, el administrador de caché intenta escribir los datos en el disco físico. Cuando el administrador de caché intenta confirmar los datos en el disco, se produce un error al escribir los datos y los datos se vacían de la caché y se descartan. El almacenamiento en caché con reescritura mejora el rendimiento del sistema, pero se puede producir pérdida de datos y pérdida de integridad del volumen como resultado de la pérdida de errores de escritura diferida.

Es importante recordar que no todas las operaciones de e/s están almacenadas en búfer por el administrador de caché. Los programas pueden establecer una marca de FILE_FLAG_NO_BUFFERING que omita el administrador de caché. Cuando SQL realiza escrituras críticas en una base de datos, esta marca se establece para garantizar que la transacción se completa directamente en el disco. Por ejemplo, las escrituras no críticas en los archivos de registro realizan operaciones de e/s almacenadas en búfer para mejorar el rendimiento general. Un mensaje con el identificador de evento 50 nunca es el resultado de una e/s no almacenada en búfer.

Hay varios orígenes diferentes para un mensaje con el identificador de evento 50. Por ejemplo, si hay un problema de conectividad de red con el redirector, se produce un mensaje con el identificador de evento 50 registrado en un origen MRxSmb. Para evitar que se realicen pasos de solución de problemas incorrectos, asegúrese de revisar el mensaje de ID. 50 para confirmar que hace referencia a un problema de e/s de disco y que se aplica este artículo.

Un mensaje con el identificador de evento 50 es similar a un identificador de evento 9 y un mensaje con el identificador de evento 11. Aunque el error no es tan grave como el error indicado por el ID. de evento 9 y un mensaje de ID. de evento 11, puede usar las mismas técnicas de solución de problemas para un mensaje con el identificador de evento 50 como lo hace para un identificador de evento 9 y un mensaje de identificador de evento 11. Sin embargo, recuerde que cualquier elemento de la pila puede producir escrituras de retraso perdido, como controladores de filtro y controladores de puertos pequeños. 

Puede usar los datos binarios asociados a cualquier error de "disco" adjunto (indicado por un mensaje de error de ID. de evento 9, 11, 51 u otros mensajes) para ayudarle a identificar el problema.

###  <a name="how-to-decode-the-data-section-of-an-event-id-50-event-message"></a>Cómo descodificar la sección de datos de un mensaje de evento de ID. de evento 50 

Al descodificar la sección de datos del ejemplo de un mensaje con el identificador de evento 50 que se incluye en la sección "Resumen", se muestra que se produjo un error al intentar realizar una operación de escritura porque el dispositivo estaba ocupado y los datos se perdieron. En esta sección se describe cómo descodificar este ID. de evento 50. 

En la tabla siguiente se describe lo que representa cada desplazamiento de este mensaje: 

|OffsetLengthValues|Length|Valores|
|-----------|------------|---------|
|0x00|2|No se utiliza|
|0x02|2|Tamaño de los datos de volcado = 0x0004|
|0x04|2|Número de cadenas = 0x0002|
|0x06|2|Desplazamiento a las cadenas|
|0x08|2|Categoría de eventos|
|0x0c|4|Código de error NTSTATUS = 0x80040032 = IO_LOST_DELAYED_WRITE|
|0x10|8|No se utiliza|
|0x18|8|No se utiliza|
|0x20|8|No se utiliza|
|0x28|4|Código de error de estado de NT|

#### <a name="key-sections-to-decode"></a>Secciones clave para descodificar

**Código de error**

En el ejemplo de la sección "Resumen", el código de error se muestra en la segunda línea. Esta línea comienza con "0008:" e incluye los cuatro últimos bytes en esta línea: 0008:00 00 00 00 32 00 04 80 en este caso, el código de error es 0x80040032. El código siguiente es el código del error 50 y es el mismo para todos los mensajes de ID. de evento 50: IO_LOST_DELAYED_WRITEWARNINGNote al convertir los datos hexadecimales del mensaje de ID. de evento en el código de estado, recuerde que los valores se representan en el formato Little-Endian.

**El disco de destino**

Puede identificar el disco al que se estaba intentando escribir mediante el vínculo simbólico que aparece en la unidad en la sección "Descripción" del mensaje de ID. de evento, por ejemplo: \Device\HarddiskVolume4.

**Código de estado final**

El código de estado final es la parte más importante de la información en un mensaje con el identificador de evento 50. Este es el código de error que se devuelve cuando se realizó la solicitud de e/s y es el origen de la clave de la información. En el ejemplo de la sección "Resumen", el código de estado final se muestra en 0x28, la sexta línea, que comienza con "0028:" e incluye los únicos cuatro octetos en esta línea: 

```
0028: 11 00 00 80 
```

En este caso, el estado final es igual a 0x80000011. Este código de estado se asigna a STATUS_DEVICE_BUSY e implica que el dispositivo está ocupado actualmente.

> [!NOTE] 
> Cuando convierta los datos hexadecimales en el mensaje con el identificador de evento 50 en el código de estado, recuerde que los valores se representan en el formato Little-Endian. Dado que el código de estado es el único elemento de información que le interesa, puede ser más fácil ver los datos en formato de palabras en lugar de en BYTEs. Si lo hace, los bytes tendrán el formato correcto y es posible que los datos sean más fáciles de interpretar rápidamente.

Para ello, haga clic en **palabras** en la ventana **propiedades del evento** . En la vista de palabras de datos, el ejemplo de la sección "síntomas" se leerá de la siguiente manera: Data: 

```
() Bytes (.) 
Words 0000: 00040000 00560002 00000000 80040032 0010: 00000000 00000000 00000000 00000000 0020: 00000000 00000000 80000011
```

Para obtener una lista de códigos de estado de Windows NT, vea NTSTATUS. H en el kit para desarrolladores de software (SDK) de Windows.
