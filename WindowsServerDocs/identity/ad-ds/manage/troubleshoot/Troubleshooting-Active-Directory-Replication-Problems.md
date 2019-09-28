---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Solución de problemas de replicación de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf6b50ab3b4991bd8cab8523494261f1284945a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409062"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Solución de problemas de replicación de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory problemas de replicación pueden tener distintos orígenes. Por ejemplo, los problemas del sistema de nombres de dominio (DNS), los problemas de red o los problemas de seguridad pueden provocar un error en la replicación Active Directory. 

En el resto de este tema se explican las herramientas y una metodología general para corregir Active Directory errores de replicación. En los temas siguientes se tratan los síntomas, las causas y cómo resolver errores de replicación específicos:

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introducción y recursos para la solución de problemas de replicación Active Directory

El error de replicación de entrada o de salida hace que Active Directory objetos que representan la topología de replicación, la programación de replicación, los controladores de dominio, los usuarios, los equipos, las contraseñas, los grupos de seguridad, las pertenencias a grupos y directiva de grupo sean incoherentes. entre controladores de dominio. La incoherencia del directorio y el error de replicación causan errores operativos o resultados incoherentes, según el controlador de dominio con el que se haya establecido contacto para la operación, y pueden impedir la aplicación de los permisos de directiva de grupo y control de acceso. Active Directory Domain Services (AD DS) depende de la conectividad de red, la resolución de nombres, la autenticación y la autorización, la base de datos de directorio, la topología de replicación y el motor de replicación. Cuando la causa principal de un problema de replicación no es evidente de inmediato, la determinación de la causa entre las diversas causas posibles requiere la eliminación sistemática de las causas probables.

Para obtener una herramienta basada en la interfaz de usuario que le ayude a supervisar la replicación y a diagnosticar errores, consulte [Active Directory Replication Status Tool](https://www.microsoft.com/download/details.aspx?id=30005)

Para obtener un documento completo en el que se describe cómo puede usar la herramienta repadmin para solucionar problemas de Active Directory la replicación está disponible; consulte [supervisión y solución de problemas de la replicación de Active Directory mediante repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Para obtener información sobre cómo funciona Active Directory la replicación, consulte las siguientes referencias técnicas:

- [Referencia técnica del modelo de replicación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Referencia técnica de la topología de replicación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Recomendaciones de soluciones de eventos y herramientas

Idealmente, los eventos rojo (error) y amarillo (ADVERTENCIA) del registro de eventos del servicio de directorio sugieren la restricción específica que está causando un error de replicación en el controlador de dominio de origen o de destino. Si el mensaje de evento sugiere pasos para una solución, siga los pasos que se describen en el evento. La herramienta repadmin y otras herramientas de diagnóstico también proporcionan información que puede ayudarle a resolver errores de replicación. 

Para obtener información detallada sobre el uso de repadmin para solucionar problemas de replicación, consulte [supervisión y solución de problemas de la replicación de Active Directory mediante repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Resolución de interrupciones intencionales o errores de hardware

A veces se producen errores de replicación debido a interrupciones intencionadas. Por ejemplo, al solucionar problemas de Active Directory de replicación, debe descartar las desconexiones intencionales y los errores de hardware o actualizaciones en primer lugar.

### <a name="intentional-disconnections"></a>Desconexiones intencionadas

Si un controlador de dominio que está intentando realizar la replicación con un controlador de dominio que se ha compilado en un sitio de ensayo ha generado errores de replicación y actualmente está sin conexión esperando su implementación en el sitio de producción final (un sitio remoto, como una sucursal) ), puede tener en cuenta los errores de replicación. Para evitar separar un controlador de dominio de la topología de replicación durante períodos prolongados, lo que provoca errores continuos hasta que el controlador de dominio se vuelve a conectar, considere la posibilidad de agregar estos equipos inicialmente como servidores miembro y mediante la instalación desde el medio ( IFM) para instalar Active Directory Domain Services (AD DS). Puede usar la herramienta de línea de comandos Ntdsutil para crear medios de instalación que puede almacenar en medios extraíbles (CD, DVD u otros medios) y enviarlos al sitio de destino. A continuación, puede usar el medio de instalación de para instalar AD DS en los controladores de dominio en el sitio, sin usar la replicación. 

### <a name="hardware-failures-or-upgradestitle"></a>Errores de hardware o actualizaciones @ no__t-0

Si se producen problemas de replicación como resultado de un error de hardware (por ejemplo, un error de una placa base, un subsistema de disco o una unidad de disco duro), notifique al propietario del servidor para que se pueda resolver el problema de hardware.

Las actualizaciones periódicas de hardware también pueden hacer que los controladores de dominio estén fuera de servicio. Asegúrese de que los propietarios del servidor tengan un buen sistema de comunicación de tales interrupciones de antemano.

### <a name="firewall-configuration"></a>Configuración de firewall

De forma predeterminada, Active Directory las llamadas a procedimiento remoto (RPC) de replicación se producen de forma dinámica a través de un puerto disponible a través del asignador de extremos de RPC (RPCSS) en el Puerto 135. Asegúrese de que Firewall de Windows con seguridad avanzada y otros firewalls estén configurados correctamente para permitir la replicación. Para obtener información acerca de cómo especificar el puerto para Active Directory la configuración de la replicación y el puerto, consulte [el artículo 224196 en Microsoft Knowledge Base](https://go.microsoft.com/fwlink/?LinkId=22578). 

Para obtener información sobre los puertos que usa la replicación de Active Directory, vea [Active Directory herramientas y configuración de replicación](https://go.microsoft.com/fwlink/?LinkId=123774).

Para obtener información acerca de cómo administrar la replicación de Active Directory a través de firewalls, consulte Active Directory de la [replicación a través de firewalls](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Respuesta a un error de un servidor obsoleto que ejecuta Windows 2000 Server

Si un controlador de dominio que ejecuta Windows 2000 Server ha producido un error durante más tiempo que el número de días de la duración de los marcadores de exclusión, la solución es siempre la misma: 

1. Mueva el servidor de la red corporativa a una red privada.
2. Quite forzosamente Active Directory o vuelva a instalar el sistema operativo.
3. Quite los metadatos del servidor de Active Directory para que el objeto de servidor no se pueda reactivar. 

Puede usar un script para limpiar los metadatos del servidor en la mayoría de los sistemas operativos Windows. Para obtener información sobre el uso de este script, vea [quitar metadatos del controlador de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkID=123599). 

De forma predeterminada, los objetos de configuración NTDS que se eliminan se reactivan automáticamente durante un período de 14 días. Por lo tanto, si no quita los metadatos del servidor (use Ntdsutil o el script mencionado previamente para realizar la limpieza de los metadatos), los metadatos del servidor se restablecen en el directorio, lo que solicita que se produzcan los intentos de replicación. En este caso, los errores se registrarán de forma persistente como resultado de la incapacidad de replicar con el controlador de dominio que falta.

## <a name="root-causes"></a>Causas raíz

Si se descartan desconexiones intencionadas, errores de hardware y controladores de dominio de Windows 2000 anticuados, el resto de los problemas de replicación casi siempre tienen una de las causas raíz siguientes:

- Conectividad de red: Es posible que la conexión de red no esté disponible o que la configuración de red no esté correctamente configurada.
- Resolución de nombres: Las configuraciones erróneas de DNS son una causa común de los errores de replicación.
- Autenticación y autorización: Los problemas de autenticación y autorización causan errores de "acceso denegado" cuando un controlador de dominio intenta conectarse a su asociado de replicación.
- Base de datos de directorio (almacén): Es posible que la base de datos de directorio no pueda procesar transacciones lo suficientemente rápido como para mantenerse al día de los tiempos de espera de replicación.
- Motor de replicación: Si las programaciones de replicación entre sitios son demasiado cortas, es posible que las colas de replicación sean demasiado grandes para procesarlas en el tiempo necesario para la programación de la replicación de salida. En este caso, la replicación de algunos cambios puede detenerse indefinidamente potencialmente, lo suficientemente largo como para superar la duración del desecho.
- Topología de replicación: Los controladores de dominio deben tener vínculos entre sitios en AD DS que se asignen a conexiones de red de área extensa (WAN) o de red privada virtual (VPN) reales. Si crea objetos en AD DS para la topología de replicación que no son compatibles con la topología de sitio real de la red, se produce un error en la replicación que requiere la topología mal configurada.

## <a name="general-approach-to-fixing-problems"></a>Enfoque general para solucionar problemas

Use el siguiente enfoque general para solucionar problemas de replicación: 

1. Supervise el estado de la replicación diariamente o use repadmin. exe para recuperar el estado de replicación diariamente.
2. Intente resolver cualquier error informado de manera oportuna mediante los métodos que se describen en los mensajes de eventos y en esta guía. Si el software puede estar causando el problema, desinstale el software antes de continuar con otras soluciones.
3. Si algún método conocido no puede resolver el problema que está causando errores en la replicación, quite AD DS del servidor y, a continuación, vuelva a instalar AD DS. Para obtener más información acerca de cómo reinstalar AD DS, consulte [retirada de un controlador de dominio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Si no se puede quitar AD DS normalmente mientras el servidor está conectado a la red, use uno de los métodos siguientes para resolver el problema:

   - Fuerce la eliminación de AD DS en Modo de restauración de servicios de directorio (DSRM), limpie los metadatos del servidor y, a continuación, vuelva a instalar AD DS.
   - Vuelva a instalar el sistema operativo y vuelva a generar el controlador de dominio.

Para obtener más información sobre cómo forzar la eliminación de AD DS, consulte [forzar la eliminación de un controlador de dominio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Uso de repadmin para recuperar el estado de replicación @ no__t-0

El estado de replicación es una manera importante de evaluar el estado del servicio de directorio. Si la replicación funciona sin errores, sabrá que los controladores de dominio están en línea. También sabe que los siguientes sistemas y servicios están funcionando:


- Infraestructura DNS
- Protocolo de autenticación Kerberos
- Servicio de hora de Windows (W32time)
- Llamada a procedimiento remoto (RPC)
- Conectividad de red

Use repadmin para supervisar el estado de la replicación diariamente mediante la ejecución de un comando que evalúe el estado de replicación de todos los controladores de dominio del bosque. El procedimiento genera un archivo. csv que se puede abrir en Microsoft Excel y filtrar por errores de replicación.

Puede usar el siguiente procedimiento para recuperar el estado de replicación de todos los controladores de dominio del bosque. 

Requisitos

La pertenencia a **Administradores de empresa**, o equivalente, es lo mínimo necesario para completar este procedimiento. 

Herramientas:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Para generar una hoja de cálculo de repadmin/showrepl para controladores de dominio

1. Abra una ventana del símbolo del sistema como administrador: En el menú Inicio, haga clic con el botón secundario en símbolo del sistema y, a continuación, haga clic en ejecutar como administrador. Si aparece el cuadro de diálogo control de cuentas de usuario, proporcione credenciales de administradores de empresa, si es necesario, y, a continuación, haga clic en continuar.
2. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `repadmin /showrepl * /csv > showrepl.csv`
3. Abra Excel.
4. Haga clic en el botón Office, haga clic en abrir, desplácese hasta showrepl. csv y, a continuación, haga clic en abrir.
5. Oculte o elimine la columna A, así como la columna de tipo de transporte, como se indica A continuación:
6. Seleccione la columna que desea ocultar o eliminar.

   - Para ocultar la columna, haga clic con el botón secundario en la columna y, a continuación, haga clic en ocultar.
   - Para eliminar la columna, haga clic con el botón secundario en la columna seleccionada y, a continuación, haga clic en eliminar.

7. Seleccione la fila 1 debajo de la fila de encabezado de columna. En la pestaña ver, haga clic en inmovilizar paneles y, a continuación, haga clic en inmovilizar fila superior.
8. Seleccione toda la hoja de cálculo. En la pestaña datos, haga clic en filtro.
9. En la columna última hora de éxito, haga clic en la flecha abajo y, a continuación, haga clic en orden ascendente.
10. En la columna DC de origen, haga clic en la flecha abajo filtrar, seleccione filtros de texto y, a continuación, haga clic en filtro personalizado.
11. En el cuadro de diálogo autofiltro personalizado, en mostrar filas donde, haga clic en no contiene. En el cuadro de texto adyacente, escriba <userInput>Supr</userInput> para eliminar de ver los resultados de los controladores de dominio eliminados.
12. Repita el paso 11 para la columna de hora del último error, pero use el valor no es igual a y, a continuación, escriba el valor 0.
13. Resuelva los errores de replicación.

Para cada controlador de dominio del bosque, la hoja de cálculo muestra el asociado de replicación de origen, la hora en que se produjo la última replicación y el momento en que se produjo el último error de replicación para cada contexto de nomenclatura (partición de directorio). Mediante el uso de Autofiltro en Excel, solo puede ver el estado de la replicación de los controladores de dominio que funcionen, los controladores de dominio que no tienen errores o los controladores de dominio que son los menos actuales, y puede ver los asociados de replicación que se están replicando. successfully.

## <a name="replication-problems-and-resolutions"></a>Problemas de replicación y soluciones

Los problemas de replicación se registran en los mensajes de eventos y en varios mensajes de error que se producen cuando una aplicación o servicio intenta una operación. Idealmente, estos mensajes los recopila la aplicación de supervisión o cuando se recupera el estado de replicación.

La mayoría de los problemas de replicación se identifican en los mensajes de eventos que se registran en el registro de eventos del servicio de directorio. Los problemas de replicación también pueden identificarse en forma de mensajes de error en la salida del comando <system>repadmin/showrepl</system> .

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin/showrepl mensajes de error que indican problemas de replicación

Para identificar Active Directory problemas de replicación, use el comando <system>repadmin/showrepl</system> , como se describe en la sección anterior. En la tabla siguiente se muestran los mensajes de error que genera este comando, junto con las causas raíz de los errores y vínculos a temas que proporcionan soluciones para los errores.

|Error de repadmin|Causa principal|Solución|
| --- | --- | --- |
|El tiempo transcurrido desde la última replicación con este servidor superó la duración del desecho.|Un controlador de dominio ha producido un error en la replicación de entrada con el controlador de dominio de origen con nombre suficiente para que una eliminación se haya extinguido, replicado y recolectado como elemento no utilizado de AD DS.|Identificador de evento 2042: transcurrió demasiado tiempo desde que este equipo se replicó por última vez|
|No hay vecinos entrantes.|Si no aparece ningún elemento en la sección "vecinos de entrada" de la salida generada por repadmin/showrepl, el controlador de dominio no pudo establecer vínculos de replicación con otro controlador de dominio.|Solución de problemas de conectividad de replicación (Id. de evento 1925)| 
|Acceso denegado.|Existe un vínculo de replicación entre dos controladores de dominio, pero la replicación no se puede realizar correctamente como resultado de un error de autenticación.|Solución de problemas de seguridad de replicación| 
|No se pudo realizar el último intento en < > de fecha y hora con el "el nombre de cuenta de destino es incorrecto".|Este problema se puede relacionar con los problemas de conectividad, DNS o autenticación. Si se trata de un error de DNS, el controlador de dominio local no pudo resolver el nombre DNS basado en el identificador único global (GUID) de su asociado de replicación.|Corrección de problemas de búsqueda de DNS de replicación (identificadores de evento 1925, 2087 y 2088) corrección de problemas de seguridad de replicación corrección de problemas de conectividad de replicación (ID. de evento 1925)| 
|Error 49 de LDAP.|Es posible que la cuenta de equipo del controlador de dominio no esté sincronizada con el Centro de distribución de claves (KDC).|Solución de problemas de seguridad de replicación| 
|No se puede abrir la conexión LDAP con el host local|La herramienta de administración no pudo ponerse en contacto con AD DS.|Solución de problemas de consulta de DNS de replicación (id. de evento 1925, 2087, 2088)| 
|Se ha cambiado la replicación de Active Directory.|El progreso de la replicación entrante fue interrumpido por una solicitud de replicación de mayor prioridad, como una solicitud que se generó manualmente con el comando repadmin/Sync.|Espere a que se complete la replicación. Este mensaje informativo indica un funcionamiento normal.| 
|Replicación publicada, en espera.| El controlador de dominio ha publicado una solicitud de replicación y está esperando una respuesta. La replicación está en curso desde este origen.|Espere a que se complete la replicación. Este mensaje informativo indica un funcionamiento normal.| 

En la tabla siguiente se enumeran los eventos comunes que podrían indicar problemas con la replicación de Active Directory, junto con las causas raíz de los problemas y vínculos a temas que proporcionan soluciones para los problemas. 

|ID. de evento y origen|Causa raíz|Solución|
| --- | --- | --- | 
|KCC NTDS 1311|La información de configuración de replicación de AD DS no refleja con precisión la topología física de la red.|Solución de problemas de topología de replicación (identificador de evento 1311)| 
|Replicación de NTDS 1388|No se aplica la coherencia estricta de la replicación y se ha replicado un objeto persistente en el controlador de dominio.|Solución de problemas de objetos persistentes de replicación (identificador de evento 1388, 1988, 2042)|
|KCC NTDS 1925|Error al intentar establecer un vínculo de replicación para una partición de directorio de escritura. Este evento puede tener diferentes causas, en función del error.| Corrigiendo problemas de conectividad de replicación (ID. de evento 1925) corrigiendo problemas de búsqueda de DNS de replicación (identificadores de evento 1925, 2087, 2088)| 
|Replicación de NTDS 1988|El controlador de dominio local ha intentado replicar un objeto de un controlador de dominio de origen que no está presente en el controlador de dominio local porque puede que se haya eliminado y que ya se haya recolectado como elemento no utilizado. La replicación no continúa para esta partición de directorio con este asociado hasta que se resuelva la situación.|Solución de problemas de objetos persistentes de replicación (identificador de evento 1388, 1988, 2042)|
|Replicación de NTDS 2042|No se ha producido la replicación con este asociado para una duración de marcadores de exclusión y la replicación no puede continuar.|Solución de problemas de objetos persistentes de replicación (identificador de evento 1388, 1988, 2042)| 
|Replicación de NTDS 2087|AD DS no pudo resolver el nombre de host DNS del controlador de dominio de origen en una dirección IP y se produjo un error en la replicación.|Solución de problemas de consulta de DNS de replicación (id. de evento 1925, 2087, 2088)| 
|Replicación de NTDS 2088 |AD DS no pudo resolver el nombre de host DNS del controlador de dominio de origen en una dirección IP, pero la replicación se realizó correctamente.|Solución de problemas de consulta de DNS de replicación (id. de evento 1925, 2087, 2088)|
|Inicio de sesión de red 5805|No se pudo autenticar una cuenta de equipo, lo que suele deberse a que hay varias instancias del mismo nombre de equipo o que el nombre de equipo no se está replicando en cada controlador de dominio.|Solución de problemas de seguridad de replicación| 

Para obtener más información sobre los conceptos de replicación, vea [Active Directory tecnologías de replicación](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, incluidos los artículos de soporte específicos de los códigos de error, vea el artículo de soporte técnico: [Cómo solucionar errores comunes de replicación de Active Directory](https://support.microsoft.com/help/3108513)
