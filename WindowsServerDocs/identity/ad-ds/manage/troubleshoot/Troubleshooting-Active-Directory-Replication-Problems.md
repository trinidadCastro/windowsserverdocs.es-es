---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: "Solucionar problemas de replicación de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: eb93ea7cab297d70aa86b71bd5466004e47bfeaf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Solucionar problemas de replicación de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Problemas de replicación de Active Directory pueden tener distintos orígenes. Por ejemplo, problemas de sistema de nombres de dominio (DNS), problemas de red o problemas de seguridad pueden producir error de replicación de Active Directory. 

El resto de este tema explica las herramientas y una metodología general para solucionar errores de replicación de Active Directory. Para un laboratorio de prácticas que demuestra cómo solucionar problemas de replicación de Active Directory, consulte [laboratorio Virtual de TechNet: solución de problemas de errores de replicación de directorio activo](https://go.microsoft.com/?linkid=9844718)

Los temas siguientes tratan síntomas, las causas y cómo resolver errores de replicación específica:
   
[Cómo solucionar problemas de objeto persistentes de replicación (identificadores de evento 1388, 1988, 2042)](https://technet.microsoft.com/library/cc949124.aspx)
  
[Cómo solucionar problemas de seguridad de replicación](https://technet.microsoft.com/library/cc949132.aspx)

[Corregir problemas de búsqueda de DNS de replicación (identificadores de evento 1925, 2087, 2088)](https://technet.microsoft.com/library/cc949133.aspx)
  
[Cómo solucionar problemas de conectividad de replicación (identificador de evento 1925)](https://technet.microsoft.com/library/cc949131.aspx)
  
[Cómo solucionar problemas de replicación topología (identificador de evento 1311)](https://technet.microsoft.com/library/cc949129.aspx)
  
[Comprueba la funcionalidad de DNS para admitir la replicación de directorio](https://technet.microsoft.com/library/dd728017.aspx)
  
[Error de replicación 8614 Active Directory no puede replicar con este servidor porque el tiempo desde la última replicación con este servidor ha superado la duración de desecho](https://technet.microsoft.com/library/replication-error-8614-the-active-directory-cannot-replicate-with-this-server-because-the-time-since-the-last-replication-with-this-server-has-exceeded-the-tombstone-lifetime.aspx)
     
[Replicación error 8524 DSA la operación es no se puede continuar debido a un error de la consulta DNS](https://technet.microsoft.com/library/replication-error-8524-the-dsa-operation-is-unable-to-proceed-because-of-a-dns-lookup-failure.aspx)
   
[Error de replicación 8456 o 8457 el origen de | servidor de destino está actualmente rechazando solicitudes de replicación](https://technet.microsoft.com/library/replication-error-8456-the-source-server-is-currently-rejecting-replication-requests-or-replication-error-8457-the-destination-server-is-currently-rejecting-replication-requests.aspx)
   
[Se denegó el acceso de replicación de replicación error 8453](https://technet.microsoft.com/library/replication-error-8453-replication-access-was-denied.aspx)
  
[Error de replicación 8452 el contexto de nomenclatura se está quitando o no se replica desde el servidor especificado](https://technet.microsoft.com/library/replication-error-8452-the-naming-context-is-in-the-process-of-being-removed-or-is-not-replicated-from-the-specified-server.aspx)
   
[Error de replicación 5 se deniega el acceso](https://technet.microsoft.com/library/replication-error-5-access-is-denied.aspx)
  

      
      

  
[Error de replicación-2146893022 el nombre principal de destino es incorrecto](https://technet.microsoft.com/library/replication-error-2146893022-the-target-principal-name-is-incorrect.aspx)
  

      
      

  
[Error de replicación 1753 allí son no hay más extremos disponibles desde el asignador de extremo](https://technet.microsoft.com/library/replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper.aspx)
  

      
      

  
[Replicación error 1722 el servidor RPC no está disponible](https://technet.microsoft.com/library/replication-error-1722-the-rpc-server-is-unavailable.aspx)
  

      
      

  
[Error de replicación error al iniciar sesión 1396 el nombre de cuenta de destino es incorrecta](https://technet.microsoft.com/library/replication-error-1396-logon-failure-the-target-account-name-is-incorrect.aspx)
  

      
      

  
[Error de replicación 1256 el sistema remoto no está disponible](https://technet.microsoft.com/library/replication-error-1256-the-remote-system-is-not-available.aspx)
  

      
      

  
[Error de replicación 1127 acceso al disco duro, una operación de disco error incluso después de varios intentos](https://technet.microsoft.com/library/replication-error-1127-while-accessing-the-hard-disk-a-disk-operation-failed-even-after-retries.aspx)
  

      
      

  
[Error de replicación 8451 la operación de replicación encontró un error de base de datos](https://technet.microsoft.com/library/replication-error-8451-the-replication-operation-encountered-a-database-error.aspx)
  

      
      

  
[Atributos de replicación error 8606 insuficiente indicaron para crear un objeto](https://technet.microsoft.com/library/replication-error-8606-insufficient-attributes-were-given-to-create-an-object.aspx)
  

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introducción y los recursos de solución de problemas de replicación de Active Directory

Entrantes o error de replicación saliente hace que los objetos de Active Directory que representan la topología de replicación, programación de replicación, controladores de dominio, los usuarios, equipos, las contraseñas, grupos de seguridad, pertenencia a grupos y directiva de grupo para ser coherente entre controladores de dominio. Error de replicación e incoherencia de directorio causar errores de funcionamiento o resultados incoherentes, según el controlador de dominio que se pone en contacto para la operación y pueden impedir que la aplicación de la directiva de grupo y los permisos de control de acceso. Los servicios de dominio de Active Directory (AD DS) depende de la conectividad de red, la resolución de nombres, autenticación y autorización, la base de datos de directorio, la topología de replicación y el motor de replicación. Cuando la causa raíz de un problema de replicación no resulten evidente de inmediato, determinar la causa entre las muchas causas posibles requiere sistemática eliminación de probables. 

Para que una herramienta basada en la interfaz de usuario ayudar a supervisar la replicación y diagnosticar errores, consulta [herramienta de estado de replicación de Active Directory](https://www.microsoft.com/download/details.aspx?id=30005). También hay un [práctica](https://go.microsoft.com/?linkid=9844718) que demuestra cómo usar el estado de replicación de Active Directory y otras herramientas para solucionar los errores. 

Para un documento global que se describe cómo usar la herramienta Repadmin para solucionar problemas de Active Directory replicación esté disponible; consulta [supervisión y solución de problemas de Active Directory replicación usando Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Para obtener información sobre cómo funciona la replicación de Active Directory, consulte las siguientes referencias técnicas:


- [Referencia técnica de modelo de réplica de Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Referencia técnica de la topología de replicación de Director de activo](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Eventos y la herramienta de recomendaciones para la solución

En teoría, el rojo (Error) y amarillo (advertencia) eventos en el registro de eventos de servicio de directorio sugieren la restricción específica que está causando el error de replicación en el controlador de dominio de origen o destino. Si el mensaje de evento sugiere pasos para una solución, prueba los pasos que se describen en el evento. La herramienta Repadmin y otras herramientas de diagnóstico también proporcionan información que puede ayudar a resolver problemas de replicación. 

Para obtener más información sobre cómo usar Repadmin para solucionar problemas de replicación, [supervisión y solución de problemas de Active Directory replicación usando Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Excluir las interrupciones de forma intencionadas o errores de hardware

A veces se producen errores de replicación debido a interrupciones de forma intencionadas. Por ejemplo, para solucionar problemas de replicación de Active Directory, descartar desconexiones intencionados y errores de hardware o actualizaciones en primer lugar.

### <a name="intentional-disconnections"></a>Desconexiones de forma intencionadas

Si los errores de replicación informa de un controlador de dominio que está intentando replicación con un controlador de dominio que se ha generado en un sitio de ensayo y está sin conexión en espera de su implementación en el sitio de producción final (un sitio remoto, como una sucursal), puede tener en cuenta los errores de replicación. Para evitar la separación de un controlador de dominio de la topología de replicación durante períodos prolongados, lo que provoca errores continuos hasta que se vuelva a conectar el controlador de dominio, considera la posibilidad de agregar estos equipos inicialmente como servidores miembro y el uso de la instalación desde el método de medios (IFM) para instalar servicios de dominio de Active Directory (AD DS). Puedes usar la herramienta de línea de comandos de Ntdsutil para crear medios de instalación que se pueden almacenar en un medio extraíble (CD, DVD u otro medio) y se envían al sitio de destino. A continuación, puedes usar los medios de instalación para instalar AD DS en los controladores de dominio en el sitio, sin el uso de replicación. 

### <a name="hardware-failures-or-upgradestitle"></a>Errores de hardware o actualizaciones</title>

Si se producen problemas de replicación como resultado el error de hardware (por ejemplo, el error de una placa base, el subsistema de disco o la unidad de disco duro), notificar el propietario del servidor para que se puede resolver el problema de hardware.

Actualizaciones de hardware periódicas también pueden provocar controladores de dominio para estar fuera de servicio. Asegúrese de que los propietarios de servidor tienen un buen sistema de comunicar estas interrupciones por adelantado.

### <a name="firewall-configuration"></a>Configuración del Firewall

De manera predeterminada, llamadas de Active Directory replicación procedimiento remoto (RPC) de forma dinámica en un puerto disponible a través de asignador de extremo de RPC (RPCSS) en el puerto 135. Asegúrate de que Firewall de Windows con seguridad avanzada y otros firewalls están configurados correctamente para permitir la replicación. Para obtener información acerca de cómo especificar el puerto de réplica de Active Directory y configuración de puerto, consulte [224196 en Microsoft Knowledge Base del artículo](https://go.microsoft.com/fwlink/?LinkId=22578). 

Para obtener información acerca de los puertos que usa la replicación de Active Directory, consulte [herramientas de réplica de Active Directory y opciones de configuración](https://go.microsoft.com/fwlink/?LinkId=123774). 

Para obtener información sobre la administración de réplica de Active Directory sobre firewalls, vea [replicación de Active Directory sobre Firewalls](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Responder a los errores de un servidor obsoleto que ejecutan Windows 2000 Server

Si se produjo un error en un controlador de dominio que ejecutan Windows 2000 Server para superar el número de días en la duración de desecho, la solución es siempre el mismo: 

1. Mover el servidor de la red corporativa a una red privada.
2. Forzar quite Active Directory o reinstalar el sistema operativo.
3. Quitar los metadatos de servidor de Active Directory, de modo que no se reactivará el objeto de servidor. 

Puedes usar un script para limpiar los metadatos de servidor en la mayoría de los sistemas operativos Windows. Para obtener información sobre cómo usar este script, consulta [quitar metadatos de controlador de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkID=123599). 

De manera predeterminada, los objetos de configuración NTDS que se eliminan se reactivará automáticamente durante un período de 14 días. Por lo tanto, si no quitas metadatos de servidor (use Ntdsutil o el script se ha indicado anteriormente para realizar la limpieza de metadatos), se restablece los metadatos de servidor en el directorio, que solicita se producen los intentos de replicación. En este caso, se registrarán errores continuamente como resultado de la incapacidad para replicar con el controlador de dominio que falta.

## <a name="root-causes"></a>Causas

Si la regla desconexiones de forma intencionadas, errores de hardware y controladores de dominio de Windows 2000 obsoletos, el resto de problemas de replicación casi siempre tiene una de las siguientes causas: 


- Conectividad de red: la conexión de red puede no estar disponible o no está configurados correctamente la configuración de red.
- Resolución de nombres: errores de configuración de DNS son una causa común de problemas de replicación.
- Autenticación y autorización: problemas de autenticación y autorización provocar errores de "Acceso denegado" cuando intenta conectarse a su duplicador un controlador de dominio.
- Base de datos del directorio (tienda): la base de datos del directorio no pueda procesar transacciones lo suficientemente rápido como para mantenerse al día con los tiempos de espera de replicación.
- Motor de replicación: si los planes de replicación entre sitios son demasiado cortos, colas de replicación pueden ser demasiado grandes para procesar en el tiempo necesario para la programación de replicación de salida. En este caso, duplicación de algunos cambios puede ser indefinitelypotentially detenido, lo suficiente como para superar la duración de desecho.
- Topología de replicación: controladores de dominio deben tener vínculos entre sitios en AD DS que se asignan a la red de área real extensa (WAN) o conexiones de red privada virtual (VPN). Si creas objetos en AD DS para la topología de replicación que no son compatibles con la topología de sitio real de la red, se produce un error de replicación que requiere la topología mal configurada.

## <a name="general-approach-to-fixing-problems"></a>Enfoque general para solucionar problemas

Utilizar el siguiente método general para solucionar problemas de replicación: 


1. Supervisar el estado de replicación de todos los días o usar Repadmin.exe para recuperar el estado de replicación de todos los días.
2. Intentar resolver los errores notificados en el momento oportuno mediante los métodos que se describen en esta guía y mensajes de eventos. Si software podría estar causando el problema, desinstala el software antes de continuar con otras soluciones.
3. Si no se puede resolver el problema que está causando el error de replicación por los métodos conocidos, quita AD DS desde el servidor y, a continuación, volver a instalar AD DS. Para obtener más información acerca de cómo reinstalar AD DS, consulta [la retirada de un controlador de dominio](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Si no se puede quitar AD DS normalmente mientras el servidor está conectado a la red, usa uno de los siguientes métodos para resolver el problema:
 

    - Forzar la eliminación de AD DS en el directorio el modo de restauración de servicios (DSRM), limpiar los metadatos del servidor, y, a continuación, volver a instalar AD DS.
    - Reinstalar el sistema operativo y volver a generar el controlador de dominio.

Para obtener más información acerca de cómo forzar la desinstalación de AD DS, consulta [forzar la eliminación de un controlador de dominio](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Uso de Repadmin para recuperar el estado de replicación</title>

Estado de replicación es un aspecto importante para TI evaluar el estado del servicio de directorio. Si la replicación funciona sin errores, sabes que los controladores de dominio que están en línea. También sabes que funcionen los siguientes sistemas y servicios:


- Infraestructura DNS
- Protocolo de autenticación Kerberos
- Servicio hora de Windows (W32time)
- Llamada a procedimiento remoto (RPC)
- Conectividad de red

Usar Repadmin para supervisar el estado de replicación de todos los días mediante la ejecución de un comando que se evalúa el estado de replicación de todos los controladores de dominio en el bosque. El procedimiento, genera un archivo .csv que se puede abrir en Microsoft Excel y filtros para problemas de replicación.

Puedes usar el siguiente procedimiento para recuperar el estado de replicación de todos los controladores de dominio del bosque. 
      
Requisitos
      
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento. 

Herramientas: 


- Repadmin.exe 
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Para generar una hoja de cálculo de repadmin /showrepl para controladores de dominio



1. Abre un símbolo del sistema como administrador: en el menú Inicio, haz clic en el símbolo del sistema y, a continuación, haz clic en Ejecutar como administrador. Si aparece el cuadro de diálogo de Control de cuentas de usuario, proporcionar credenciales de administrador de empresa, si es necesario y, a continuación, haz clic en continuar. 
2. En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:`repadmin /showrepl * /csv &gt;showrepl.csv`
3. Abre Excel.
4. Haz clic en el botón de Office, haz clic en Abrir, desplácese hasta showrepl.csv y, a continuación, haz clic en Abrir.
5. Ocultar o eliminar columna, así como en la columna de tipo de transporte, como sigue:
6. Selecciona una columna que desea ocultar o eliminar.
      

    - Para ocultar la columna, haz clic en la columna y, a continuación, haz clic en Ocultar.
    - Para eliminar la columna, haz clic en la columna seleccionada y, a continuación, haz clic en eliminar.
7. Selecciona la fila 1 debajo de la fila de encabezado de columna. En la pestaña vista, haz clic en Inmovilizar paneles y, a continuación, haz clic en Inmovilizar fila superior.
8. Selecciona toda la hoja de cálculo. En la pestaña datos, haz clic en el filtro.
9. En la columna de la última vez correcta, haz clic en la flecha hacia abajo y, a continuación, haz clic en orden ascendente.
10. En la columna de DC de origen, haz clic en el filtro de flecha abajo, elija filtros de texto y, a continuación, haz clic en filtro personalizado.
11. En el cuadro de diálogo Autofiltro personalizado, en donde, haz clic en carecerá de filas de mostrar. En el cuadro de texto, escribe <userInput>SUPR</userInput> eliminar de la vista de los resultados para elimina los controladores de dominio.
12. Repite el paso 11 para la columna de la última vez con error, pero usa el valor no es igual y, a continuación, escribe el valor 0.
13. Resolver errores de replicación.

Para cada controlador de dominio en el bosque de la hoja de cálculo muestra el asociado de replicación de origen, el tiempo que la replicación última ocurrió y el tiempo que se produjo el último error de replicación para cada contexto de nomenclatura (partición del directorio). Mediante Autofiltro en Excel, puede ver el estado de replicación para trabajar en controladores de dominio únicamente, si no solo los controladores de dominio o controladores de dominio actual menos o a la mayoría, y puedes ver a los asociados de replicación replican correctamente.

## <a name="replication-problems-and-resolutions"></a>Resoluciones y problemas de replicación
    
En mensajes de eventos y en varios mensajes de error que se producen cuando una aplicación o servicio intenta realizar una operación, se notifican problemas de replicación. En teoría, estos mensajes se recopilan por tu aplicación de supervisión o cuando esta se recupera el estado de replicación.

La mayoría de los problemas de replicación se identifica en los mensajes de eventos que se registran en el registro de eventos de servicio de directorio. Problemas de replicación también pueden detectarse en forma de mensajes de error en los resultados de la <system>repadmin /showrepl</system> comando. 

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin /showrepl mensajes de error que indican problemas de replicación

Para identificar problemas de replicación de Active Directory, usa el <system>repadmin /showrepl</system> comando, como se describe en la sección anterior. La siguiente tabla muestra mensajes de error que se genera este comando, junto con las causas de los errores y vínculos a temas que ofrecen soluciones para los errores.


|Error repadmin|Causa raíz|Solución|
| --- | --- | --- |
|El tiempo desde la última replicación con este servidor ha superado la duración de desecho.|Un controlador de dominio se produjo un error de replicación de entrada con el controlador de dominio de origen con nombre largo suficiente para una eliminación han excluido, replicar y se recolectan de AD DS.|Identificador de evento: 2042: Ha transcurrido mucho tiempo desde este equipo replican|
|No hay vecinos de entrada.|Si no hay elementos aparecen en la sección "Vecinos de entrada" de los resultados que se generan por repadmin /showrepl, el controlador de dominio no pudo establecer vínculos de replicación con otro controlador de dominio.|Cómo solucionar problemas de conectividad de replicación (identificador de evento 1925)| 
|Acceso denegado.|Existe un vínculo de replicación entre dos controladores de dominio, pero no se puede realizar replicación correctamente como resultado un error de autenticación.|Cómo solucionar problemas de seguridad de replicación| 
|El último intento < fecha - hora > no se pudo realizar con el "nombre de la cuenta de destino es incorrecta."|Este problema puede estar relacionada con problemas de autenticación, DNS o conectividad. Si se trata de un error DNS, el controlador de dominio local no pudo resolver el identificador único global (GUID)-según el nombre DNS de su asociado de replicación.|Solución replicación DNS búsqueda problemas (identificadores de evento 1925, 2087, 2088) corregir seguridad problemas de replicación de solución de problemas de conectividad de replicación (identificador de evento 1925)| 
|Error LDAP 49.|Es posible que la cuenta de equipo del controlador de dominio no se sincronicen con el centro de distribución de claves (KDC).|Cómo solucionar problemas de seguridad de replicación| 
|No se puede abrir la conexión LDAP al host local|La herramienta de administración no podría ponerse en contacto con AD DS.|Corregir problemas de búsqueda de DNS de replicación (identificadores de evento 1925, 2087, 2088)| 
|Ha tenido preferencia sobre la replicación de Active Directory.|Una solicitud de replicación de mayor prioridad, como una solicitud que se haya generado manualmente con el comando repadmin /sync interrumpió el progreso de replicación de entrada.|Espera para que la replicación en completarse. Este mensaje informativo indica una operación normal.| 
|Replicación registrado, espera.| El controlador de dominio solicitado una replicación y está esperando una respuesta. Replicación está en curso de esta fuente.|Espera para que la replicación en completarse. Este mensaje informativo indica una operación normal.| 

La siguiente tabla enumera los eventos más comunes que pueden indicar problemas de replicación, junto con la raíz de las causas de los problemas y vínculos a temas que ofrecen soluciones para los problemas de Active Directory. 

|Identificador de evento y el código fuente|Causa raíz|Solución|
| --- | --- | --- | 
|1311 NTDS KCC|La información de configuración de replicación de AD DS no refleja la topología física de la red.|Cómo solucionar problemas de replicación topología (identificador de evento 1311)| 
|1388 NTDS replicación|Coherencia de replicación estricta no está en efecto, y un objeto persistente se replique al controlador de dominio.|Cómo solucionar problemas de objeto persistentes de replicación (identificadores de evento 1388, 1988, 2042)|
|1925 NTDS KCC|Error al intentar establecer un vínculo de replicación de una partición de directorio grabable. Este evento puede tener causas diferentes, dependiendo del error.| Solución conectividad problemas (identificador de evento 1925) corregir replicación DNS búsqueda problemas de replicación (identificadores de evento 1925, 2087, 2088)| 
|1988 NTDS replicación|El controlador de dominio local ha intentado replicar un objeto desde un controlador de dominio de origen que no está presente en el controlador de dominio local porque puede se han eliminado y ya recolección. Replicación no continuará para esta partición de directorio con este socio hasta que se resuelva la situación.|Cómo solucionar problemas de objeto persistentes de replicación (identificadores de evento 1388, 1988, 2042)|
|Replicación de NTDS 2042|Replicación no ha ocurrido con este socio durante un ciclo de vida de desecho y no se puede continuar replicación.|Cómo solucionar problemas de objeto persistentes de replicación (identificadores de evento 1388, 1988, 2042)| 
|2087 NTDS replicación|AD DS no pudo resolver el nombre de host DNS del controlador de dominio de origen a una dirección IP y error de replicación.|Corregir problemas de búsqueda de DNS de replicación (identificadores de evento 1925, 2087, 2088)| 
|2088 NTDS replicación |AD DS no pudo resolver el nombre de host DNS del controlador de dominio de origen a una dirección IP, pero la replicación se realizó correctamente.|Corregir problemas de búsqueda de DNS de replicación (identificadores de evento 1925, 2087, 2088)|
|De net Logon 5805|Una cuenta de equipo no pudo autenticar, que está provocado habitualmente por cualquier varias instancias del mismo nombre de equipo o el nombre del equipo no se replican a cada controlador de dominio.|Cómo solucionar problemas de seguridad de replicación| 

Para obtener más información sobre los conceptos de replicación, consulta [tecnologías de replicación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
