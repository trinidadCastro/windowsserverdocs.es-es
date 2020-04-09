---
title: Usar ETW para solucionar problemas de conexiones LDAP
description: Cómo activar y usar ETW para realizar un seguimiento de las conexiones LDAP entre AD DS controladores de dominio.
author: Teresa-Motiv
manager: dcscontentpm
ms.prod: windows-server-dev
ms.technology: active-directory-lightweight-directory-services
audience: Admin
ms.author: v-tea
ms.topic: article
ms.date: 11/22/2019
ms.openlocfilehash: f7b7df714dbd02b15555fa20c70c1e995e121a48
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822938"
---
# <a name="using-etw-to-troubleshoot-ldap-connections"></a>Usar ETW para solucionar problemas de conexiones LDAP

[Seguimiento de eventos para Windows (ETW)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) puede ser una valiosa herramienta para la solución de problemas de Active Directory Domain Services (AD DS). Puede usar ETW para realizar el seguimiento de las comunicaciones del Protocolo ligero de acceso a directorios ([LDAP](https://docs.microsoft.com/previous-versions/windows/desktop/ldap/lightweight-directory-access-protocol-ldap-api)) entre los clientes de Windows y los servidores LDAP, incluidos los controladores de dominio de AD DS.

## <a name="how-to-turn-on-etw-and-start-a-trace"></a>Cómo activar ETW e iniciar un seguimiento

**Para activar ETW**

1. Abra el editor del registro y cree la siguiente subclave del registro:

   **HKEY\_equipo LOCAL\_\\sistema\\CurrentControlSet\\Services\\ldap\\Tracing\\* Processname***

   En esta subclave, *processName* es el nombre completo del proceso del que se desea realizar un seguimiento, incluida su extensión (por ejemplo, "svchost. exe").

1. (**Opcional**) En esta subclave, cree una nueva entrada denominada **PID**. Para usar esta entrada, asigne un identificador de proceso como un valor DWORD.  

   Si especifica un identificador de proceso, ETW realiza un seguimiento solo de la instancia de la aplicación que tiene este identificador de proceso.

**Para iniciar una sesión de seguimiento**

- Abra una ventana del símbolo del sistema y ejecute el siguiente comando:

   ```cmd
   tracelog.exe -start <SessionName> -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f <FileName> -flag <TraceFlags>
   ```

   Los marcadores de posición de este comando representan los valores siguientes.

  - \<*nombresesión*> es un identificador arbitrario que se utiliza para etiquetar la sesión de seguimiento.  
  > [!NOTE]  
  > Tendrá que hacer referencia a este nombre de sesión más adelante al detener la sesión de seguimiento.
  - \<*nombre*de archivo > especifica el archivo de registro en el que se escribirán los eventos.
  - \<*TraceFlags*> debe ser uno o varios de los valores enumerados en la [tabla de marcas de seguimiento](#values-for-trace-flags).

## <a name="how-to-end-a-tracing-session-and-turn-off-event-tracing"></a>Cómo finalizar una sesión de seguimiento y desactivar el seguimiento de eventos

**Para detener el seguimiento**

- Escriba el siguiente comando en el símbolo del sistema:

   ```cmd
   tracelog.exe -stop <SessionName>
   ```

   En este comando, \<*nombresesión*> es el mismo nombre que usó en el comando **tracelog. exe-start** .

**Para desactivar ETW**

- En el editor del registro, elimine la subclave **HKEY\_LOCAL\_MACHINE\\System\\CurrentControlSet\\Services\\ldap\\Tracing\\* processName***.

## <a name="values-for-trace-flags"></a>Valores de marcas de seguimiento

Para usar una marca, sustituya el valor de marca del marcador de posición <*TraceFlags*> en los argumentos del comando **tracelog. exe-start** .

> [!NOTE]  
> Puede especificar varias marcas mediante la suma de los valores de marca adecuados. Por ejemplo, para especificar las marcas **debug\_Search** (0x00000001) y **Debug\_cache** (0x00000010), el valor adecuado \<*TraceFlags*> es **0x00000011**.

|Nombre del marcador |Valor de marca |Descripción de la marca |
| --- | --- | --- |
|**DEBUG_SEARCH** |0x00000001 |Registra las solicitudes de búsqueda y los parámetros que se pasan a esas solicitudes. Las respuestas no se registran aquí. Solo se registran las solicitudes de búsqueda. (Utilice **DEBUG_SPEWSEARCH** para registrar las respuestas a las solicitudes de búsqueda). |
|**DEBUG_WRITE** |0x00000002 |Registra las solicitudes de escritura y los parámetros que se pasan a esas solicitudes. Las solicitudes de escritura incluyen las operaciones de agregar, eliminar, modificar y extender. |
|**DEBUG_REFCNT** |0x00000004 |Registra los datos de recuento de referencias y las operaciones para las conexiones y las solicitudes. |
|**DEBUG_HEAP** |0x00000008 |Registra todas las asignaciones de memoria y las versiones de memoria. |
|**DEBUG_CACHE** |0x00000010 |Registra la actividad de caché. Esta actividad incluye adiciones, eliminaciones, aciertos, errores, etc. |
|**DEBUG_SSL** |0x00000020 |Registra la información y los errores de SSL. |
|**DEBUG_SPEWSEARCH** |0x00000040 |Registra todas las respuestas del servidor en las solicitudes de búsqueda. Estas respuestas incluyen los atributos solicitados, además de todos los datos que se recibieron. |
|**DEBUG_SERVERDOWN** |0x00000080 |Registra los errores de conexión y del servidor. |
|**DEBUG_CONNECT** |0x00000100 |Registra los datos relacionados con el establecimiento de una conexión.<br />Utilice **DEBUG_CONNECTION** para registrar otros datos relacionados con las conexiones. |
|**DEBUG_RECONNECT** |0x00000200 |Registra la actividad de reconexión automática. Esta actividad incluye intentos de reconexión, errores y errores relacionados. |
|**DEBUG_RECEIVEDATA** |0x00000400 |Registra la actividad relacionada con la recepción de mensajes desde el servidor. Esta actividad incluye eventos como "esperando la respuesta del servidor" y la respuesta recibida desde el servidor. |
|**DEBUG_BYTES_SENT** |0x00000800 |Registra todos los datos enviados por el cliente LDAP en el servidor. Esta función es esencialmente el registro de paquetes, pero siempre registra datos sin cifrar. (Si un paquete se envía a través de SSL, esta función registra el paquete no cifrado). Este registro puede ser detallado. Probablemente, esta marca se usa por sí misma o se combina con **DEBUG_BYTES_RECEIVED**. |
|**DEBUG_EOM** |0x00001000 |Registra los eventos que están relacionados con el fin de una lista de mensajes. Estos eventos incluyen información como "lista de mensajes desactivada", etc. |
|**DEBUG_BER** |0x00002000 |Registra las operaciones y los errores relacionados con las reglas de codificación básicas (BER). Estas operaciones y errores incluyen problemas en la codificación, problemas de tamaño de búfer, etc. |
|**DEBUG_OUTMEMORY** |0x00004000 |Registra errores para asignar memoria. También registra cualquier error para calcular la memoria necesaria (por ejemplo, un desbordamiento que se produce al calcular el tamaño de búfer necesario). |
|**DEBUG_CONTROLS** |0x00008000 |Registra los datos relacionados con los controles. Estos datos incluyen controles que se insertan, problemas que afectan a los controles, controles obligatorios en una conexión, etc. |
|**DEBUG_BYTES_RECEIVED** |0x00010000 |Registra todos los datos recibidos por el cliente LDAP. Este comportamiento es esencialmente el registro de paquetes, pero siempre registra datos sin cifrar. (Si un paquete se envía a través de SSL, esta opción registra el paquete no cifrado). Este tipo de registro puede ser detallado. Probablemente, esta marca se usa por sí misma o se combina con **DEBUG_BYTES_SENT**. |
|**DEBUG_CLDAP** |0x00020000 |Registra eventos que son específicos de UDP y LDAP sin conexión. |
|**DEBUG_FILTER** |0x00040000 |Registra los eventos y errores que se producen al construir un filtro de búsqueda.<br/>**Nota:** Esta opción registra eventos de cliente solo durante la construcción del filtro. No registra ninguna respuesta del servidor sobre un filtro. |
|**DEBUG_BIND** |0x00080000 |Registra los eventos y errores de enlace. Estos datos incluyen información de negociación, enlace correcto, error de enlace, etc. |
|**DEBUG_NETWORK_ERRORS** |0x00100000 |Registra errores generales de red. Estos datos incluyen errores de envío y recepción.<br/>**Nota:** Si se pierde una conexión o no se puede tener acceso al servidor, **DEBUG_SERVERDOWN** es la etiqueta preferida. |
|**DEBUG_VERBOSE** |0x00200000 |Registra mensajes generales. Use esta opción para cualquier mensaje que tiende a generar una gran cantidad de resultados. Por ejemplo, registra mensajes como "fin de mensaje alcanzado", "el servidor no ha respondido todavía", etc. Esta opción también es útil para los mensajes genéricos. |
|**DEBUG_PARSE** |0x00400000 |Registra eventos y errores de mensajes generales más los eventos y errores de análisis de paquetes y codificación. |
|**DEBUG_REFERRALS** |0x00800000 |Registra datos sobre las referencias y las referencias de seguimiento. |
|**DEBUG_REQUEST** |0x01000000 |Registra el seguimiento de las solicitudes. |
|**DEBUG_CONNECTION** |0x02000000 |Registra los datos de conexión generales y los errores. |
|**DEBUG_INIT_TERM** |0x04000000 |Registra la inicialización y limpieza del módulo (DLL Main, etc.). |
|**DEBUG_API_ERRORS** |0x08000000 |Admite el registro del uso incorrecto de la API. Por ejemplo, esta opción registra los datos si se llama dos veces a la operación de enlace en la misma conexión. |
|**DEBUG_ERRORS** |0x10000000 |Registra errores generales. La mayoría de estos errores se pueden clasificar como errores de inicialización de módulo, errores de SSL o errores de desbordamiento o subdesbordamiento. |
|**DEBUG_PERFORMANCE** |0x20000000 |Registra datos acerca de las estadísticas de actividad LDAP global del proceso después de recibir una respuesta del servidor para una solicitud LDAP. |

## <a name="example"></a>Ejemplo

Considere una aplicación, app1. exe, que establece contraseñas para las cuentas de usuario. Supongamos que app1. exe genera un error inesperado. Para usar ETW para ayudar a diagnosticar este problema, siga estos pasos:

1. En el editor del registro, cree la siguiente entrada del registro:

   **HKEY\_equipo LOCAL de\_\\sistema\\CurrentControlSet\\Services\\seguimiento de\\LDAP\\app1. exe**

1. Para iniciar una sesión de seguimiento, abra una ventana del símbolo del sistema y ejecute el siguiente comando:

   ```cmd
   tracelog.exe -start ldaptrace -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f .\ldap.etl -flag 0x80000
   ```

   Una vez que se inicia este comando, **DEBUG\_BIND** se asegura de que ETW escribe mensajes de seguimiento en.\\LDAP. ETL.

1. Inicie app1. exe y reproduzca el error inesperado.

1. Para detener la sesión de seguimiento, ejecute el siguiente comando en el símbolo del sistema:

   ```cmd
    tracelog.exe -stop ldaptrace
   ```

1. Para evitar que otros usuarios puedan hacer un seguimiento de la aplicación, elimine el seguimiento **HKEY\_LOCAL\_MACHINE**\\**System**\\**CurrentControlSet**\\**Services**\\**LDAP**\\**Tracing**\\la entrada del registro **app1. exe** .

1. Para revisar la información del registro de seguimiento, ejecute el siguiente comando en el símbolo del sistema:

   ```cmd
    tracerpt.exe .\ldap.etl -o -report
    ```

   > [!NOTE]  
   > En este comando, **tracerpt. exe** es una herramienta de [consumidor de seguimiento](https://go.microsoft.com/fwlink/p/?linkid=83876) .
