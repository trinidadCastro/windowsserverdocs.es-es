---
title: "Recuperación de bosque de AD - realizar una recuperación inicial"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: fc2ec09c5b96b76229d532adc6a254c8108d8940
ms.sourcegitcommit: ac73f0f0dca04be731b928183bfdffc97d82c277
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="perform-initial-recovery"></a>Realizar una recuperación inicial  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Esta sección incluye los siguientes pasos:  
  
-   [Restaurar el primer controlador de dominio grabable en cada dominio](#Restore-the-first-writeable-domain-controller-in-each-domain)  

-   [Volver a conectar cada controlador de dominio grabable restaurado a la red](#Reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
  
-   [Agregar el catálogo global a un controlador de dominio en el dominio raíz del bosque](#Add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  
  
  
## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurar el primer controlador de dominio grabable en cada dominio  
 A partir de un controlador de dominio grabable en el dominio raíz, completa los pasos de esta sección para restaurar el primer controlador de dominio. Dominio raíz del bosque es importante porque almacena los grupos Administradores de esquema y administradores de empresa. También ayuda a mantener la jerarquía de confianza en el bosque. Además, el dominio raíz contiene por lo general el servidor de raíz DNS para el espacio de nombres del bosque DNS. En consecuencia, la zona DNS integrado en Active Directory para ese dominio contiene los registros de recursos de alias (CNAME) para todos los otros controladores de dominio en el bosque (que son necesarios para la replicación) y los registros de recursos DNS catálogo global.  
  
 Después de recuperar el dominio raíz, repite los mismos pasos para recuperar los dominios restantes en el bosque. Puedes recuperar más de un dominio simultáneamente. Sin embargo, siempre que recuperar un dominio principal antes de recuperar un elemento secundario para evitar cualquier interrupción en la jerarquía de confianza o la resolución de nombres DNS.  
  
 Para cada dominio que lo recuperes, restaurar solo un controlador de dominio grabable de copia de seguridad. Esta es la parte más importante de la recuperación porque el controlador de dominio debe tener una base de datos que no se han afectada por los que la causa del bosque un error. Es importante tener una copia de seguridad de confianza que se ha probado exhaustivamente antes de que se presenta en el entorno de producción.  
  
 A continuación, realiza los siguientes pasos. Los procedimientos para realizar algunos pasos [recuperación de bosque de AD - procedimientos ](AD-Forest-Recovery-Procedures.md).  
  
1.  Si tienes previsto restaurar un servidor físico, asegúrate de que el cable de red del destino de controlador de dominio no está conectado y, por tanto, no está conectado a la red de producción. Para una máquina virtual, puedes quitar el adaptador de red o usar un adaptador de red que está conectado a otra red donde puedes probar el proceso de recuperación mientras aislado de la red de producción.  
  
2.  Dado que es el primer controlador de dominio grabable en el dominio, debes realizar una restauración no autorizada de AD DS y una restauración de SYSVOL. La operación de restauración debe realizarse mediante el uso de una copia de seguridad compatibles con Active Directory y restaurar la aplicación, como copia de seguridad de Windows Server (es decir, no se debe restaurar el controlador de dominio mediante métodos no admitidos como la restauración de una instantánea de la máquina virtual).  
  
     Una restauración de SYSVOL es necesaria porque la replicación de SYSVOL replicados carpeta debe iniciarse después de recuperar de un desastre. Todos los controladores de dominio posteriores que se agregan en el dominio deben volver a sincronizar la carpeta SYSVOL con una copia de la carpeta que se ha seleccionado esté autorizado antes de la carpeta puede anunciarse.  
  
    > [!CAUTION]
    >  Realizar una operación de restauración autorizado (o principal) de SYSVOL solo para el primer controlador de dominio restaurarse en el dominio raíz. Incorrectamente realizar operaciones de restauración principal de la carpeta SYSVOL en otros controladores de dominio conduce a conflictos de replicación de datos SYSVOL.  
  
     Hay dos opciones realizan una restauración no autorizada de AD DS y una restauración de SYSVOL:  
  
    -   Realizar una recuperación completa del servidor y, a continuación, a continuación, forzar una sincronización autorizada de SYSVOL. Para obtener información detallada, consulta [realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md) y [realizar una sincronización autorizada de SYSVOL replicados DFSR ](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
    -   Realizar una recuperación completa del servidor, seguida de una restauración del estado del sistema. Esta opción requiere que crees ambos tipos de copias de seguridad de antemano: una copia de seguridad completa del servidor y una copia de seguridad de estado del sistema. Para obtener información detallada, consulta [realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md) y [realizar una restauración de servicios de dominio de Active Directory no autorizada ](AD-Forest-Recovery-Nonauthoritative-Restore.md).  
  
3.  Después de restaurar y reiniciar el controlador de dominio grabable, comprueba que el error no afectó a los datos en el controlador de dominio. Si los datos del controlador de dominio está dañados, a continuación, repite el paso 2 con una copia de seguridad diferente.  
  
     Si el controlador de dominio restaurado hospeda un rol de maestro de operaciones, debes agregar la siguiente clave del registro para evitar AD DS no está disponible hasta que haya completado la replicación de una partición de directorio de escritura:  
  
     **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl sincronizaciones iniciales**  
  
     Crear la entrada con el tipo de datos **REG_DWORD** y un valor de **0**. Una vez recuperado el bosque completamente, puedes restablecer el valor de esta entrada de **1**, lo que requiere un controlador de dominio que se reinicia y contiene los roles de maestro para que las operaciones de éxito de entrada de AD DS y replicación saliente con sus asociados de réplica conocidos antes de que se anuncia como controlador de dominio y comienza a proporcionar servicios a los clientes. Para obtener más información sobre los requisitos de la sincronización inicial, consulte el artículo [305476](https://support.microsoft.com/kb/305476).  
  
     Seguir los siguientes pasos solo después de restaurar y comprobar los datos y antes de unir el equipo a la red de producción.  
  
4.  Si sospechas que estaba relacionada con el error de todo el bosque intrusiones en la red o ataques malintencionados, restablecer las contraseñas de cuenta para todas las cuentas administrativas, incluidos a los miembros de los administradores de empresa, administradores de dominio, administradores de esquema, los operadores de servidor, grupos de operadores de cuentas y así sucesivamente. El restablecimiento de contraseñas de las cuentas administrativas debe realizarse antes de los controladores se instalan en la siguiente etapa de la recuperación de bosque de dominio adicional.  
  
5.  En la primera restaurado en el dominio raíz del bosque asumir todos los roles de maestro de operaciones de todo el dominio y todo el bosque. Credenciales de administrador de empresa y administradores de esquema son necesarios para asumir roles de maestro de operaciones de todo el bosque.  
  
     En cada dominio secundario, asumir roles de maestro de operaciones de todo el dominio. Aunque es posible que conservar los roles de maestro de operaciones en el controlador de dominio restaurado solo temporalmente, asumir estos roles garantiza referentes a qué DC aloja en este punto en el proceso de recuperación del bosque. Como parte del proceso posterior a la recuperación, puedes redistribuir los roles de maestro de operaciones según sea necesario. Para obtener más información acerca de cómo asumir roles de maestro de operaciones, consulta [asumir un rol de maestro de operaciones](AD-forest-recovery-seizing-operations-master-role.md). Para obtener recomendaciones sobre dónde colocar los roles de maestro de operaciones, consulta [¿cuáles son los maestros de operaciones?](https://technet.microsoft.com/en-us/library/cc779716.aspx).  
  
6.  Limpiar los metadatos de todos los otros controladores de dominio grabables en el dominio raíz que no va a restaurar desde la copia de seguridad (todos pueden escribir controladores de dominio del dominio excepto este primer controlador de dominio). Si usas la versión de los usuarios de Active Directory y equipos o sitios de Active Directory y servicios que se incluyen con Windows Server 2008 o posterior o RSAT para Windows Vista o versiones posteriores, limpieza de los metadatos se realiza automáticamente cuando se elimina un objeto de DC. Además, el objeto de servidor y el objeto de equipo para el controlador de dominio eliminado también se eliminan automáticamente. Para obtener más información, consulta [limpieza de metadatos de quita controladores de dominio grabables](AD-Forest-Recovery-Cleaning-Metadata.md).  
  
     Limpiar los metadatos evita la duplicación posibles de objetos de configuración NTDS si AD DS está instalada en un controlador de dominio en otro sitio. Posiblemente, esto también podría guardar el Comprobador de coherencia de la información (KCC) el proceso de creación de vínculos de replicación cuando los controladores de dominio a sí mismos podrían no estar presentes. Además, como parte de la limpieza de los metadatos, se eliminarán los registros de recursos de DNS de ubicador de controlador de dominio para todos los otros controladores de dominio en el dominio de DNS.  
  
     Hasta que se eliminan los metadatos de todos los otros controladores de dominio del dominio, este controlador de dominio, si se tratara de un patrón de RID antes de recuperación, no supone la función de maestro RID y, por tanto, no podrá emitir RID nuevo. Es posible que veas el identificador de evento 16650 en el registro del sistema en el Visor de eventos que indica que este error, pero deberías ver el identificador de evento 16648 que indica éxito un poco después de que has borrado los metadatos.  
  
7.  Si tienes zonas DNS que se almacenan en AD DS, asegúrate de que el servicio de servidor DNS local está instalarse y ejecutarse en el controlador de dominio que se ha restaurado. Si este controlador de dominio no era un servidor DNS antes del error de bosque, debes instalar y configurar el servidor DNS.  
  
    > [!NOTE]
    >  Si el controlador de dominio restaurado ejecuta Windows Server 2008, debes instalar la revisión en el artículo KB [975654](https://support.microsoft.com/kb/975654) o conectar el servidor a una red aislada temporalmente para instalar el servidor DNS. La revisión no es necesaria para cualquier otra versión de Windows Server.  
  
     En el dominio raíz del bosque, configurar el controlador de dominio restaurado con su propia dirección IP (o una dirección de bucle invertido, como 127.0.0.1) como su servidor DNS. Puedes configurar esta configuración en las propiedades de TCP/IP del adaptador de red de área local (LAN). Este es el primer servidor DNS en el bosque. Para obtener más información, consulta [configurar TCP/IP para que use DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     En cada dominio secundario, configurar el controlador de dominio restaurado con la dirección IP del servidor DNS primera en el dominio raíz del bosque como su servidor DNS. Puedes configurar esta configuración en las propiedades de TCP/IP del adaptador de LAN. Para obtener más información, consulta [configurar TCP/IP para que use DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     En las zonas DNS _msdcs y dominio, eliminar registros NS de controladores de dominio que ya no existen después de limpieza de metadatos. Comprueba si se quitaron los registros SRV de los controladores de dominio limpios arriba. Para acelerar la eliminación de registros de SRV de DNS, ejecuta:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8.  Generar el valor del conjunto de RIDS disponible en 100.000. Para obtener más información, consulta [generar el valor de grupos RID](AD-Forest-Recovery-Raise-RID-Pool.md). Si tienes motivo creer que generar el grupo de eliminar en 100.000 es insuficiente para su situación concreta, debes determinar el aumento más bajo que es seguro usar aún. RID son un recurso finito que no debe usarse una innecesariamente.  
  
     Si se crearon nuevos principales de seguridad en el dominio después de la hora de la copia de seguridad que usas para la restauración, estas entidades de seguridad pueden tener derechos de acceso en determinados objetos. Estas entidades de seguridad ya no existen después de recuperación porque la recuperación se revierte a la copia de seguridad; Sin embargo, podrían seguir existiendo sus derechos de acceso. Si no se genera el conjunto de RIDS disponible tras una restauración, nuevo usuario objetos que se crean después de la recuperación de bosque puede obtener identificadores de seguridad idénticos (SID) y puede tener acceso a esos objetos, lo que no se diseñó originalmente.  
  
     Para ilustrar esto, considera el ejemplo del empleado nuevo denominado a Amy mencionado en la introducción. Ya no existe el objeto de usuario para Amy después de la operación de restauración porque se creó después de la copia de seguridad que se usó para restaurar el dominio. Sin embargo, es posible que conserva los derechos de acceso que se han asignado a ese objeto de usuario después de la operación de restauración. Si se vuelve a asignar el SID de ese objeto de usuario a un nuevo objeto después de la operación de restauración, el nuevo objeto obtendrían los derechos de acceso.  
  
9. Invalidar el conjunto de RID actual. Después de restaurar el estado del sistema, se invalida el conjunto de RID actual. Pero si no se realizó una restauración del estado del sistema, el conjunto actual de RID tiene que invalidar para evitar que el controlador de dominio restaurado volver a emitir RID desde el conjunto de RID asignado en el momento en que se creó la copia de seguridad. Para obtener más información, consulta [invalidar el conjunto actual de RID](AD-Forest-Recovery-Invaildate-RID-Pool.md).  
  
    > [!NOTE]
    >  La primera vez que se intenta crear un objeto con un SID después invalidar el conjunto de RID recibirá un error. Intentar crear un objeto desencadena una solicitud de un nuevo grupo RID. Reintentar la operación se realiza correctamente porque se asignará el nuevo grupo RID.  
  
10. Restablecer la contraseña de cuenta de equipo este controlador de dominio dos veces. Para obtener más información, consulta [restablecer la contraseña de cuenta de equipo del controlador de dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md).  
  
11. Restablecer la contraseña de krbtgt dos veces. Para obtener más información, consulta [restablecer la contraseña de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md).  
  
     Dado que el historial de contraseñas krbtgt es dos contraseñas, restablecer contraseñas dos veces para quitar la contraseña (previa al error) original desde el historial de contraseñas.  
  
    > [!NOTE]
    >  Si es la recuperación de bosque en respuesta a una infracción de seguridad, también puede restablecer las contraseñas de confianza. Para obtener más información, consulta [restablecer una contraseña de confianza a un lado de la confianza](AD-Forest-Recovery-Reset-Trust.md).  
  
12. Si el bosque tiene varios dominios y el controlador de dominio restaurado era un servidor de catálogo global antes del error, desactive la **catálogo Global** casilla de verificación de las propiedades de configuración NTDS para quitar el catálogo global desde el controlador de dominio. La excepción a esta regla es el caso común de un bosque con un solo dominio. En este caso, no es necesario quitar el catálogo global. Para obtener más información, consulta [eliminar el catálogo global](AD-Forest-Recovery-Remove-GC.md).  
  
     Un catálogo global de la restauración desde una copia de seguridad más recientes que otras copias de seguridad que se usan para restaurar los controladores de dominio de otros dominios, puede incorporar objetos persistentes. Considera el siguiente ejemplo. En un dominio, se restaurará DC1 desde una copia de seguridad que se tomó tiempo T1. En el dominio B DC2 se restaura desde una copia de seguridad de catálogo global que se tomó tiempo T2. Supongamos que T2 es más reciente que T1 y algunos objetos creados entre T1 y T2. Después de que se restauran estos controladores de dominio, DC2, que es un catálogo global, contiene datos más recientes para réplica parcial de dominio que contiene el dominio A sí mismo. DC2, en este caso, contiene los objetos persistentes porque estos objetos no están presentes en DC1.  
  
     La presencia de objetos persistentes puede provocar problemas. Por ejemplo, no se pueden entregar mensajes de correo electrónico a un usuario cuyo objeto de usuario se ha movido entre dominios. Después de que el servidor de catálogo global en línea o el DC obsoleto, aparecen ambas instancias del objeto de usuario en el catálogo global. Ambos objetos tienen la misma dirección de correo electrónico; por lo tanto, no se puede entregar mensajes de correo electrónico.  
  
     Un segundo problema es que una cuenta de usuario que ya no existe aún puede aparecer en la lista global de direcciones. Un tercer problema es que un grupo universal con la que ya no existe aún puede aparecer en el token de acceso de un usuario.  
  
     Si Restaurar un controlador de dominio que era un catálogo global, ya sea sin darse cuenta o porque era la copia de seguridad solitaria que de confianza, te recomendamos que evitar la aparición de objetos persistentes deshabilitando el catálogo global pronto después de completar la operación de restauración. Deshabilitar el indicador de catálogo global generará el equipo de pérdida de todas sus réplicas parciales (particiones) y relegating propio estado normal de DC.  
  
13. Configurar el servicio hora de Windows. En el dominio raíz del bosque, configurar el emulador PDC para sincronizar la hora de una fuente externa. Para obtener más información, consulta [configurar el servicio hora de Windows en el emulador PDC en el dominio raíz](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
 

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Volver a conectar cada controlador de dominio grabable restaurado a una red común  
 En esta etapa debe tener un controlador de dominio restaurado (y los pasos de recuperación realiza) en el dominio raíz del bosque y en cada uno de los dominios restantes. Únete a estos controladores de dominio a una red común que está aislada del resto del entorno y realiza los siguientes pasos para poder validar la salud de bosque y de replicación.  
  
> [!NOTE]
>  Cuando te unes a los controladores de dominio físicos a una red aislada, es podrán que debas cambiar sus direcciones IP. Como resultado, las direcciones IP de los registros DNS serán incorrectos. Como un servidor de catálogo global no está disponible, se producirá un error en las actualizaciones dinámicas seguras para DNS. Controladores de dominio virtuales son más beneficiosas en este caso porque puede estar unidos a una nueva red virtual sin cambiar sus direcciones IP. Esto es uno de los motivos por qué se recomiendan los controladores de dominio virtuales como los controladores de dominio de primer restaurarse durante la recuperación de bosque.  
  
 Después de la validación, Únete a los controladores de dominio a la red de producción y completar los pasos para comprobar el estado de replicación de bosque.  
  
-   Para corregir la resolución de nombres, crear registros de delegación DNS y configurar DNS de sugerencias de reenvío y raíz según sea necesario. Ejecutar **/Replsum repadmin** para comprobar la replicación entre controladores de dominio.  
  
-   Si el controlador de dominio restaurado no está asociados de replicación directos, recuperación de replicación será mucho más rápido mediante la creación de objetos temporales conexión entre ellas.  
  
-   Para validar la limpieza de los metadatos, ejecute **Repadmin /viewlist \ *** para obtener una lista de todos los controladores de dominio del bosque. Ejecutar **Nltest /DCList:***< domain\ >* para obtener una lista de todos los controladores de dominio del dominio.  
  
-   Para comprobar el estado del controlador de dominio y DNS, ejecutar DCDiag /v informes de errores en todos los controladores de dominio del bosque.  
  
  

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Agregar el catálogo global a un controlador de dominio en el dominio raíz del bosque  
 Un catálogo global es necesario para estas y otras razones:  
  
-   Para habilitar los inicios de sesión para los usuarios.  
  
-   Para habilitar el servicio de Net Logon que se ejecuta en los controladores de dominio en cada dominio secundario para registrar y eliminar registros en el servidor DNS del dominio raíz.  
  
 Si bien es preferible que el controlador de dominio raíz del bosque se convierten en un catálogo global, es posible elegir cualquiera de los controladores de dominio restaurados a convertirse en un catálogo global.  
  
> [!NOTE]
>  Un controlador de dominio no se anunciarán como un servidor de catálogo global hasta que haya completado una sincronización completa de todas las particiones de directorio en el bosque. Por lo tanto, el controlador de dominio debe verse obligado a replicar con cada uno de los controladores de dominio restaurados en el bosque.  
>   
>  Supervisar el servicio de directorio registro de eventos en el Visor de eventos para el evento 1119 ID, que indica que este controlador de dominio es un servidor de catálogo global, o para comprobar que la siguiente clave del registro tiene un valor de 1:  
>   
>  **Promoción de HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global catálogo completo**  
  
 Para obtener más información, consulta [agregar el catálogo global](AD-Forest-Recovery-Add-GC.md).  
  
 En esta etapa debe tener un bosque estable, con un controlador de dominio para cada dominio y un catálogo global en el bosque. Debes realizar una nueva copia de seguridad de cada uno de los controladores de dominio que se ha restaurado solo. Ahora puede empezar a implementar otros controladores de dominio en el bosque mediante la instalación de AD DS.  

## <a name="next-steps"></a>Pasos siguientes
-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
  
