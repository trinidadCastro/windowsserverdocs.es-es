---
title: 'Recuperación del bosque de AD: determinar cómo recuperar el bosque'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fea55dc5551198f7bc06afb2ec38077398b9cf77
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824058"
---
# <a name="determine-how-to-recover-the-forest"></a>Determinar cómo recuperar el bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

La recuperación de un bosque de Active Directory completo implica su restauración a partir de una copia de seguridad o la reinstalación de Active Directory Domain Services (AD DS) en cada controlador de dominio (DC) del bosque. La recuperación del bosque restaura cada dominio del bosque a su estado en el momento de la última copia de seguridad de confianza. Por consiguiente, la operación de restauración producirá la pérdida de al menos los siguientes datos de Active Directory:

- Todos los objetos (como usuarios y equipos) que se agregaron después de la última copia de seguridad de confianza
- Todas las actualizaciones que se realizaron en los objetos existentes desde la última copia de seguridad de confianza
- Todos los cambios que se realizaron en la partición de configuración o en la partición de esquema en AD DS (por ejemplo, cambios de esquema) desde la última copia de seguridad de confianza

Para cada dominio del bosque, debe conocerse la contraseña de una cuenta de administrador de dominio. Preferiblemente, se trata de la contraseña de la cuenta de administrador integrada. También debe conocer la contraseña de DSRM para realizar una restauración del estado del sistema de un controlador de dominio. En general, es recomendable archivar la cuenta de administrador y el historial de la contraseña de DSRM en un lugar seguro mientras las copias de seguridad sean válidas, es decir, dentro del período de duración de objetos de desecho o en el período de vigencia del objeto eliminado si la papelera de reciclaje de Active Directory está habilitada. También puede sincronizar la contraseña de DSRM con una cuenta de usuario de dominio para que sea más fácil de recordar. Para obtener más información, consulte el artículo [961320](https://support.microsoft.com/kb/961320)de Knowledge base. La sincronización de la cuenta DSRM debe realizarse antes de la recuperación del bosque, como parte de la preparación.

> [!NOTE]
> De forma predeterminada, la cuenta Administrador es miembro del grupo administradores integrado, como son los grupos Admins. del dominio y administradores de organización. Este grupo tiene control total sobre todos los controladores de dominio del dominio.

## <a name="determining-which-backups-to-use"></a>Determinación de las copias de seguridad que se van a usar

Realice una copia de seguridad de al menos dos controladores de dominio grabables para cada dominio con regularidad para que pueda elegir entre varias copias de seguridad. Tenga en cuenta que no puede usar la copia de seguridad de un controlador de dominio de solo lectura (RODC) para restaurar un controlador de dominio grabable. Se recomienda restaurar los controladores de seguridad mediante el uso de copias de seguridad que se realizaron unos días antes de que se produjera el error. En general, debe determinar un equilibrio entre la reciente y la seguridad de los datos restaurados. La elección de una copia de seguridad más reciente recupera datos más útiles, pero podría aumentar el riesgo de reintroducir los datos peligrosos en el bosque restaurado.

La restauración de las copias de seguridad del estado del sistema depende del sistema operativo y del servidor de la copia de seguridad. Por ejemplo, no debe restaurar una copia de seguridad de estado del sistema en un servidor diferente. En este caso, puede ver la siguiente ADVERTENCIA:

"La copia de seguridad especificada es de un servidor diferente del actual. No se recomienda realizar una recuperación del estado del sistema con la copia de seguridad en un servidor alternativo, ya que el servidor podría quedar inutilizable. ¿Está seguro de que desea usar esta copia de seguridad para recuperar el servidor actual? "

Si necesita restaurar Active Directory en hardware diferente, cree copias de seguridad completas del servidor y planee realizar una recuperación completa del servidor.

> [!IMPORTANT]
> A partir de Windows Server 2008, no se admite la restauración de la copia de seguridad del estado del sistema en una nueva instalación de Windows Server en hardware nuevo o en el mismo hardware. Si Windows Server se vuelve a instalar en el mismo hardware, como se recomienda más adelante en esta guía, puede restaurar el controlador de dominio en este orden:
>
> 1. Realice una restauración completa del servidor para restaurar el sistema operativo y todos los archivos y aplicaciones.
> 2. Realice una restauración del estado del sistema con Wbadmin. exe para marcar SYSVOL como autoritativo.
>
> Para obtener más información, consulte el artículo [249694](https://support.microsoft.com/kb/249694)de Microsoft Knowledge base.

Si no se conoce la hora de la aparición del error, investigue más para identificar las copias de seguridad que contienen el último estado de seguridad del bosque. Este enfoque es menos deseable. Por lo tanto, se recomienda encarecidamente que mantenga registros detallados sobre el estado de mantenimiento de AD DS diariamente, de modo que, si se produce un error en todo el bosque, se pueda identificar el tiempo aproximado de error. También debe mantener una copia local de las copias de seguridad para permitir una recuperación más rápida.

Si Active Directory papelera de reciclaje está habilitada, la duración de la copia de seguridad es igual al valor de **deletedObjectLifetime** o al valor de **tombstoneLifetime** , lo que sea menor. Para obtener más información, vea [Active Directory guía paso a paso](https://go.microsoft.com/fwlink/?LinkId=178657) de la papelera de reciclaje (https://go.microsoft.com/fwlink/?LinkId=178657).

Como alternativa, también puede usar la herramienta de montaje de bases de datos Active Directory (DSAMain. exe) y una herramienta LDAP (Protocolo ligero de acceso a directorios), como LDP. exe o Active Directory usuarios y equipos, para identificar qué copia de seguridad tiene el último estado de seguridad del bosque. La herramienta de montaje de bases de datos de Active Directory, que se incluye en los sistemas operativos Windows Server 2008 y versiones posteriores, expone Active Directory datos almacenados en copias de seguridad o instantáneas como un servidor LDAP. A continuación, puede usar una herramienta LDAP para examinar los datos. Este enfoque tiene la ventaja de que no requiere reiniciar ningún controlador de dominio en Modo de restauración de servicios de directorio (DSRM) para examinar el contenido de la copia de seguridad de AD DS.

Para obtener más información sobre el uso de la herramienta de montaje de bases de datos de Active Directory, consulte la [Guía paso a paso de la herramienta de montaje de Active Directory Database](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

También puede usar el comando de **instantánea de Ntdsutil** para crear instantáneas de la base de datos de Active Directory. Mediante la programación de una tarea para crear instantáneas periódicamente, puede obtener copias adicionales de la base de datos Active Directory a lo largo del tiempo. Puede usar estas copias para identificar mejor Cuándo se ha producido el error en todo el bosque y, a continuación, elegir la mejor copia de seguridad para restaurar. Para crear instantáneas, utilice la versión de **Ntdsutil** que se incluye con windows Server 2008 o el herramientas de administración remota del servidor (RSAT) para Windows Vista o posterior. El controlador de dominio de destino puede ejecutar cualquier versión de Windows Server. Para obtener más información acerca del uso del comando de **instantánea de Ntdsutil** , consulte [Snapshot](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Determinar los controladores de dominio que se van a restaurar

La facilidad del proceso de restauración es un factor importante a la hora de decidir qué controlador de dominio desea restaurar. Se recomienda tener un DC dedicado para cada dominio que sea el controlador de dominio preferido para una restauración. Un controlador de dominio de restauración dedicado facilita el planeamiento y la ejecución de la recuperación del bosque, ya que se usa la misma configuración de origen que se utilizó para realizar pruebas de restauración. Puede crear un script para la recuperación y no lidiar con distintas configuraciones, como si el DC contiene roles de maestro de operaciones o no, o si es un servidor de GC o DNS o no.

> [!NOTE]
> Aunque no se recomienda restaurar el titular de la función de maestro de operaciones en aras de la simplicidad, algunas organizaciones pueden optar por restaurar una para otras ventajas. Por ejemplo, restaurar el maestro RID puede ayudar a evitar problemas con la administración de RID durante la recuperación.  

Elija un controlador de dominio que se adapte mejor a los siguientes criterios:

- Un controlador de dominio que se va A escribir. Esto es necesario.

- Un controlador de dominio que ejecuta Windows Server 2012 como una máquina virtual en un hipervisor que admite VM-GenerationID. Este controlador de dominio se puede usar como origen para la clonación.
- Un controlador de dominio al que se puede tener acceso, ya sea físicamente o en una red virtual, y que, preferiblemente, se encuentra en un centro de recursos. De este modo, puede aislarla fácilmente de la red durante la recuperación del bosque.
- Un controlador de dominio que tenga una buena copia de seguridad completa del servidor. Una buena copia de seguridad es una copia de seguridad que se puede restaurar correctamente, se tomó unos días antes del error y contiene tantos datos útiles como sea posible.
- Un DC que era un servidor de sistema de nombres de dominio (DNS) antes de que se produjera el error. Esto ahorra el tiempo necesario para reinstalar DNS.
- Si también usa servicios de implementación de Windows, elija un controlador de dominio que no esté configurado para usar el desbloqueo de red de BitLocker. En este caso, no se admite el desbloqueo de red de BitLocker para el primer controlador de dominio que se restaura a partir de una copia de seguridad durante la recuperación de un bosque.

   Desbloqueo de red de BitLocker como el *único* protector de clave *no se puede* usar en controladores de dominio en los que ha implementado los servicios de implementación de Windows (WDS) porque al hacerlo se produce un escenario en el que el primer DC requiere Active Directory y que WDS funcione para poder desbloquearlo. Pero antes de restaurar el primer controlador de dominio, Active Directory todavía no está disponible para WDS, por lo que no se puede desbloquear.

   Para determinar si un controlador de dominio está configurado para usar el desbloqueo de red de BitLocker, compruebe que se ha identificado un certificado de desbloqueo de red en la siguiente clave del registro:

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Mantenga los procedimientos de seguridad al administrar o restaurar archivos de copia de seguridad que incluyan Active Directory. La urgencia que acompaña a la recuperación del bosque puede conducir involuntariamente a las prácticas recomendadas de seguridad. Para obtener más información, vea la sección titulada "establecer estrategias de copia de seguridad y restauración del controlador de dominio" en la [Guía de procedimientos recomendados para proteger las instalaciones de Active Directory y las operaciones cotidianas: parte II](https://technet.microsoft.com/library/bb727066.aspx).

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificación de la estructura del bosque actual y las funciones del controlador de dominio

Determinar la estructura del bosque actual identificando todos los dominios del bosque. Cree una lista de todos los controladores de dominio de cada dominio, especialmente los controladores de dominio que tienen copias de seguridad, y los controladores de dominio virtualizados que pueden ser un origen para la clonación. Una lista de controladores de dominio para el dominio raíz del bosque será la más importante porque recuperará primero este dominio. Después de restaurar el dominio raíz del bosque, puede obtener una lista de los demás dominios, controladores de dominio y sitios del bosque mediante el uso de complementos de Active Directory.

Prepare una tabla que muestre las funciones de cada DC del dominio, tal como se muestra en el ejemplo siguiente. Esto le ayudará a volver a la configuración de error previa del bosque después de la recuperación.

|Nombre del controlador de dominio|Sistema operativo|FSMO|GC|RODC|Backup|DNS|Server Core|Máquina virtual|VM: genio|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Maestro de esquema, maestro de nomenclatura de dominios|Sí|No|Sí|No|No|Sí|Sí|  
|DC_2|Windows Server 2012|Ninguno|Sí|No|Sí|Sí|No|Sí|Sí|  
|DC_3|Windows Server 2012|Maestro de infraestructura|No|No|No|Sí|Sí|Sí|Sí|  
|DC_4|Windows Server 2012|Emulador de PDC, maestro RID|Sí|No|No|No|No|Sí|No|  
|DC_5|Windows Server 2012|Ninguno|No|No|Sí|Sí|No|Sí|Sí|  
|RODC_1|Windows Server 2008 R2|Ninguno|Sí|Sí|Sí|Sí|Sí|Sí|No|  
|RODC_2|Windows Server 2008|Ninguno|Sí|Sí|No|Sí|Sí|Sí|No|  

Para cada dominio del bosque, identifique un solo DC grabable que tenga una copia de seguridad de confianza de la base de datos de Active Directory para ese dominio. Tenga cuidado al elegir una copia de seguridad para restaurar un controlador de dominio. Si el día y la causa del error son aproximadamente conocidos, la recomendación general es usar una copia de seguridad que se haya realizado unos días antes de esa fecha.
  
En este ejemplo, hay cuatro candidatos de copia de seguridad: DC_1, DC_2, DC_4 y DC_5. De estos candidatos de copia de seguridad, solo se restaura uno. El controlador de dominio recomendado es DC_5 por los siguientes motivos:  

- Cumple los requisitos para usarlo como origen de la clonación de controladores de dominio virtualizados, es decir, ejecuta Windows Server 2012 como un controlador de dominio virtual en un hipervisor que admite VM-GenerationID, ejecuta el software que se permite clonar (o que se puede quitar si no es capaz de clonarse). Después de la restauración, el rol de emulador de PDC se asumirá para ese servidor y se puede Agregar al grupo controladores de dominio clonables del dominio.  
- Ejecuta una instalación completa de Windows Server 2012. Un controlador de dominio que ejecute una instalación Server Core puede ser menos práctico como destino de la recuperación.  
- Es un servidor DNS. Por lo tanto, no es necesario volver a instalar DNS.  

> [!NOTE]
> Dado que DC_5 no es un servidor de catálogo global, también tiene la ventaja de que no es necesario quitar el catálogo global después de la restauración. Pero si el controlador de dominio también es un servidor de catálogo global no es un factor decisivo, ya que a partir de Windows Server 2012, todos los controladores de dominio son servidores de catálogo global de forma predeterminada, y quitar y agregar el catálogo global una vez que se recomienda la restauración como parte del proceso de recuperación del bosque en cualquier caso.  

## <a name="recover-the-forest-in-isolation"></a>Recuperación del bosque en aislamiento

El escenario preferido es apagar todos los controladores de dominio grabables antes de que el primer controlador de dominio restaurado vuelva a producción. Esto garantiza que los datos peligrosos no se vuelvan a replicar en el bosque recuperado. Es especialmente importante apagar todos los titulares de la función de maestro de operaciones.  

> [!NOTE]
> Puede haber casos en los que mueva el primer DC que planea recuperar para cada dominio a una red aislada, a la vez que permite que otros controladores de dominio permanezcan en línea para minimizar el tiempo de inactividad del sistema. Por ejemplo, si va a realizar la recuperación a partir de una actualización de esquema con errores, puede optar por mantener los controladores de dominio que se ejecutan en la red de producción mientras realiza los pasos de recuperación de aislamiento.

Si está ejecutando controladores de red virtualizados, puede moverlos a una red virtual que esté aislada de la red de producción en la que realizará la recuperación. Mover los controladores de red virtualizados a una red independiente ofrece dos ventajas:  

- Los controladores de DC recuperados no se pueden reproducir el problema que provocó la recuperación del bosque porque están aislados.  
- La clonación de controladores de dominio virtualizados se puede realizar en la red independiente para que se pueda ejecutar y probar un número crítico de controladores de dominio antes de que vuelvan a la red de producción.

Si está ejecutando controladores de dominio en hardware físico, desconecte el cable de red del primer controlador de dominio que vaya a restaurar en el dominio raíz del bosque. Si es posible, desconecte también los cables de red de los demás controladores de red. Esto impide que los controladores de la replicación se repliquen si se inician accidentalmente durante el proceso de recuperación del bosque.  

En un bosque grande que está distribuido en varias ubicaciones, puede ser difícil garantizar que todos los controladores de DC que se pueden escribir están apagados. Por esta razón, los pasos de recuperación, como el restablecimiento de la cuenta de equipo y la cuenta de krbtgt, además de la limpieza de metadatos, están diseñados para garantizar que los controladores de seguridad de escritura recuperados no se replican con controladores de seguridad de escritura peligrosos (en el caso de que algunos estén aún en línea en el bosque).  
  
Sin embargo, solo si los DC que se pueden escribir están sin conexión, puede garantizar que no se produzca la replicación. Por lo tanto, siempre que sea posible, debe implementar tecnología de administración remota que pueda ayudarle a apagar y aislar físicamente los controladores de DC grabables durante la recuperación del bosque.  
  
Los RODC pueden seguir funcionando mientras los DC grabables están sin conexión. Ningún otro controlador de dominio replicará directamente ningún cambio de ningún RODC, especialmente, no cambia el esquema ni el contenedor de configuración, por lo que no suponen el mismo riesgo que los controladores de dominio grabables durante la recuperación. Después de que todos los controladores de seguridad que se pueden escribir se recuperen y estén en línea, debe volver a generar todos los RODC.  
  
Los RODC seguirán permitiendo el acceso a los recursos locales que se almacenan en caché en sus sitios respectivos mientras las operaciones de recuperación se están realizando en paralelo. Los recursos locales que no estén almacenados en caché en el RODC tendrán solicitudes de autenticación reenviadas a un controlador de dominio grabable. Estas solicitudes producirán un error porque los DC que se pueden escribir están sin conexión. Algunas operaciones, como los cambios de contraseña, no funcionarán hasta que recupere los controladores de DC que se pueden escribir.  
  
Si usa una arquitectura de red de concentrador y radio, puede concentrarse primero en la recuperación de los controladores de red que se pueden escribir en los sitios del concentrador. Posteriormente, puede volver a generar los RODC en sitios remotos.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
