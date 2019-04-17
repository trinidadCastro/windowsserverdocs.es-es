---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: "Compatibilidad de réplica de Hyper-V para controladores de dominio virtualizada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0444198196ed08a22aba92a0f59cc6e7a2518a2e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Compatibilidad de réplica de Hyper-V para controladores de dominio virtualizada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica la compatibilidad del uso de réplica de Hyper-V para replicar una máquina virtual (VM) que se ejecuta como un controlador de dominio (corriente continua). Réplica de Hyper-V es una nueva funcionalidad de Windows Server 2012 que proporciona un mecanismo de replicación integrado en un nivel de la máquina virtual a partir de Hyper-V.  
  
Hyper-V réplica replica asincrónicamente máquinas virtuales seleccionadas desde un host de Hyper-V principal a un host de Hyper-V réplica a través de LAN o vínculos WAN. Una vez completada la replicación inicial, los cambios subsiguientes se replicarán en un intervalo definido por el administrador.  
  
Puede ser la conmutación por error planeada o no. Se inicia una conmutación por error planeada por un administrador en la máquina virtual principal, y los cambios no replicados se copiarán a la réplica de VM para evitar la pérdida de datos. En la máquina virtual de réplica en respuesta a un error inesperado de la máquina virtual principal, se inicia una conmutación por error imprevisto. Pérdida de datos es posible porque hay una oportunidad para transmitir los cambios en la máquina virtual principal es posible que no replicados aún.  
  
Para obtener más información acerca de réplica de Hyper-V, consulta [información general de réplica de Hyper-V](https://technet.microsoft.com/library/jj134172.aspx) y [implementar réplica de Hyper-V](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> Réplica de Hyper-V se puede ejecutar solo en Windows Server Hyper-V, no a la versión de Hyper-V que se ejecuta en Windows 8.  
  
## <a name="windows-server-2012-domain-controllers-required"></a>Controladores de dominio de Windows Server 2012 necesarios  
Windows Server 2012 Hyper-V también presenta VM-GenerationID (VMGenID). VMGenID proporciona una forma de que el hipervisor para comunicarse con el SO invitado cuando se han producido cambios significativos. Por ejemplo, el hipervisor puede comunicar a un controlador de dominio virtualizada que una restauración de instantánea se ha producido (tecnología de restauración de Hyper-V instantánea, restaurar copia de seguridad no). AD DS en Windows Server 2012 es consciente de tecnología de VM VMGenID y lo usa para detectar cuándo se realizan operaciones de hipervisor, tales como la restauración de instantánea, lo cual te permite proteger mejor a sí mismo.  
  
> [!NOTE]  
> Para reforzar el punto, solo AD DS en controladores de dominio de Windows Server 2012 ofrece estas medidas de seguridad resultantes de VMGenID; Controladores de dominio que se ejecuten todas las versiones anteriores de Windows Server están sujetos a problemas como la reversión de USN que puede ocurrir cuando un controlador de dominio virtualizada se restaura mediante un mecanismo no compatible, como restaurar instantánea. Para obtener más información sobre estas medidas de seguridad y cuando se desencadenen, consulta [virtualizados arquitectura de controlador de dominio](https://technet.microsoft.com/library/jj574118.aspx).  
  
Cuando (planeada o no), se produce un error de réplica de Hyper-V, Windows Server 2012 virtualizados DC detecta un restablecimiento VMGenID, desencadenar las características de seguridad mencionados anteriormente. Operaciones de Active Directory, a continuación, continúe forma normal. La réplica VM que se ejecuta en lugar de la máquina virtual principal.  
  
> [!NOTE]  
> Dado que ahora ahora hay dos instancias de la misma identidad de controlador de dominio, es posible para la instancia principal y la instancia replicada para ejecutarse. Mientras réplica de Hyper-V tiene mecanismos de control para garantizar el principal y máquinas virtuales de réplica no se ejecutan al mismo tiempo, es posible que se ejecuten al mismo tiempo en el caso de que se produce un error en el vínculo entre ellas después de la replicación de la máquina virtual. En caso de que este improbable, virtualizadas controladores de dominio que ejecutan Windows Server 2012 tienen medidas de seguridad para ayudar a proteger los AD DS, mientras que virtualizan los controladores de dominio que ejecutan versiones anteriores de Windows Server no.  
  
Cuando uses réplica de Hyper-V, asegúrate de que sigas los procedimientos recomendados para [ejecutan controladores de dominio virtual en Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). Esto describe, por ejemplo, recomendaciones para almacenar archivos de Active Directory en discos SCSI virtuales, que proporciona más sólidas garantías de duración de datos.  
  
## <a name="supported-and-unsupported-scenarios"></a>Escenarios admitidos y no admitidos  
Solo máquinas virtuales que ejecutan Windows Server 2012 se admiten para la conmutación por error imprevisto y para las pruebas de conmutación por error. Incluso para conmutación por error planeada, Windows Server 2012 se recomienda para el controlador de dominio virtualizada para mitigar los riesgos en caso de que un administrador accidentalmente inicia la máquina virtual principal y la máquina virtual replicada al mismo tiempo.  
  
Máquinas virtuales que ejecutan versiones anteriores de Windows Server son compatibles con planeada conmutación por error, pero no compatible para la conmutación por error imprevisto debido a la posibilidad de reversión de USN. Para obtener más información sobre la reversión de USN, consulta [USN y reversión de USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).  
  
> [!NOTE]  
> Existen requisitos de nivel funcionales del dominio o bosque; Existen requisitos del sistema operativo solo para los controladores de dominio que se ejecutan como máquinas virtuales que se replican con Hyper-V réplica. En un bosque que contiene otros controladores de dominio físicas o virtuales que ejecutan versiones anteriores de Windows Server y puede o no también se replique con réplica de Hyper-V, se pueden implementar las máquinas virtuales.  
  
Esta declaración de soporte técnico se basa en pruebas realizadas en un bosque de dominio único, aunque también se admiten las configuraciones de bosque de varios dominios. Para estas pruebas, controladores de dominio virtualizada DC1 y DC2 son asociados de replicación de Active Directory en el mismo sitio, hospedados en un servidor que ejecuta Hyper-V en Windows Server 2012. El Invitado VM que se ejecuta DC2 tiene réplica de Hyper-V habilitado. El servidor de réplica está hospedado en otro datacenter distante. Para ayudar a explicar los procesos de caso de prueba que se describen a continuación, la máquina virtual que se ejecuta en el servidor de réplica se conoce como DC2 Rec (aunque en la práctica se reserva el mismo nombre que la máquina virtual original).  
  
### <a name="windows-server-2012"></a>Windows Server 2012  
La siguiente tabla explica la compatibilidad para virtualizada controladores de dominio que ejecutan Windows Server 2012 y casos de prueba.  
  
|||  
|-|-|  
|Planeada de conmutación por error|Conmutación por error imprevisto|  
|Admite|Admite|  
|Caso de prueba:<br /><br />-DC1 y DC2 ejecutan Windows Server 2012.<br /><br />-DC2 se apaga y se realiza una conmutación por error en DC2 Rec. Puede ser la conmutación por error planeada o no.<br /><br />-Cuando se inicie el DC2 Rec, comprueba si el valor de VMGenID que tiene en su base de datos es igual al valor desde el controlador de máquina virtual guardado en el servidor de réplica de Hyper-V.<br /><br />-Como resultado, DC2 Rec desencadena medidas de seguridad de virtualización; en otras palabras, restablece el Id. de invocación, descarta su grupo RID y establece un requisito de sincronización inicial antes de supone un rol de maestro de operaciones. Para obtener más información sobre el requisito de sincronización inicial, consulta.<br /><br />-DC2-Rec, a continuación, guarda el nuevo valor de VMGenID en su base de datos y confirma las actualizaciones subsiguientes en el contexto de la invocación nuevo.<br /><br />-Como resultado de este restablecimiento de invocación, DC1 se convergen en todos los cambios que AD introducidos DC2 Rec incluso si se ha deshecho en el tiempo, lo que significa que las actualizaciones de anuncios se realizan en DC2 Rec después de forma segura se converge la conmutación por error|El caso de prueba es el mismo que planeada conmutación por error, con las siguientes excepciones:<br /><br />-Un anuncio actualiza DC2 recibidos en, pero aún no replica por AD en un duplicador antes de que el evento de conmutación por error, se perderán.<br /><br />-Actualizaciones de AD recibidas en DC2 después de que el tiempo del punto de recuperación que se replicaron por AD DC1 DC1 se replicarán volver a DC2 Rec.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 y versiones anteriores  
La siguiente tabla explica la compatibilidad para virtualizada controladores de dominio que ejecutan Windows Server 2008 R2 y versiones anteriores.  
  
|||  
|-|-|  
|Planeada de conmutación por error|Conmutación por error imprevisto|  
|Compatible pero no se recomienda porque los controladores de dominio que se ejecutan estas versiones de Windows Server no admiten VMGenID o usar medidas de seguridad de virtualización asociado. Esto coloca corriendo el riesgo de reversión de USN. Para obtener más información, consulta [USN y reversión de USN](https://technet.microsoft.com/en-us/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|No admite **Nota:** la conmutación por error imprevisto admitiría donde la reversión de USN no es un riesgo, como un único controlador de dominio del bosque (es decir, una configuración que no se recomienda).|  
|Caso de prueba:<br /><br />-DC1 y DC2 ejecutan Windows Server 2008 R2.<br /><br />-DC2 se apaga y se realiza una conmutación por error planeada en DC2 Rec. Todos los datos de DC2 se replica en DC2 Rec antes de que el apagado.<br /><br />-Después DC2 Rec se inicia, se reanuda replicación con DC1 usando el mismo Id. de invocación como DC2.|N/D|  
  


