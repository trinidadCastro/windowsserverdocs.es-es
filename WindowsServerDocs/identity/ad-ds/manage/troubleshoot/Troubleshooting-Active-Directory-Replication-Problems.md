---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Solución de problemas de replicación de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffc0933f70a2ab518c575a944efdac4c2dcd6a1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869426"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Solución de problemas de replicación de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Problemas de replicación de Active Directory pueden tener varios orígenes distintos. Por ejemplo, problemas, problemas de red o problemas de seguridad del sistema de nombres de dominio (DNS) pueden producir un error de replicación de Active Directory. 

El resto de este tema explica las herramientas y una metodología general para solucionar errores de replicación de Active Directory. Los subtemas siguientes abarcan los síntomas, causas y cómo resolver errores de replicación específico:

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introducción y recursos para solucionar problemas de replicación de Active Directory

Entrada o error de replicación de salida hace que los objetos de Active Directory que representan la topología de replicación, programación de replicación, los controladores de dominio, usuarios, equipos, las contraseñas, grupos de seguridad, las pertenencias a grupos y directiva de grupo sean incoherentes entre los controladores de dominio. Error de incoherencia y replicación de Active provocar errores de funcionamiento o resultados incoherentes, según el controlador de dominio que se establece contacto para la operación y puede impedir que la aplicación de directiva de grupo y los permisos de control de acceso. Los servicios de dominio de Active Directory (AD DS) depende de la conectividad de red, resolución de nombres, autenticación y autorización, la base de datos de directorio, la topología de replicación y el motor de replicación. Cuando la causa raíz de un problema de replicación no es evidente de inmediato, para determinar la causa entre las muchas causas posibles requiere eliminación sistemática de causas probables.

Para que una herramienta basada en la interfaz de usuario ayudar a supervisar la replicación y diagnosticar errores, vea [herramienta de estado de replicación de Active Directory](https://www.microsoft.com/download/details.aspx?id=30005)

Para un documento exhaustivo que se describe cómo puede utilizar la herramienta Repadmin para solucionar problemas de Active Directory, la replicación está disponible; consulte [supervisión y solución de problemas de Active Directory replicación con Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Para obtener información acerca de cómo funciona la replicación de Active Directory, consulte las siguientes referencias técnicas:

- [Referencia técnica del modelo de replicación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Referencia técnica de topología de replicación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Recomendaciones de evento y la herramienta de la solución

Idealmente, el rojo (Error) y amarillo (advertencia) eventos en el registro de eventos del servicio de directorio sugieren la restricción específica que está causando el error de replicación en el controlador de dominio de origen o destino. Si el mensaje de evento sugiere pasos para una solución, pruebe los pasos que se describen en el evento. La herramienta Repadmin y otras herramientas de diagnóstico también proporcionan información que puede ayudarle a resolver errores de replicación. 

Para obtener información detallada sobre cómo usar Repadmin para solucionar problemas de replicación, vea [supervisión y solución de problemas de Active Directory replicación con Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Y excluir intencionadas interrupciones o errores de hardware

A veces se producen errores de replicación debido a interrupciones intencionadas. Por ejemplo, para solucionar problemas de replicación de Active Directory, descartar desconexiones intencionadas y errores de hardware o las actualizaciones en primer lugar.

### <a name="intentional-disconnections"></a>Desconexiones intencionadas

Si un controlador de dominio que está tratando de replicación con un controlador de dominio que se ha compilado en un sitio de ensayo y está sin conexión en espera de su implementación en el sitio de producción final (un sitio remoto, como una sucursal informa de errores de replicación ), puede tener en cuenta para dichos errores de replicación. Para evitar la separación de un controlador de dominio de la topología de replicación durante períodos prolongados, lo que provoca errores continuos hasta que se vuelve a conectar el controlador de dominio, considere la posibilidad de agregar estos equipos inicialmente como servidores miembro y usar la instalación desde medios ( Método IFM) para instalar los servicios de dominio de Active Directory (AD DS). Puede usar la herramienta de línea de comandos Ntdsutil para crear medios de instalación que se pueden almacenar en medios extraíbles (CD, DVD u otro medio) y enviarlas al sitio de destino. A continuación, puede usar los medios de instalación para instalar AD DS en los controladores de dominio en el sitio, sin el uso de replicación. 

### <a name="hardware-failures-or-upgradestitle"></a>Errores de hardware o las actualizaciones</title>

Si se producen problemas de replicación como resultado un error de hardware (por ejemplo, error de una placa base, el subsistema de disco o la unidad de disco duro), notifique al propietario de servidor para que se puede resolver el problema de hardware.

Actualizaciones periódicas de hardware también pueden provocar que los controladores de dominio esté fuera de servicio. Asegúrese de que los propietarios de servidor tienen un buen sistema de comunicación, como interrupciones de antemano.

### <a name="firewall-configuration"></a>Configuración de firewall

De forma predeterminada, Active Directory replication procedimiento remoto (RPC) se realizan llamadas dinámicamente a través de un puerto disponible a través del asignador de punto de conexión de RPC (RPCSS) en el puerto 135. Asegúrese de que Firewall de Windows con seguridad avanzada y otros firewalls están configurados correctamente para permitir la replicación. Para obtener información acerca de cómo especificar el puerto para la replicación de Active Directory y la configuración de puerto, consulte [artículo 224196 en Microsoft Knowledge Base](https://go.microsoft.com/fwlink/?LinkId=22578). 

Para obtener información acerca de los puertos que usa la replicación de Active Directory, vea [las herramientas de replicación de Active Directory y la configuración](https://go.microsoft.com/fwlink/?LinkId=123774).

Para obtener información acerca de cómo administrar la replicación de Active Directory a través de firewalls, consulte [replicación de Active Directory a través de Firewalls](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Responder al error de un servidor obsoleto que ejecutan Windows 2000 Server

Si un controlador de dominio que ejecutan Windows 2000 Server se ha producido durante más tiempo que el número de días de la duración de objetos de desecho, la solución es siempre el mismo: 

1. Mover el servidor de la red corporativa a una red privada.
2. Forzosamente quite Active Directory o reinstalar el sistema operativo.
3. Quite los metadatos del servidor de Active Directory para que no se reactiva el objeto de servidor. 

Puede usar un script para limpiar los metadatos de servidor en la mayoría de los sistemas operativos Windows. Para obtener información sobre el uso de esta secuencia de comandos, consulte [quitar metadatos de controladores de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkID=123599). 

De forma predeterminada, los objetos de configuración NTDS que se han eliminado se reactiva automáticamente durante un período de 14 días. Por lo tanto, si no quita los metadatos del servidor (use Ntdsutil o el script se ha mencionado anteriormente para realizar la limpieza de metadatos), se vuelve a establecer los metadatos del servidor en el directorio, que solicita los intentos de replicación que se produzca. En este caso, se registrarán errores forma persistente como resultado la incapacidad para replicar con el controlador de dominio que falta.

## <a name="root-causes"></a>Causas raíz.

Si descarta desconexiones intencionadas, errores de hardware y obsoletos de los controladores de dominio de Windows 2000, el resto de los problemas de replicación casi siempre tiene una de las siguientes causas:

- Conectividad de red: La conexión de red podría no estar disponible o configuración de red no está configurada correctamente.
- Resolución de nombres: Errores de configuración de DNS son una causa común de errores de replicación.
- Autenticación y autorización: Problemas de autenticación y autorización producir errores de "Acceso denegado" cuando un controlador de dominio intenta conectarse a su asociado de replicación.
- Base de datos de directorio (almacén): La base de datos de directorio podría no ser capaz de procesar las transacciones lo suficientemente rápido para mantenerse al día con los tiempos de espera de replicación.
- Motor de replicación: Si las programaciones de replicación entre sitios son demasiado breves, las colas de replicación podrían ser demasiado grandes para procesar en el tiempo requerido por la programación de replicación de salida. En este caso, la replicación de algunos cambios puede ser suficiente como para superar la duración de objetos de desecho, detenidas potencialmente indefinidamente.
- Topología de replicación: Los controladores de dominio deben tener vínculos entre sitios en AD DS que se asignan a la red de área real extensa (WAN) o conexiones de red privada virtual (VPN). Si crea los objetos de AD DS para la topología de replicación que no son compatibles con la topología de sitio real de la red, se produce un error de replicación que requiere la topología mal configurada.

## <a name="general-approach-to-fixing-problems"></a>Enfoque general para solucionar problemas

Use el siguiente enfoque general para solucionar problemas de replicación: 

1. Supervisar el estado de replicación diaria o usa Repadmin.exe para recuperar el estado de replicación diaria.
2. Intentar resolver cualquier error notificado de manera oportuna mediante los métodos que se describen en esta guía y los mensajes de eventos. Si el software podría estar causando el problema, desinstale el software antes de continuar con otras soluciones.
3. Si no se puede resolver el problema que está causando un error de replicación mediante los métodos conocidos, quitar AD DS del servidor y, a continuación, vuelva a instalar AD DS. Para obtener más información acerca de cómo reinstalar AD DS, consulte [retirada de un controlador de dominio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Si no se puede quitar AD DS normalmente mientras el servidor está conectado a la red, use uno de los métodos siguientes para resolver el problema:

   - Forzar la desinstalación de AD DS en el directorio el modo de restauración de servicios (DSRM), limpiar los metadatos del servidor, y, a continuación, vuelva a instalar AD DS.
   - Reinstalar el sistema operativo y volver a generar el controlador de dominio.

Para obtener más información acerca de cómo forzar la desinstalación de AD DS, consulte [forzar la eliminación de un controlador de dominio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Utilizar Repadmin para recuperar el estado de replicación</title>

Estado de replicación es un aspecto importante para evaluar el estado del servicio de directorio. Si la replicación funciona sin errores, sabrá que los controladores de dominio que están en línea. También sabe que funcionan los sistemas y servicios siguientes:


- Infraestructura de DNS
- Protocolo de autenticación Kerberos
- Servicio de hora de Windows (W32time)
- Llamada a procedimiento remoto (RPC)
- Conectividad de red

Utilizar Repadmin para supervisar el estado de replicación diaria mediante la ejecución de un comando que se evalúa el estado de replicación de todos los controladores de dominio del bosque. El procedimiento genera un archivo .csv que se puede abrir en Microsoft Excel y filtro de errores de replicación.

Puede usar el procedimiento siguiente para recuperar el estado de replicación de todos los controladores de dominio del bosque. 

Requisitos

La pertenencia a **Administradores de empresa**, o equivalente, es lo mínimo necesario para completar este procedimiento. 

Herramientas:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Para generar una hoja de cálculo de repadmin /showrepl para controladores de dominio

1. Abra una ventana del símbolo del sistema como administrador: En el menú Inicio, haga clic en el símbolo del sistema y, a continuación, haga clic en Ejecutar como administrador. Si aparece el cuadro de diálogo Control de cuentas de usuario, proporcione las credenciales de administradores de organización, si es necesario y, a continuación, haga clic en continuar.
2. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR: `repadmin /showrepl * /csv > showrepl.csv`
3. Abra Excel.
4. Haga clic en el botón de Office, haga clic en Abrir, desplácese hasta showrepl.csv y, a continuación, haga clic en Abrir.
5. Ocultar o eliminar una columna, así como la columna de tipo de transporte, como sigue:
6. Seleccione una columna que desea ocultar o eliminar.

   - Para ocultar la columna, haga clic en la columna y, a continuación, haga clic en Ocultar.
   - Para eliminar la columna, haga clic en la columna seleccionada y, a continuación, haga clic en eliminar.

7. Seleccione la fila 1 debajo de la fila de encabezado de columna. En la pestaña de vista, haga clic en Inmovilizar paneles y, a continuación, haga clic en la fila superior inmovilizar.
8. Seleccione toda la hoja de cálculo. En la pestaña datos, haga clic en filtro.
9. En la columna de la última vez correcta, haga clic en la flecha hacia abajo y, a continuación, haga clic en orden ascendente.
10. En la columna de DC de origen, haga clic en el filtro de la flecha abajo, seleccione los filtros de texto y, a continuación, haga clic en filtro personalizado.
11. En el cuadro de diálogo Personalizar Autofiltro, en filas Show donde, haga clic en no contiene. En el cuadro de texto contiguo, escriba <userInput>SUPR</userInput> eliminar desde la vista de los resultados para elimina los controladores de dominio.
12. Repita el paso 11 para la columna de la última vez con error, pero use el valor no es igual y, a continuación, escriba el valor 0.
13. Resolver errores de replicación.

Para cada controlador de dominio del bosque, la hoja de cálculo muestra el asociado de replicación de origen, el tiempo que la replicación se produjo por última vez y la hora en que se produjo el último error de replicación para cada contexto de nomenclatura (partición de directorio). Mediante Autofiltro en Excel, puede ver el estado de replicación para trabajar únicamente, los controladores de dominio no se pudo sólo los controladores de dominio o los controladores de dominio que son más o menos actual, y puede ver a los asociados de replicación que se replican correctamente.

## <a name="replication-problems-and-resolutions"></a>Las resoluciones y problemas de replicación

Problemas de replicación se notifican los mensajes de eventos y en diversos mensajes de error que se producen cuando una aplicación o servicio intenta una operación. Idealmente, estos mensajes se recopilan por su aplicación de supervisión o al recuperar el estado de replicación.

La mayoría de los problemas de replicación se identifica en los mensajes de eventos que se registran en el registro de eventos del servicio de directorio. También se podrían identificar problemas de replicación en forma de mensajes de error en la salida de la <system>repadmin /showrepl</system> comando.

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin /showrepl los mensajes de error que indican problemas de replicación

Para identificar problemas de replicación de Active Directory, use el <system>repadmin /showrepl</system> de comandos, como se describe en la sección anterior. La siguiente tabla muestra los mensajes de error que genera este comando, junto con las causas raíz de los errores y vínculos a temas que ofrecen soluciones para los errores.

|Error de repadmin|Causa principal|Solución|
| --- | --- | --- |
|El tiempo transcurrido desde la última replicación con este servidor ha superado el período de duración de objetos de desecho.|Un controlador de dominio de error en la replicación entrante con el controlador de dominio de origen con nombre largo suficiente para una eliminación que se ha desechado, replicarse y recolección de AD DS.|Identificador de evento 2042: transcurrió demasiado tiempo desde que este equipo se replicó por última vez|
|No hay vecinos de entrada.|Si no hay elementos aparecen en la sección "Vecinos de entrada" de la salida generada por repadmin /showrepl, el controlador de dominio no pudo establecer vínculos de replicación con otro controlador de dominio.|Solución de problemas de conectividad de replicación (Id. de evento 1925)| 
|Acceso denegado.|Existe un vínculo de replicación entre dos controladores de dominio, pero la replicación no puede realizarse correctamente como resultado un error de autenticación.|Solución de problemas de seguridad de replicación| 
|El último intento < fecha - hora > error con el "nombre de la cuenta de destino es correcto".|Este problema puede deberse a problemas de autenticación, DNS o conectividad. Si se trata de un error de DNS, el controlador de dominio local no pudo resolver el identificador único global (GUID): nombre DNS de su asociado de replicación basado en.|Corrección replicación DNS búsqueda problemas (Id. de evento 1925, 2087, 2088) corregir replicación seguridad problemas de solución de problemas de conectividad de replicación (Id. de evento 1925)| 
|Error LDAP 49.|Es posible que la cuenta de equipo del controlador de dominio no sincronizada con el centro de distribución de claves (KDC).|Solución de problemas de seguridad de replicación| 
|No se puede abrir la conexión LDAP con el host local|La herramienta de administración no pudo ponerse en contacto con AD DS.|Solución de problemas de consulta de DNS de replicación (id. de evento 1925, 2087, 2088)| 
|Se cambió la replicación de Active Directory.|El progreso de la replicación de entrada se ha interrumpido por una solicitud de replicación de prioridad más alta, por ejemplo, una solicitud que se ha generado manualmente con el comando de repadmin/sync.|Espere a que finalice la replicación. Este mensaje informativo indica que el funcionamiento normal.| 
|Registra la replicación, en espera.| El controlador de dominio registra una solicitud de replicación y está esperando una respuesta. La replicación está en curso desde este origen.|Espere a que finalice la replicación. Este mensaje informativo indica que el funcionamiento normal.| 

En la tabla siguiente se enumera los eventos comunes que podrían indicar problemas con Active Directory hace que la replicación, junto con la raíz de los problemas y vínculos a temas que ofrecen soluciones para los problemas. 

|Id. de evento y origen|Causa raíz|Solución|
| --- | --- | --- | 
|1311 NTDS KCC|La información de configuración de replicación de AD DS no reflejan con precisión la topología física de la red.|Solución de problemas de topología de replicación (identificador de evento 1311)| 
|Replicación de NTDS 1388|Coherencia estricta de replicaciones no está en vigor, y un objeto persistente se haya replicado en el controlador de dominio.|Solución de problemas de objetos persistentes de replicación (identificador de evento 1388, 1988, 2042)|
|1925 NTDS KCC|Error al intentar establecer un vínculo de replicación para una partición de directorio de escritura. Este evento puede tener diversas causas, dependiendo del error.| Corrección replicación problemas (Id. de evento 1925) corregir replicación DNS búsqueda problemas de conectividad (Id. de evento 1925, 2087, 2088)| 
|Replicación de NTDS 1988|El controlador de dominio local ha intentado replicar un objeto desde un controlador de dominio de origen que no está presente en el controlador de dominio local porque es posible que se han eliminado y ya recopilados de elementos no utilizados. La replicación no continúa para esta partición de directorio con este socio hasta que se resuelva la situación.|Solución de problemas de objetos persistentes de replicación (identificador de evento 1388, 1988, 2042)|
|Replicación de NTDS 2042|La replicación no se ha producido este asociado para una duración de objetos de desecho y no puede continuar la replicación.|Solución de problemas de objetos persistentes de replicación (identificador de evento 1388, 1988, 2042)| 
|Replicación de NTDS 2087|AD DS no pudo resolver el nombre de host DNS del controlador de dominio de origen a una dirección IP y no se pudo replicar.|Solución de problemas de consulta de DNS de replicación (id. de evento 1925, 2087, 2088)| 
|Replicación de NTDS 2088 |AD DS no pudo resolver el nombre de host DNS del controlador de dominio de origen a una dirección IP, pero la replicación se ha realizado correctamente.|Solución de problemas de consulta de DNS de replicación (id. de evento 1925, 2087, 2088)|
|5805 net Logon|Una cuenta de equipo no se pudo realizar la autenticación, que puede deberse a cualquier varias instancias del mismo nombre de equipo o el nombre del equipo no se replica en cada controlador de dominio.|Solución de problemas de seguridad de replicación| 

Para obtener más información sobre los conceptos de replicación, vea [las tecnologías de replicación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, incluida la compatibilidad artículos específicos para los códigos de error consulte el artículo de soporte técnico: [Cómo solucionar errores comunes de replicación de Active Directory](https://support.microsoft.com/help/3108513)
