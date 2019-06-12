---
title: Recuperación de bosques de AD - realizar una recuperación inicial
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 9883d337520c3920f8638ddfe5f6bd393e31fd2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442864"
---
# <a name="perform-initial-recovery"></a>Realizar recuperación inicial  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Esta sección incluye los siguientes pasos:  

- [Restaurar el primer controlador de dominio grabable en cada dominio](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Volver a conectar cada controlador de dominio grabable que se restauró a la red](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Agregar el catálogo global a un controlador de dominio en el dominio raíz del bosque](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurar el primer controlador de dominio grabable en cada dominio  

A partir de un controlador de dominio grabable en el dominio raíz del bosque, complete los pasos descritos en esta sección para restaurar el primer controlador de dominio. El dominio raíz del bosque es importante, ya que almacena los grupos Administradores de esquema y administradores de empresa. También ayuda a mantener la jerarquía de confianza del bosque. Además, el dominio raíz del bosque contiene normalmente el servidor raíz DNS para el espacio de nombres DNS del bosque. Por lo tanto, la zona DNS integrado en Active Directory para ese dominio contiene los registros de recursos de alias (CNAME) para todos los demás controladores de dominio en el bosque (que son necesarios para la replicación) y los registros de recursos DNS de catálogo global. 
  
Después de recuperar el dominio raíz del bosque, repita los mismos pasos para recuperar los restantes dominios del bosque. Puede recuperar más de un dominio al mismo tiempo; Sin embargo, siempre que recupere un dominio primario antes de recuperar un elemento secundario para evitar cualquier interrupción en la jerarquía de confianza o la resolución de nombres DNS. 
  
Para cada dominio que recuperar, restaurar un único controlador de dominio grabable de copia de seguridad. Se trata de la parte más importante de la recuperación porque el controlador de dominio debe tener una base de datos que no esté influido por aquello que ocasionó el bosque para producir un error. Es importante tener una copia de seguridad de confianza que se prueba exhaustivamente antes de se introduce en el entorno de producción. 
  
A continuación, realice los pasos siguientes. Los procedimientos para llevar a cabo ciertos pasos [recuperación de bosques de AD - procedimientos](AD-Forest-Recovery-Procedures.md). 
  
1. Si va a restaurar un servidor físico, asegúrese de que el cable de red del destino DC no está conectado y, por tanto, no está conectado a la red de producción. Para una máquina virtual, puede quitar el adaptador de red o use un adaptador de red que está conectado a otra red donde puede probar el proceso de recuperación mientras aislada de la red de producción. 
  
2. Puesto que es el primer controlador de dominio grabable en el dominio, debe realizar una restauración no autoritativa de AD DS y una restauración autoritativa de SYSVOL. La operación de restauración debe realizarse mediante el uso de una copia de seguridad compatibles con Active Directory y restaurar la aplicación, como copias de seguridad de Windows Server (es decir, no se debe restaurar el controlador de dominio mediante el uso de métodos no admitidos como la restauración de una instantánea de máquina virtual). 
   - Una restauración autoritativa de SYSVOL es necesaria porque la replicación de SYSVOL replicado carpeta debe iniciarse después de recuperarse ante un desastre. Todos los controladores de dominio posteriores que se agregan en el dominio deben volver a sincronizar sus carpetas SYSVOL con una copia de la carpeta que se ha seleccionado para ser autoritativos antes de la carpeta puede anunciarse. 

   > [!CAUTION]
   > Realizar una operación de restauración autoritativa (o principal) de SYSVOL solo para el primer controlador de dominio que se restaure en el dominio raíz del bosque. Incorrectamente realizar las operaciones de restauración principal de la carpeta SYSVOL en otros controladores de dominio da lugar a conflictos de replicación de datos de SYSVOL. 

   - Hay dos opciones de realizan una restauración no autoritativa de AD DS y una restauración autoritativa de SYSVOL:  
   - Realizar una recuperación completa del servidor y, a continuación, forzar una sincronización autoritativa de SYSVOL. Para obtener información detallada, consulte [realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md) y [realizar una sincronización autoritativa de SYSVOL DFSR replicado](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Realizar una recuperación completa del servidor seguido de una restauración del estado del sistema. Esta opción requiere la creación de antemano de ambos tipos de copias de seguridad: una copia de seguridad completa y una copia de seguridad del estado del sistema. Para obtener información detallada, consulte [realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md) y [realizar una restauración no autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Después de restaurar y reiniciar el controlador de dominio grabable, compruebe que el error no afectó a los datos en el controlador de dominio. Si los datos del controlador de dominio está dañados, a continuación, repita el paso 2 con una copia de seguridad diferente. 
   - Si el controlador de dominio restaurado hospeda un rol de maestro de operaciones, deberá agregar la siguiente entrada del registro para evitar que se va a estar disponible hasta que haya completado la replicación de una partición de directorio de escritura de AD DS:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl realizar sincronizaciones iniciales**  
  
      Cree la entrada con el tipo de datos **REG_DWORD** y un valor de **0**. Una vez recuperado completamente el bosque, puede restablecer el valor de esta entrada de **1**, lo que requiere un controlador de dominio que se reinicia y de entrada contiene roles maestros de operaciones para tener éxito AD DS y replicación de salida con su conoce los asociados de réplica antes de que se anuncia como controlador de dominio y empieza a proporcionar servicios a los clientes. Para obtener más información acerca de los requisitos de sincronización inicial, consulte el artículo de KB [305476](https://support.microsoft.com/kb/305476). 
  
      Continúe con los pasos siguientes después de restaurar y comprobar los datos y antes de unir este equipo a la red de producción. 
  
4. Si sospecha que estaba relacionada con el error en todo el bosque de red intrusión o un ataque malintencionado, restablecer las contraseñas de cuenta para todas las cuentas administrativas, incluidos a los miembros de los administradores de organización, Admins. del dominio, administradores de esquema, operadores de servidor, la cuenta Grupos de operadores y así sucesivamente. El restablecimiento de contraseñas de cuenta administrativa debe realizarse antes de los controladores se instalan durante la fase siguiente de la recuperación de bosque de dominio adicional. 
5. En el primer controlador de dominio restaurado en el dominio raíz del bosque, asuma todos los roles de maestro de operaciones de todo el dominio y todo el bosque. Se requieren credenciales de administradores de empresas y administradores de esquema para asumir los roles de maestro de operaciones de todo el bosque. 
  
     En cada dominio secundario, asumir los roles de maestro de operaciones de todo el dominio. Aunque podría retener los roles de maestro de operaciones en el controlador de dominio restaurado sólo temporalmente, asumir estas funciones le garantiza respecto a qué controlador de dominio hospeda en este momento en el proceso de recuperación del bosque. Como parte del proceso posterior a la recuperación, puede redistribuir los roles de maestro de operaciones según sea necesario. Para obtener más información acerca de cómo asumir roles de maestro de operaciones, consulte [asumir un rol de maestro de operaciones](AD-forest-recovery-seizing-operations-master-role.md). Para obtener recomendaciones sobre dónde colocar los roles de maestro de operaciones, consulte [¿cuáles son los maestros de operaciones?](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Limpiar los metadatos de todos los demás controladores de dominio grabables en el dominio raíz del bosque que no va a restaurar desde copia de seguridad (todos los DC grabables del dominio excepto este primer controlador de dominio). Si usa la versión de los usuarios de Active Directory y los equipos o los sitios de Active Directory y los servicios que se incluye con Windows Server 2008 o posterior o RSAT para Windows Vista o versiones posteriores, la limpieza de metadatos se realiza automáticamente cuando se elimina un objeto de controlador de dominio. Además, el objeto de servidor y el objeto de equipo para el controlador de dominio eliminado también se eliminan automáticamente. Para obtener más información, consulte [limpiar metadatos de quita los controladores de dominio grabables](AD-Forest-Recovery-Cleaning-Metadata.md). 
  
     Limpiar metadatos evita la posible duplicación de los objetos de configuración NTDS si AD DS está instalado en un controlador de dominio en un sitio diferente. Potencialmente, esto también podría guardar el Comprobador de coherencia de la información (KCC) el proceso de creación de vínculos de replicación cuando los propios controladores de dominio podrían no estar presentes. Además, como parte de la limpieza de metadatos, se eliminará los registros de recursos DNS del ubicador de DC para todos los demás controladores de dominio en el dominio de DNS. 
  
     Hasta que se quitan los metadatos de todos los demás controladores de dominio en el dominio, este controlador de dominio, si se tratara de un maestro de RID antes de recuperación, no supondrá que la función de maestro de RID y, por tanto, no podrá emitir RID nuevos. Es posible que vea Id. de evento 16650 en el registro del sistema en el Visor de eventos que indica este error, pero debería ver Id. de evento 16648 que indica el éxito un poco después de que ha limpiado los metadatos. 
  
7. Si tiene zonas DNS almacenadas en AD DS, asegúrese de que el servicio servidor DNS local está instalado y ejecutándose en el controlador de dominio que se haya restaurado. Si este controlador de dominio no era un servidor DNS antes del error de bosque, debe instalar y configurar el servidor DNS. 
  
    > [!NOTE]
    > Si el controlador de dominio restaurado ejecuta Windows Server 2008, deberá instalar la revisión del artículo de KB [975654](https://support.microsoft.com/kb/975654) o conectar el servidor a una red aislada temporalmente para instalar el servidor DNS. La revisión no es necesaria para cualquier otra versión de Windows Server. 
  
     En el dominio raíz del bosque, configurar el controlador de dominio restaurado con su propia dirección IP (o una dirección de bucle invertido, por ejemplo, 127.0.0.1) como su servidor DNS preferido. Puede configurar esta opción en las propiedades de TCP/IP del adaptador de red de área local (LAN). Este es el primer servidor DNS en el bosque. Para obtener más información, consulte [configurar TCP/IP para utilizar DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     En cada dominio secundario, configure el controlador de dominio restaurado con la dirección IP del primer servidor DNS en el dominio raíz del bosque como su servidor DNS preferido. Puede configurar esta opción en las propiedades de TCP/IP del adaptador de LAN. Para obtener más información, consulte [configurar TCP/IP para utilizar DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     En las zonas DNS _msdcs y dominio, elimine los registros NS de controladores de dominio que ya no existen después de la limpieza de metadatos. Compruebe si se han quitado los registros SRV de los controladores de dominio limpios. Para ayudar a acelerar la eliminación de registros SRV de DNS, ejecute:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Aumente el valor del grupo RID disponible en 100.000. Para obtener más información, consulte [generar el valor de los grupos de RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md). Si tiene motivos para creer que elevar el grupo RID por 100 000 no es suficiente para su situación concreta, debe determinar el aumento de más bajo que todavía puede utilizarse con seguridad. Los RID son un recurso finito que no se debe usar seguridad innecesariamente. 
  
     Si se crearon nuevas entidades de seguridad en el dominio después de la hora de la copia de seguridad que usará para la restauración, estas entidades de seguridad que tenga derechos de acceso en determinados objetos. Estas entidades de seguridad dejan de existir tras la recuperación porque la recuperación se ha revertido a la copia de seguridad; Sin embargo, podrían seguir existiendo sus derechos de acceso. Si el grupo RID disponible no se produce después de una restauración, nuevo usuario los objetos que se crean después de la recuperación del bosque puede obtener los identificadores de seguridad idénticos (SID) y podría tener acceso a esos objetos, lo que no se diseñó originalmente. 
  
     Para ilustrarlo, considere el ejemplo del nuevo empleado denominado a Amy mencionada en la introducción. Ya no existe el objeto de usuario para Amy después de la operación de restauración porque se creó después de la copia de seguridad que se usó para restaurar el dominio. Sin embargo, pueden conservar los derechos de acceso que se asignaron a ese objeto de usuario después de la operación de restauración. Si el SID para ese objeto de usuario se reasigna a un nuevo objeto después de la operación de restauración, el nuevo objeto obtendría esos derechos de acceso. 
  
9. Invalidar el grupo RID actual. Se invalida el grupo RID actual después de restaurar el estado del sistema. Pero si no se ha realizado una restauración del estado del sistema, el grupo RID actual debe invalidarse para evitar que el controlador de dominio restaurado volver a emitir RID desde el grupo RID que asignó en el momento en que se creó la copia de seguridad. Para obtener más información, consulte [invalidar el grupo RID actual](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > La primera vez que se está intentando crear un objeto con un SID después de invalidar el grupo RID recibirá un error. Error al intentar crear un objeto desencadena una solicitud de un nuevo grupo RID. Reintento de la operación se realiza correctamente porque el nuevo grupo RID que se va a asignar. 
  
10. Restablecer la contraseña de cuenta de equipo de este controlador de dominio dos veces. Para obtener más información, consulte [restablecer la contraseña de cuenta de equipo del controlador de dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Restablecer la contraseña de krbtgt dos veces. Para obtener más información, consulte [restablecer la contraseña de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     Dado que el historial de contraseñas de krbtgt es dos contraseñas, restablecer contraseñas dos veces para quitar la contraseña (previa al error) original del historial de contraseñas. 
  
    > [!NOTE]
    > Si la recuperación del bosque es en respuesta a una infracción de seguridad, también puede restablecer las contraseñas de confianza. Para obtener más información, consulte [restablecer una contraseña de confianza en un lado de la confianza](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Si el bosque tiene varios dominios y el controlador de dominio restaurado era un servidor de catálogo global antes del error, desactive la **catálogo Global** casilla en las propiedades de configuración NTDS para quitar el catálogo global desde el controlador de dominio. La excepción a esta regla es el caso común de un bosque con un único dominio. En este caso, no es necesario quitar el catálogo global. Para obtener más información, consulte [quitar el catálogo global](AD-Forest-Recovery-Remove-GC.md). 
  
     Puede restaurar un catálogo global desde una copia de seguridad más recientes que los de otras copias de seguridad que se usan para restaurar los controladores de dominio en otros dominios, podría producir objetos persistentes. Considere el ejemplo siguiente. En el dominio, DC1 se restaura desde una copia de seguridad realizada en T1. En el dominio B, DC2 se restaura desde una copia de seguridad de catálogo global que se realizó en el tiempo T2. Supongamos que T2 es más reciente que T1 y algunos objetos se crearon entre T1 y T2. Después de restauraron estos controladores de dominio, DC2, que es un catálogo global, contiene datos más recientes para réplica parcial del dominio que contiene un dominio propio. DC2, en este caso, contiene los objetos persistentes, porque estos objetos no están presentes en DC1. 
  
     La presencia de los objetos persistentes puede causar problemas. Por ejemplo, mensajes de correo electrónico no pueden enviarse a un usuario cuyo objeto de usuario se ha movido entre dominios. Después de conectar el DC obsoleta o servidor de catálogo global en línea, ambas instancias del objeto de usuario aparecen en el catálogo global. Los dos objetos tienen la misma dirección de correo electrónico; por lo tanto, no se puede entregar mensajes de correo electrónico. 
  
     Un segundo problema es que una cuenta de usuario que ya no existe todavía es posible que aparecen en la lista global de direcciones. Un tercer problema es que un grupo universal que ya no existe podría seguir apareciendo en el token de acceso de un usuario. 
  
     Si restauró un controlador de dominio que era un catálogo global, ya sea por accidente o porque ese era la copia de seguridad solitario que de confianza, se recomienda que impida la aparición de objetos persistentes deshabilitando el catálogo global, poco después de la operación de restauración íntegro. Deshabilitar el indicador de catálogo global dará como resultado el equipo perder todas sus réplicas parciales (particiones) y relegating a sí mismo en estado normal de controlador de dominio. 
  
13. Configurar el servicio de hora de Windows. En el dominio raíz del bosque, configurar el emulador de PDC para sincronizar la hora de un origen de hora externo. Para obtener más información, consulte [configurar el servicio de hora de Windows en el emulador de PDC en el dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Volver a conectar cada controlador de dominio grabable restaurada a una red común

En esta fase debe tener un controlador de dominio restaurado (y los pasos de recuperación realiza) en el dominio raíz del bosque y en cada uno de los dominios restantes. Únase a estos controladores de dominio a una red común que está aislada del resto del entorno y complete los pasos siguientes para validar la replicación y el estado del bosque.

> [!NOTE]
> Cuando se une a los controladores de dominio físicos a una red aislada, deberá cambiar sus direcciones IP. Como resultado, las direcciones IP de los registros DNS será incorrecta. Dado que un servidor de catálogo global no está disponible, se producirá un error en las actualizaciones dinámicas seguras para DNS. Controladores de dominio virtuales son más beneficiosas en este caso porque puede unirse a una nueva red virtual sin cambiar sus direcciones IP. Se trata de uno de los motivos por qué se recomiendan los controladores de dominio virtuales como controladores de dominio de primer restaurarse durante la recuperación del bosque. 
  
Después de la validación, únase a los controladores de dominio a la red de producción y complete los pasos para comprobar el estado de replicación del bosque.

- Para solucionar el problema de resolución de nombres, cree registros de delegación DNS y configure DNS de sugerencias de raíz y reenvío según sea necesario. Ejecute **repadmin/Replsum** para comprobar la replicación entre controladores de dominio. 
- Si el controlador de dominio restaurado no es asociados de replicación directos, recuperación de replicación será mucho más rápido mediante la creación de objetos de conexión temporal entre ellos. 
- Para validar la limpieza de metadatos, ejecute **Repadmin /viewlist \\** * para obtener una lista de todos los controladores de dominio del bosque. Ejecute **/DCLIST Nltest:** *< dominio\>*  para obtener una lista de todos los controladores de dominio en el dominio. 
- Para comprobar el estado de DC y DNS, ejecute DCDiag /v para informar de errores en todos los controladores de dominio del bosque. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Agregar el catálogo global a un controlador de dominio en el dominio raíz del bosque

Un catálogo global es necesario para estas y otras razones:  
  
- Para habilitar los inicios de sesión para los usuarios. 
- Para habilitar el servicio de Net Logon que se ejecutan en los controladores de dominio en cada dominio secundario para registrar y eliminar registros en el servidor DNS en el dominio raíz. 
  
Aunque es preferible que el controlador de dominio raíz del bosque se convierta en un catálogo global, es posible elegir cualquiera de los controladores de dominio restaurados para convertirse en un catálogo global. 
  
> [!NOTE]
> Un controlador de dominio no se anunciará como un servidor de catálogo global hasta que haya completado una sincronización completa de todas las particiones de directorio en el bosque. Por lo tanto, se debería forzar el controlador de dominio para replicar con cada uno de los controladores de dominio restaurados en el bosque. 
>
> Supervisar el servicio de directorio registro de eventos en el Visor de eventos para el evento ID 1119, lo que indica que este controlador de dominio es un servidor de catálogo global, o compruebe que la siguiente clave del registro tiene un valor de 1:  
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global Catalog Promotion Complete**  
  
Para obtener más información, consulte [adición del catálogo global](AD-Forest-Recovery-Add-GC.md). 
  
En esta fase debe tener un bosque estable, con un controlador de dominio para cada dominio y un catálogo global en el bosque. Debe realizar una nueva copia de seguridad de cada uno de los controladores de dominio que acaba de restaurar. Ahora puede comenzar a implementar otros controladores de dominio del bosque mediante la instalación de AD DS. 

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosques de AD - idear un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosques de AD: identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación de bosques de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosques de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosques de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosques de AD - recuperar un único dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosques de AD: recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
