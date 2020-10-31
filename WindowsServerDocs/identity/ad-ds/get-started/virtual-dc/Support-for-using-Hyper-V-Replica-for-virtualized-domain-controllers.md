---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Compatibilidad de la réplica de Hyper-V con controladores de dominio virtualizados
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0a8d59da05f7dbf675114c96ceac5e755b06a66a
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071057"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Compatibilidad de la réplica de Hyper-V con controladores de dominio virtualizados

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica la compatibilidad para usar Réplica de Hyper-V con objeto de replicar una máquina virtual (VM) que se ejecuta como un controlador de dominio (DC). Réplica de Hyper-V es una nueva capacidad de Hyper-V incluida a partir de Windows Server 2012 que proporciona un mecanismo integrado de replicación en el nivel de una VM.

Las VM seleccionadas se replican asincrónicamente desde un host de Hyper-V principal con un host de Hyper-V de réplica mediante vínculos LAN o WAN. Una vez completada la replicación inicial, los cambios posteriores se replican según el intervalo que el administrador haya definido.

La conmutación por error puede ser planeada o no planeada. Una conmutación por error planeada debe iniciarla un administrador en la VM principal, y todos los cambios que no se hayan replicado se copian en la VM de réplica para no perder datos, mientras que una conmutación por error no planeada se inicia en la VM de réplica como respuesta a un error inesperado que ha tenido lugar en la VM principal. En este caso sí se pueden perder datos, ya que no existe la posibilidad de transmitir los cambios en la VM principal que no hayan podido replicarse aún.

Para obtener información adicional sobre la Réplica de Hyper-V, consulte [Introducción a Réplica de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134172(v=ws.11)) y [Implementar la réplica de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134207(v=ws.11)).

> [!NOTE]
> Réplica de Hyper-V solo se puede ejecutar en Windows Server Hyper-V, y no en la versión de Hyper-V que se ejecuta en Windows 8.

## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>Se requieren Windows Server 2012 o controladores de dominio más recientes

Windows Server 2012 Hyper-V presentó VM-GenerationID (VMGenID). VMGenID constituye una forma por parte del hipervisor de comunicarse con el SO invitado cuando han tenido lugar cambios reseñables. Por ejemplo, el hipervisor se puede comunicar con un DC virtualizado donde ha tenido lugar una restauración desde una instantánea (tecnología de restauración de instantánea de Hyper-V, no restauración de copia de seguridad). AD DS en Windows Server 2012 y versiones más recientes es consciente de la tecnología de máquina virtual VMGenID y la usa para detectar cuándo se realizan las operaciones de hipervisor, como la restauración de instantáneas, lo que le permite protegerse mejor.

> [!NOTE]
> Solo AD DS en controladores de DC de Windows Server 2012 o más recientes proporcionan estas medidas de seguridad resultantes de VMGenID; Los controladores de dominio que ejecutan todas las versiones anteriores de Windows Server están sujetos a problemas como la reversión de USN que puede producirse cuando se restaura un controlador de dominio virtualizado mediante un mecanismo no compatible, como la restauración de instantáneas. Para obtener más información sobre estas medidas de seguridad y cuándo se activan, consulte [arquitectura de controladores de dominio virtualizados](./virtualized-domain-controller-architecture.md).

Cuando se produce una conmutación por error de réplica de Hyper-V (planeada o no planeada), el controlador de dominio virtualizado detecta un restablecimiento de VMGenID y desencadena las características de seguridad anteriores. Tras esto, Active Directory prosigue con su funcionamiento habitual. La VM de réplica se ejecuta como la VM principal.

> [!NOTE]
> Ahora que hay dos instancias de la misma identidad de DC, existe la posibilidad de que tanto la instancia principal como la replicada se ejecuten. Si bien Réplica de Hyper-V está dotado de mecanismos de control para garantizar que las VM principal y de réplica no se ejecutan al mismo tiempo, esto puede suceder si se produce un error en el vínculo entre ambas después de la replicación de la VM. En el improbable caso de que esto ocurra, los controles de dominio virtualizados que ejecutan Windows Server 2012 cuentan con medidas de seguridad para proteger AD DS, pero no así los controles de dominio virtualizados que ejecutan versiones anteriores de Windows Server.

Al utilizar la réplica de Hyper-V, asegúrese de seguir las prácticas recomendadas para [ejecutar controladores de dominio virtuales en Hyper-V](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)). Este artículo incluye recomendaciones para, por ejemplo, almacenar archivos de Active Directory en discos SCSI virtuales, lo que constituye una mayor garantía de durabilidad de los datos.

## <a name="supported-and-unsupported-scenarios"></a>Escenarios admitidos y no admitidos

Solo se admiten las máquinas virtuales que ejecutan Windows Server 2012 o posterior para la conmutación por error no planeada y para probar la conmutación por error. Incluso en el caso de la conmutación por error planeada, se recomienda Windows Server 2012 o una versión más reciente para el controlador de dominio virtualizado a fin de mitigar los riesgos en caso de que un administrador inicie involuntariamente la máquina virtual principal y la máquina virtual replicada al mismo tiempo.

Las VM que ejecutan versiones anteriores de Windows Server se pueden usar en las conmutaciones por error planeadas, pero no en las no planeadas, dadas las probabilidades de que se produzca una reversión de USN. Para obtener más información acerca de la reversión de USN, vea [USN y reversión de USN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)).

> [!NOTE]
> No existen requisitos de nivel funcional de dominio o bosque, sino únicamente requisitos de sistema operativo para los controladores de dominio que se ejecutan como VM y que se replican mediante Réplica de Hyper-V. Las VM se pueden implementar en un bosque que contenga otros controladores de dominio físicos o virtuales donde se ejecute una versión anterior de Windows Server y que pueden o no replicarse a través de Réplica de Hyper-V.

Esta afirmación de compatibilidad se sustenta en diversas pruebas que se han realizado en un bosque con un solo dominio, si bien también se admiten configuraciones de bosque multidominio. En dichas pruebas, los controladores de dominio virtualizados DC1 y DC2 son asociados de replicación de Active Directory hospedados en el mismo sitio y en un servidor que ejecuta Hyper-V en Windows Server 2012. Réplica de Hyper-V está habilitado en el invitado de VM. El servidor Réplica está hospedado en un centro de datos alejado geográficamente. Para poder entender mejor los procesos del caso de prueba que vamos a explicar a continuación, haremos referencia a la VM que ejecuta el servidor Réplica como DC2-Rec (aunque su nombre sea de facto el mismo que el de la VM original).

### <a name="windows-server-2012"></a>Windows Server 2012

En la siguiente tabla se detalla la compatibilidad de los controladores de dominio virtualizados que ejecutan Windows Server 2012, así como los casos de prueba.

| Conmutación por error planeada | Conmutación por error no planeada |
|--|--|
| Compatible. | Compatible. |
| Caso de prueba:<p>-DC1 y DC2 ejecutan Windows Server 2012.<p>-DC2 se apaga y se realiza una conmutación por error en DC2-Rec. La conmutación por error puede ser planeada o no planeada.<p>-Después de que se inicia DC2-Rec, comprueba si el valor de VMGenID que tiene en su base de datos es el mismo que el valor del controlador de la máquina virtual guardado por el servidor de réplica de Hyper-V.<p>-Como resultado, DC2-Rec desencadena medidas de seguridad de virtualización; en otras palabras, restablece su identificador de invocación, descarta su grupo de RID y establece un requisito de sincronización inicial antes de asumir un rol de maestro de operaciones. Para obtener más información sobre el requisito de sincronización inicial, consulta .<p>-DC2-Rec, a continuación, guarda el nuevo valor de VMGenID en su base de datos y confirma las actualizaciones posteriores en el contexto del nuevo invocación.<p>-Como resultado del restablecimiento del invocación, DC1 convergirá en todos los cambios de AD introducidos por DC2-Rec incluso si se revirtió en el tiempo, lo que significa que cualquier actualización de AD realizada en DC2-Rec después de la conmutación por error convergerá de forma segura | El caso de prueba es el mismo que el de una conmutación por error planeada, salvo por las siguientes excepciones:<p>-Todas las actualizaciones de AD recibidas en DC2, pero que AD no haya replicado aún en un asociado de replicación antes de que se pierda el evento de conmutación por error.<p>-Las actualizaciones de AD recibidas en DC2 después de la hora del punto de recuperación replicado por AD en DC1 se replicarán desde DC1 de vuelta a DC2-Rec. |

### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 y versiones anteriores

En la siguiente tabla se refleja la compatibilidad de los controladores de dominio virtualizados que ejecutan Windows Server 2008 R2 y versiones anteriores.

| Conmutación por error planeada | Conmutación por error no planeada |
|--|--|
| Compatible, pero no recomendable, ya que los controladores de dominio que ejecutan estas versiones de Windows Server no admiten VMGenID y carecen de medidas de seguridad de virtualización asociadas. Esto supone un riesgo de reversión de USN. Para obtener más información, vea [USN y reversión de USN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)). | No compatible<p>**Nota:** La conmutación por error no planeada se admitirá cuando la reversión de USN no sea un riesgo, como un único controlador de dominio en el bosque (una configuración que no se recomienda). |
| Caso de prueba:<p>-DC1 y DC2 ejecutan Windows Server 2008 R2.<p>-DC2 se apaga y se realiza una conmutación por error planeada en DC2-Rec. Todos los datos en DC2 se replican en DC2-Rec antes de que se complete el apagado.<p>-Después de que se inicia DC2-Rec, reanuda la replicación con DC1 usando el mismo invocación que DC2. | N/D |
