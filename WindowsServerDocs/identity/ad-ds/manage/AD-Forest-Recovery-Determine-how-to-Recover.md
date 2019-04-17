---
title: "Recuperación de bosque de AD - determinar cómo recuperar el bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: cc2525068225644b0964a2726bd9c0dcf2e0cba0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="determine-how-to-recover-the-forest"></a>Determinar cómo recuperar el bosque  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Recuperación de un bosque de Active Directory implica restaurarla a partir de copia de seguridad o volver a instalar los servicios de dominio de Active Directory (AD DS) en cada controlador de dominio (corriente continua) en el bosque. Recuperación del bosque restaura cada dominio en el bosque a su estado en el momento de la última copia de seguridad de confianza. En consecuencia, la operación de restauración dará como resultado la pérdida de al menos los siguientes datos de Active Directory:  
  
-   Todos los objetos (como los usuarios y equipos) que se han agregado después de la última copia de seguridad de confianza  
  
-   Todas las actualizaciones que se hayan realizado a los objetos existentes desde la confianza de la última copia de seguridad  
  
-   Todos los cambios realizados en la partición de configuración o la partición de esquema de AD DS (como cambios de esquema) desde la última copia de seguridad de confianza  
  
 Para cada dominio en el bosque, debe conocer la contraseña de una cuenta de administrador de dominio. Preferiblemente, esta es la contraseña de la cuenta predefinida de administrador, que no debe estar deshabilitada. También debes conocer la contraseña de DSRM para realizar una restauración del estado del sistema de un controlador de dominio. En general, es una buena práctica para archivar la cuenta de administrador y el historial de contraseñas DSRM en un lugar seguro para siempre y cuando las copias de seguridad son válidas, es decir, en el período de vida de desecho o en el objeto eliminado período de duración si está habilitada la Papelera de reciclaje de Active Directory. También puede sincronizar la contraseña DSRM con una cuenta de usuario de dominio para que sea más fácil de recordar. Para obtener más información, consulte el artículo [961320](https://support.microsoft.com/kb/961320). Sincronización de la cuenta DSRM debe hacerse antes de la recuperación de bosque, como parte de preparación.  
  
> [!NOTE]
>  La cuenta de administrador es miembro del grupo integrado Administradores de manera predeterminada, como son los grupos Administradores de dominio y administradores de empresa. Este grupo tiene control total de todos los controladores de dominio del dominio.  
  
## <a name="determining-which-backups-to-use"></a>Determinar las copias de seguridad para usar  
 Haz una copia al menos dos controladores de dominio grabables para cada dominio con regularidad para varias copias de seguridad para elegir. Ten en cuenta que no puedes usar la copia de seguridad de un controlador de dominio de solo lectura (RODC) para restaurar un controlador de dominio grabable. Se recomienda que restaure los controladores de dominio mediante el uso de las copias de seguridad que se tomaron unos días antes de la aparición del error. En general, debes determinar un equilibrio entre la recentness y la safeness de los datos restaurados. Elección de una copia de seguridad más reciente recupera los datos más útiles, pero es posible que aumente el riesgo de reintroducir peligrosos datos en el bosque restaurado.  
  
 Restaurar copias de seguridad de sistema estado depende del sistema operativo y el servidor de la copia de seguridad original. Por ejemplo, no debe restaurar una copia de seguridad de estado del sistema a un servidor diferente. En este caso, verás la siguiente advertencia:  
  
 "La copia de seguridad especificado es de un servidor diferente a la actual. No recomendamos realizar una recuperación de estado del sistema con la copia de seguridad en un servidor alternativo porque el servidor puede quedar inutilizable. ¿Estás seguro de que quieras usar esta copia de seguridad para recuperar el servidor actual? "  
  
 Si necesitas restaurar Active Directory en hardware diferente, crear copias de seguridad completa del servidor y va a realizar una recuperación completa del servidor.  
  
> [!IMPORTANT]
>  A partir de Windows Server 2008, no se admite para restaurar la copia de seguridad a una nueva instalación de Windows Server en el nuevo hardware o el mismo hardware. Si se vuelve a instalar Windows Server en el mismo hardware, como se recomienda más adelante en esta guía, a continuación, puedes restaurar el controlador de dominio en el siguiente orden:  
>   
>  1.  Realizar una restauración completa del servidor para restaurar el sistema operativo y todos los archivos y aplicaciones.  
> 2.  Realizar una restauración del estado del sistema con wbadmin.exe para marcar SYSVOL como autorizado.  
>   
>  Para obtener más información, consulta el artículo [249694](https://support.microsoft.com/kb/249694).  
  
 Si se conoce el tiempo de la aparición del error, es necesario investigar más para identificar las copias de seguridad que contienen el último estado seguro del bosque. Este enfoque es menos deseable. Por lo tanto, te recomendamos que mantengas registros detallados sobre el estado de AD DS diariamente para que, si se produce un error de todo el bosque, puede identificar el tiempo aproximado de error. También debe mantener una copia local de las copias de seguridad para habilitar la recuperación más rápida.  
  
 Si se habilita la Papelera de reciclaje de Active Directory, la duración de la copia de seguridad es igual a la **deletedObjectLifetime** valor o el **tombstoneLifetime** valor, lo que sea menor. Para obtener más información, consulta [Papelera de reciclaje de Active Directory Step-by-Step guía](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).  
  
 Como alternativa, también puedes usar el Active Directory base de datos montaje herramienta (Dsamain.exe) y una herramienta de protocolo ligero de acceso a directorios (LDAP), como Ldp.exe o usuarios de Active Directory y equipos, para identificar qué copia de seguridad tiene el último estado seguro del bosque. La herramienta de montaje de base de datos de Active Directory, que se incluye en Windows Server 2008 y versiones posteriores sistemas operativos de Windows Server, expone los datos de Active Directory que se almacenan en las copias de seguridad o instantáneas como un servidor LDAP. A continuación, puedes usar una herramienta LDAP para examinar los datos. Este enfoque tiene la ventaja de que no requiere que se reinicie en modo de restauración de servicios de directorio (DSRM) para examinar el contenido de la copia de seguridad de AD DS de cualquier controlador de dominio.  
  
 Para obtener más información sobre el uso de la base de datos de Active Directory montaje de la herramienta, consulta el [herramienta de montaje de base de datos de Active Directory Step-by-Step guía](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).  
  
 También puedes usar la **ntdsutil instantánea** comando para crear instantáneas de la base de datos de Active Directory. Al programar una tarea para crear instantáneas de periódicamente, se pueden obtener copias adicionales de la base de datos de Active Directory con el tiempo. Puedes usar estas copias para identificar mejor cuando se produjo el error de todo el bosque y, a continuación, elige la copia de seguridad recomendado para restaurar. Para crear instantáneas, usa la versión de **ntdsutil** que se incluye con Windows Server 2008 o el servidor de administración remota herramientas (RSAT) para Windows Vista o versiones posteriores. El controlador de dominio de destino puede ejecutar cualquier versión de Windows Server. Para obtener más información sobre cómo usar la **ntdsutil instantánea** de comandos, consulta [instantánea](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).  
  
  
## <a name="determining-which-domain-controllers-to-restore"></a>Determinar qué controladores de dominio para restaurar  
 Accesibilidad del proceso de restauración es un factor importante cuando vayas a decidir qué controlador de dominio para restaurar. Se recomienda tener un controlador de dominio dedicado para cada dominio que es el controlador de dominio preferido para una restauración. Pruebas de restauración de una restauración dedicada DC, que es más fácil planear y ejecutar la recuperación de bosque porque se usa la misma configuración de origen que se usó para realizar de forma confiable. Puedes incluir la recuperación y competir con configuraciones diferentes, como si el controlador de dominio contiene operaciones de roles de maestro o no, o si es un servidor DNS o GC o no.  
  
> [!NOTE]
>  Mientras no se recomienda para restaurar el titular de una función de maestro de operaciones por motivos de simplicidad, algunas organizaciones pueden optar por restaurar uno para otras ventajas. Por ejemplo restaurar al maestro RID puede ayudar a evitar problemas con la administración de RID durante la recuperación.  
  
 Elegir un controlador de dominio que mejor se adapte a los siguientes criterios:  
  
-   Un controlador de dominio que se puede escribir. Esto es obligatorio.  
  
-   Un controlador de dominio que ejecute Windows Server 2012 como una máquina virtual en un hipervisor que admite VM-GenerationID. Este controlador de dominio puede usarse como origen para clonar.  
  
-   Un controlador de dominio que sea accesible, físicamente o en una red virtual y preferiblemente ubicado en un centro de datos. De esta forma, puede fácilmente aislar, desde la red durante la recuperación de bosque.  
  
-   Un controlador de dominio que tenga una copia de seguridad completa del servidor buena. Una buena copia de seguridad es una copia de seguridad que se puede restaurar correctamente, se tomó unos días antes de error y contiene como datos muy útiles como sea posible.  
  
-   Un controlador de dominio que era un servidor de sistema de nombres de dominio (DNS) antes del error. Esto ahorra el tiempo necesario para volver a instalar DNS.  
  
-   Si también usa los servicios de implementación de Windows, elige un controlador de dominio no está configurado para usar el desbloqueo de BitLocker en red. En este caso, el desbloqueo de BitLocker en red no se admite que se utilizará para el primer controlador de dominio que restaurar a partir de copia de seguridad durante la recuperación de un bosque.  
  
     Desbloqueo en red BitLocker como el *solo* protector de clave *no* usarse en controladores de dominio que han implementado servicios de implementación de Windows (WDS) porque hacerlo presenta en un escenario donde el primer controlador de dominio requiere Active Directory y WDS funciona para desbloquear. Pero antes de restaurar el primer controlador de dominio, Active Directory aún no está disponible para WDS, por lo que no se puede desbloquear.  
  
     Para determinar si un controlador de dominio está configurado para usar el desbloqueo de BitLocker en red, comprueba que un certificado de desbloqueo en red se identifica en la siguiente clave del registro:  
  
     HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP  
  
 Mantener procedimientos de seguridad al control o restaurar archivos de copia de seguridad que incluyen Active Directory. La urgencia que acompaña a la recuperación de bosque puede provocar involuntariamente alto los procedimientos recomendados de seguridad. Para obtener más información, consulta la sección titulada "Establecimiento de controlador de copia de seguridad y restaurar estrategias dominios" en [Guía de recomendaciones para proteger instalaciones de Active Directory y las operaciones cotidianas: parte II](https://technet.microsoft.com/library/bb727066.aspx).  
  
## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificar la estructura de bosque actual y funciones de controlador de dominio  
 Determinar la estructura de bosque actual mediante la identificación de todos los dominios del bosque. Hacer una lista de todos los controladores de dominio en cada dominio, especialmente los controladores de dominio que las copias de seguridad y los controladores de dominio virtualizadas que pueden ser un origen para clonar. Una lista de controladores de dominio raíz del bosque será más importantes, porque se recuperará este dominio primero. Después de restaurar el dominio raíz del bosque, puede obtener una lista de los otros dominios, controladores de dominio y los sitios en el bosque mediante el uso de complementos de Active Directory.  
  
 Preparar una tabla que muestra las funciones de cada controlador de dominio en el dominio, como se muestra en el siguiente ejemplo. Esto ayudará a volver a la configuración de error previos del bosque después de recuperación.  
  
|Nombre del controlador de dominio|Sistema operativo|FSMO|GC|RODC|Copia de seguridad|DNS|Server Core|MÁQUINA VIRTUAL|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Maestro de esquema, maestro nombres de dominio|Sí|No|Sí|No|No|Sí|Sí|  
|DC_2|Windows Server 2012|Ninguno|Sí|No|Sí|Sí|No|Sí|Sí|  
|DC_3|Windows Server 2012|Maestro de infraestructura|No|No|No|Sí|Sí|Sí|Sí|  
|DC_4|Windows Server 2012|Emulador PDC, eliminar maestro|Sí|No|No|No|No|Sí|No|  
|DC_5|Windows Server 2012|Ninguno|No|No|Sí|Sí|No|Sí|Sí|  
|RODC_1|Windows Server 2008 R2|Ninguno|Sí|Sí|Sí|Sí|Sí|Sí|No|  
|RODC_2|Windows Server 2008|Ninguno|Sí|Sí|No|Sí|Sí|Sí|No|  
  
 Para cada dominio en el bosque, identifica un controlador de dominio grabable único que tiene una copia de seguridad de confianza de la base de datos de Active Directory para ese dominio. Ten cuidado al elegir una copia de seguridad para restaurar un controlador de dominio. Si el día y la causa del error de aproximadamente se conocen, la recomendación general es usar una copia de seguridad que se realizó unos días antes de esa fecha.  
  
 En este ejemplo, hay cuatro candidatos de copia de seguridad: DC_1, DC_2, DC_4 y DC_5. Estos candidatos de copia de seguridad, restaurar solo uno. El controlador de dominio recomendada es DC_5 por los siguientes motivos:  
  
-   Cumple los requisitos para usarlo como un origen de clonación virtualizada de DC, que es, ejecuta Windows Server 2012 como un controlador de dominio virtual en un hipervisor que admite VM-GenerationID, se ejecuta software que se permite clonación (o que se pueden quitar si no puede ser clonados). Después de la restauración, el rol de emulador PDC se transferirse a que server y pueden agregarse al grupo de controladores de dominio clonación del dominio.  
  
-   Se ejecuta una instalación completa de Windows Server 2012. Un controlador de dominio que se ejecuta una instalación Server Core puede ser menos cómodo como destino para la recuperación.  
  
-   Es un servidor DNS. Por lo tanto, no tiene que reinstalarse DNS.  
  
> [!NOTE]
>  Como DC_5 no es un servidor de catálogo global, también tiene una ventaja que el catálogo global no deben quitarse después de la restauración. Pero si el controlador de dominio es también que un servidor de catálogo global no es un factor decisivo porque a partir de Windows Server 2012, o no todos los controladores de dominio son servidores de catálogo global de manera predeterminada y quitar y agregar el catálogo global después de que se recomienda la restauración como parte del proceso de recuperación de bosque en cualquier caso.  
  
## <a name="recover-the-forest-in-isolation"></a>Recuperar el bosque de manera aislada  
 El escenario preferido es apagar todos los controladores de dominio grabables antes de la primera restaurado se vuelve a producción. Esto garantiza que ningún dato peligroso no se replica en el bosque recuperado. Es especialmente importante apagar todos los titulares de rol de maestro de operaciones.  
  
> [!NOTE]
>  Puede haber casos donde mueves el primer controlador de dominio que deseas recuperar para cada dominio a una red aislada y permitir que otros controladores de dominio permanecer conectado a Internet para minimizar el tiempo de inactividad del sistema. Por ejemplo, si se realiza la recuperación de una actualización de esquema error, puedes mantener los controladores de dominio en la red de producción en ejecución mientras realizas pasos de recuperación de manera aislada.  
  
 Si estás ejecutando virtualizadas de controladores de dominio, puede moverlos a una red virtual que esté aislada de la red de producción donde realizará la recuperación. Mover virtualizada de controladores de dominio a una red independiente ofrece dos ventajas:  
  
-   Controladores de dominio recuperados se impide se reproduzca en el problema que causó la recuperación de bosque, ya que están aislados.  
  
-   Clonación de DC virtualizado puede realizada en la red independiente para que un número de controladores de dominio crítico se puede ejecutar y probar antes de que se volvió a la red de producción.  
  
 Si estás ejecutando controladores de dominio en hardware físico, desconecta el cable de red del primer controlador de dominio que deseas restaurar en el dominio raíz del bosque. Si es posible, también desconectar los cables de red de otros controladores de dominio. Esto impide que DC replicar, si se inician durante el proceso de recuperación de bosque accidentalmente.  
  
 En un bosque de gran tamaño que se propaga entre varias ubicaciones, puede ser difícil garantizar que todos los controladores de dominio grabables se cierran. Por este motivo, los pasos de recuperación, como restablecer la cuenta de equipo y la cuenta krbtgt, además de limpiar metadatos: están diseñados para garantizar que los controladores de dominio grabables recuperados no replican con controladores de dominio grabables peligrosos (en caso de algunas aún están en línea en el bosque).  
  
 ¿Sin embargo, solo al desconectar los controladores de dominio grabables puede garantiza que la replicación no se produce? Por lo tanto, siempre que sea posible, debe implementar la tecnología de administración remota que puede ayudarte a apagar y aislar físicamente los controladores de dominio grabables durante la recuperación de bosque.  
  
 RODC pueden continuar funcionando mientras controladores de dominio grabables están sin conexión. Ningún otro DC directamente replicará los cambios desde cualquier RODC, especialmente, ningún cambio de contenedor esquema o configuración, por lo que no representan el mismo riesgo como controladores de dominio grabables durante la recuperación. Después de todos los controladores de dominio grabables recuperada y en línea, debes recompilar el de los RODC.  
  
 RODC seguirá permitir el acceso a los recursos locales que se almacenan en caché en sus respectivos sitios mientras las operaciones de recuperación están en paralelo. Los recursos locales que no se almacenan en caché en el RODC tendrá reenviadas a un controlador de dominio grabable de solicitudes de autenticación. Estas solicitudes se producirá un error porque los controladores de dominio grabables están sin conexión. Algunas operaciones, como los cambios de contraseña también no funcionará hasta que lo recuperes controladores de dominio grabables.  
  
 Si estás usando una arquitectura de red de concentrador y radio, puede centrarse primero en recuperación de los controladores de dominio grabables en los sitios de concentrador. Más adelante, puede reconstruir el RODC en sitios remotos.  
  
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
