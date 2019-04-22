---
title: Recuperación de bosques de AD - determinar cómo recuperar el bosque
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 30d23d977b4d7cd320d78ff340120df7013dee4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817736"
---
# <a name="determine-how-to-recover-the-forest"></a>Determinar cómo recuperar el bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Recuperación de un bosque de Active Directory completo implica restaurarla a partir de copia de seguridad o volver a instalar los servicios de dominio de Active Directory (AD DS) en cada controlador de dominio (DC) en el bosque. Recuperación del bosque restaura cada dominio del bosque a su estado en el momento de la última copia de seguridad de confianza. Por lo tanto, la operación de restauración dará como resultado la pérdida de al menos los siguientes datos de Active Directory:

- Todos los objetos (por ejemplo, los usuarios y equipos) que se agregaron después de la última copia de seguridad de confianza
- Todas las actualizaciones que se han realizado a los objetos existentes desde la confianza de la última copia de seguridad
- Todos los cambios realizados a la partición de configuración o la partición del esquema en AD DS (por ejemplo, los cambios de esquema) desde la última copia de seguridad de confianza

Para cada dominio del bosque, debe conocer la contraseña de una cuenta de administrador de dominio. Si es posible, se trata de la contraseña de la cuenta de administrador integrada. También debe conocer la contraseña de DSRM para realizar una restauración del estado del sistema de un controlador de dominio. En general, resulta una buena práctica para archivar la cuenta de administrador y el historial de la contraseña DSRM en un lugar seguro para siempre y cuando las copias de seguridad son válidas, es decir, en el período de vigencia del objeto de desecho o dentro de la duración del objeto eliminado si Active Directory reciclaje Está habilitada la Papelera. También puede sincronizar la contraseña de DSRM con una cuenta de usuario de dominio para que resulte más fácil de recordar. Para obtener más información, consulte el artículo de KB [961320](https://support.microsoft.com/kb/961320). Sincronización de la cuenta de DSRM debe realizarse antes de la recuperación del bosque, como parte de preparación.

> [!NOTE]
> La cuenta Administrador es miembro del grupo de administradores integrado de forma predeterminada, como son los grupos Admins. del dominio y administradores de empresa. Este grupo tiene control total de todos los controladores de dominio en el dominio.

## <a name="determining-which-backups-to-use"></a>Determinar qué copias de seguridad para usar

Copia de seguridad al menos dos controladores de dominio grabables para cada dominio con regularidad para que tenga varias copias de seguridad que puede elegir. Tenga en cuenta que no se puede usar la copia de seguridad de un controlador de dominio de solo lectura (RODC) para restaurar un controlador de dominio grabable. Se recomienda que restaure los controladores de dominio mediante el uso de copias de seguridad que se tomaron durante unos días antes de la aparición del error. En general, debe determinar un equilibrio entre la recentness y la safeness de los datos restaurados. Elección de una copia de seguridad más reciente recupera los datos más útiles, pero podría aumentar el riesgo de volver a introducir datos peligrosos en el bosque restaurado.

Restaurar copias de seguridad de estado del sistema depende del sistema operativo y el servidor de la copia de seguridad original. Por ejemplo, no debe restaurar una copia de seguridad del estado del sistema a un servidor diferente. En este caso, es posible que vea la advertencia siguiente:

"La copia de seguridad especificada es de un servidor distinto al actual. No se recomienda realizar una recuperación del estado del sistema con la copia de seguridad en un servidor alternativo, porque el servidor podría quedar inutilizable. ¿Está seguro de que desea utilizar esta copia de seguridad para recuperar el servidor actual? "

Si necesita restaurar Active Directory a un hardware diferente, cree copias de seguridad de servidor completo y planea realizar una recuperación del servidor completo.

> [!IMPORTANT]
> A partir de Windows Server 2008, no se admite para restaurar la copia de seguridad de estado del sistema a una nueva instalación de Windows Server en el nuevo hardware o el mismo hardware. Si se vuelve a instalar Windows Server en el mismo hardware, como se recomienda más adelante en esta guía, a continuación, puede restaurar el controlador de dominio en este orden:
>
> 1. Realizar una restauración completa del servidor para restaurar el sistema operativo y todos los archivos y aplicaciones.
> 2. Realizar una restauración del estado del sistema con wbadmin.exe para marcar SYSVOL como autoritativo.
>
> Para obtener más información, vea Microsoft el artículo [249694](https://support.microsoft.com/kb/249694).

Si la hora de la aparición del error es desconocida, investigar en profundidad para identificar las copias de seguridad que contienen el último estado de prueba de errores del bosque. Este enfoque es menos deseable. Por lo tanto, se recomienda encarecidamente mantener registros detallados sobre el estado de mantenimiento de AD DS a diario para que, si se produce un error de todo el bosque, se puede identificar el tiempo aproximado de error. También debe tener una copia local de las copias de seguridad para permitir una recuperación más rápida.

Si está habilitada la Papelera de reciclaje de Active Directory, la duración de la copia de seguridad es igual a la **deletedObjectLifetime** valor o el **tombstoneLifetime** valor, lo que sea menor. Para obtener más información, consulte [Active Directory guía Papelera de reciclaje paso a paso](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).

Como alternativa, también puede usar la herramienta de montaje de base de datos de Active Directory (Dsamain.exe) y una herramienta de protocolo ligero de acceso a directorios (LDAP), como Ldp.exe o usuarios de Active Directory y los equipos, para identificar qué copia de seguridad tiene el último estado de seguridad de la bosque. La herramienta de montaje de base de datos de Active Directory, que se incluye en Windows Server 2008 y sistemas operativos de servidor de Windows posteriores, expone los datos de Active Directory que se almacenan en instantáneas o copias de seguridad como un servidor LDAP. A continuación, puede usar una herramienta LDAP para examinar los datos. Este enfoque tiene la ventaja de que no requiere que se reinicie cualquier controlador de dominio en modo de restauración de servicios de directorio (DSRM) para examinar el contenido de la copia de seguridad de AD DS.

Para obtener más información sobre cómo usar la herramienta de montaje de la base de datos de Active Directory, consulte el [Guía de paso a paso de herramienta de montaje de base de datos de Active Directory](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

También puede usar el **ntdsutil instantánea** comando para crear instantáneas de la base de datos de Active Directory. Mediante la programación de una tarea para crear instantáneas de forma periódica, puede obtener copias adicionales de la base de datos de Active Directory con el tiempo. Puede usar para identificar mejor cuando se produjo el error de todo el bosque y, a continuación, elija la mejor copia de seguridad para restaurar estas copias. Para crear las instantáneas, use la versión de **ntdsutil** que se incluye con Windows Server 2008 o el servidor de administración remota herramientas (RSAT) para Windows Vista o posterior. El controlador de dominio de destino puede ejecutar cualquier versión de Windows Server. Para obtener más información sobre el uso de la **ntdsutil instantánea** de comandos, consulte [instantánea](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Determinar qué controladores de dominio para restaurar

Facilitar el proceso de restauración es un factor importante al decidir qué controlador de dominio para restaurar. Se recomienda tener un controlador de dominio dedicado para cada dominio que es el controlador de dominio preferido para una restauración. Una restauración dedicada DC facilita a planear y ejecutar la recuperación de bosque debido a que usa la misma configuración de origen que se usó para realizar de forma confiable restaurar las pruebas. Puede incluir la recuperación y no competir con distintas configuraciones, como si el controlador de dominio contiene las operaciones de roles de maestro o no, o bien, ya sea un servidor DNS o GC o no.

> [!NOTE]
> Aunque no se recomienda para restaurar un titular de la función de maestro de operaciones por simplificar, algunas organizaciones pueden optar por restaurar uno para otras ventajas. Por ejemplo restaurar al maestro de RID puede ayudar a evitar problemas con la administración de RID durante la recuperación.  

Elija un controlador de dominio que mejor se adapte a los siguientes criterios:

- Un controlador de dominio es grabable. Esto es obligatorio.

- Un controlador de dominio que ejecutan Windows Server 2012 como una máquina virtual en un hipervisor que admita VM-GenerationID. Este controlador de dominio puede usarse como origen de clonación.
- Un controlador de dominio que es accesibles, ya sea físicamente o en una red virtual y, preferiblemente, ubicado en un centro de datos. De este modo, se puede fácilmente aislarla de la red durante la recuperación del bosque.
- Un controlador de dominio que tenga una copia de seguridad buena completa del servidor. Una buena copia de seguridad es una copia de seguridad que se puede restaurar correctamente, se toma unos días antes del error y que incluye como datos demasiado útiles como sea posible.
- Un controlador de dominio que era un servidor de sistema de nombres de dominio (DNS) antes del error. Esto reduce el tiempo necesario para volver a instalar DNS.
- Si también utiliza servicios de implementación de Windows, elija un controlador de dominio que no está configurado para usar el desbloqueo de BitLocker en red. En este caso, el desbloqueo de BitLocker en red no se admite que se usará para el primer controlador de dominio que va a restaurar desde copia de seguridad durante la recuperación del bosque.

   BitLocker desbloqueo en red como el *sólo* protector de clave *no* utilizarse en controladores de dominio donde ha implementado los servicios de implementación de Windows (WDS) porque hacerlo resultaría en un escenario donde se requiere el primer controlador de dominio Active Directory y WDS a trabajar con el fin de desbloquear. Pero antes de restaurar el primer controlador de dominio, Active Directory aún no está disponible para WDS, por lo que no se puede desbloquear.

   Para determinar si un controlador de dominio está configurado para usar el desbloqueo de BitLocker en red, compruebe que se identifica un certificado de desbloqueo en red en la siguiente clave del registro:

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Mantener los procedimientos de seguridad al control o restaurar los archivos de copia de seguridad que incluyen Active Directory. La urgencia que acompaña a la recuperación de bosques puede provocar accidentalmente pasando por alto los procedimientos recomendados de seguridad. Para obtener más información, vea la sección titulada "Establecimiento de controlador de copia de seguridad y restaurar estrategias de dominios" en [Best Practice Guide for Securing Active Directory Installations y las operaciones cotidianas: Parte II](https://technet.microsoft.com/library/bb727066.aspx).

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificar la estructura del bosque actual y las funciones de controlador de dominio

Determinar la estructura del bosque actual mediante la identificación de todos los dominios del bosque. Realice una lista de todos los controladores de dominio en cada dominio, especialmente los controladores de dominio que tienen copias de seguridad y los controladores de dominio virtualizados que pueden ser un origen de clonación. Una lista de controladores de dominio para el dominio raíz del bosque será el más importante porque recuperará en este dominio, en primer lugar. Después de restaurar el dominio raíz del bosque, puede obtener una lista de los demás dominios, los controladores de dominio y los sitios del bosque mediante los complementos de Active Directory.

Preparación de una tabla que muestra las funciones de cada controlador de dominio en el dominio, tal como se muestra en el ejemplo siguiente. Esto le permitirá volver a la configuración de un error previo del bosque después de la recuperación.

|Nombre de controlador de dominio|Sistema operativo|FSMO|GC|RODC|Copias de seguridad|DNS|Server Core|Máquina virtual|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Maestro de esquema, maestro de nomenclatura de dominio|Sí|No|Sí|No|No|Sí|Sí|  
|DC_2|Windows Server 2012|Ninguno|Sí|No|Sí|Sí|No|Sí|Sí|  
|DC_3|Windows Server 2012|Maestro de infraestructura|No|No|No|Sí|Sí|Sí|Sí|  
|DC_4|Windows Server 2012|Emulador de PDC, maestro de RID|Sí|No|No|No|No|Sí|No|  
|DC_5|Windows Server 2012|Ninguno|No|No|Sí|Sí|No|Sí|Sí|  
|RODC_1|Windows Server 2008 R2|Ninguno|Sí|Sí|Sí|Sí|Sí|Sí|No|  
|RODC_2|Windows Server 2008|Ninguno|Sí|Sí|No|Sí|Sí|Sí|No|  

Para cada dominio del bosque, identificar un controlador de dominio grabable único que tiene una copia de seguridad de confianza de la base de datos de Active Directory para ese dominio. Tenga cuidado al elegir una copia de seguridad para restaurar un controlador de dominio. Si se conocen aproximadamente el día y la causa del error, la recomendación general es usar una copia de seguridad que se realizó unos días antes de esa fecha.
  
En este ejemplo, hay cuatro candidatos de copia de seguridad: DC_1, DC_2, DC_4 y DC_5. De estos candidatos de copia de seguridad, restaurar solo uno. El controlador de dominio recomendada es DC_5 por las razones siguientes:  

- Cumple los requisitos para usarlo como origen para la clonación de controlador de dominio virtualizado, que es, ejecuta Windows Server 2012 como un controlador de dominio virtual en un hipervisor que admita VM-GenerationID, se ejecuta software que puede ser clonado (o que se puede quitar si no puede ser clon (d). Después de la restauración, el rol de emulador PDC se asumirse a que server y se pueden agregar al grupo de dominio clonables para el dominio.  
- Se ejecuta una instalación completa de Windows Server 2012. Un controlador de dominio que ejecuta una instalación Server Core puede ser poco práctica como destino para la recuperación.  
- Es un servidor DNS. Por lo tanto, DNS no tiene que volver a instalarse.  

> [!NOTE]
> Dado que DC_5 no es un servidor de catálogo global, también tiene una ventaja que no deben quitarse después de la restauración del catálogo global. Pero si el controlador de dominio también es que un servidor de catálogo global no es un factor decisivo porque a partir de Windows Server 2012, todos los controladores de dominio son servidores de catálogo global de forma predeterminada y quitando y el catálogo global se agrega después de que la restauración se recomienda como parte del bosque en cualquier caso procesos de recuperación.  

## <a name="recover-the-forest-in-isolation"></a>Recuperar el bosque de forma aislada

El escenario preferido es apagar todos los controladores de dominio grabables antes del primer controlador de dominio restaurado vuelve a producción. Esto garantiza que todos los datos peligrosos no se replican en el bosque recuperado. Es especialmente importante apagar todos los titulares de función de maestro de operaciones.  

> [!NOTE]
> Puede haber casos donde se mueva el primer controlador de dominio que va a recuperar para cada dominio a una red aislada, al tiempo que permite a otros controladores de dominio permanezcan en línea con el fin de minimizar el tiempo de inactividad del sistema. Por ejemplo, si va a recuperar de una actualización con errores de esquema, puede mantener controladores de dominio que se ejecutan en la red de producción mientras realiza los pasos de recuperación de forma aislada.

Si utiliza controladores de dominio virtualizados, puede moverlos a una red virtual que está aislada de la red de producción donde realizará la recuperación. Mover controladores de dominio virtualizados a una red independiente ofrece dos ventajas:  

- Los controladores de dominio recuperados se impide que se reproduzca el problema que provocó la recuperación de bosques, ya que están aislados.  
- Clonación de controlador de dominio virtualizado puede realizada en la red independiente para que se puede ejecutar un número de controladores de dominio crítico y probada antes de que regresa a la red de producción.

Si utiliza controladores de dominio en el hardware físico, desconecte el cable de red del primer controlador de dominio que va a restaurar en el dominio raíz del bosque. Si es posible, también desconecte los cables de red de todos los demás controladores de dominio. Esto impide que los controladores de dominio replicar, si se han iniciado por accidente durante el proceso de recuperación del bosque.  

En un bosque de gran tamaño que se reparte entre varias ubicaciones, puede ser difícil garantizar que todos los controladores de dominio grabables se apagan. Por este motivo, los pasos de recuperación, como el restablecimiento de la cuenta de equipo y la cuenta krbtgt, además a la limpieza de metadatos: están diseñados para garantizar que los controladores de dominio grabables recuperados no se replican con controladores de dominio grabables peligrosos (en caso de algunos aún están en línea en el bosque).  
  
Sin embargo, sólo al desconectar los controladores de dominio grabables puede se garantiza que la replicación no se produce. Por lo tanto, siempre que sea posible, debe implementar la tecnología de administración remota que puede ayudarle a apagar y aislar físicamente los controladores de dominio grabables durante la recuperación del bosque.  
  
Los RODC pueden continuar funcionando mientras los controladores de dominio grabables están sin conexión. Directamente ningún otro DC replicará los cambios de los RODC, especialmente, ningún cambio de contenedor de esquema o la configuración, por lo que no supongan un riesgo mismo como controladores de dominio grabables durante la recuperación. Una vez recuperada y en línea todos los controladores de dominio grabables, debe volver a generar todos los RODC.  
  
Los RODC continuará permitir el acceso a los recursos locales que se almacenan en caché en sus respectivos sitios mientras que las operaciones de recuperación están ocurriendo en paralelo. Los recursos locales que se almacenan en caché en el RODC no tendrán las solicitudes de autenticación que se reenvían a un controlador de dominio grabable. Estas solicitudes se producirá un error porque los controladores de dominio grabables están sin conexión. Algunas operaciones, como los cambios de contraseña también no funcionará hasta que recuperar los controladores de dominio grabables.  
  
Si usa una arquitectura de red de concentrador y radio, puede concentrarse primero en la recuperación de los controladores de dominio grabables en los sitios de concentrador. Más adelante, puede volver a generar el RODC en sitios remotos.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de bosques de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosques de AD - idear un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosques de AD: identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación de bosques de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosques de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosques de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosques de AD - recuperar un único dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosques de AD: recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
