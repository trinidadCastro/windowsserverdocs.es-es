---
title: Guía de planeación de los proveedores de hospedaje VM blindada y tejido protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 320723f7a0a25784180b232ce05d42c2ced933c8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284186"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Guía de planeación de los proveedores de hospedaje VM blindada y tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se trata las decisiones de planificación que deben realizarse en Habilite blindada las máquinas virtuales para ejecutar en el tejido. Si actualiza un tejido de Hyper-V existente o crear un tejido de nuevo, ejecute blindadas máquinas virtuales consta de dos componentes principales:

- El servicio de protección de Host (HGS) proporciona atestación y protección de claves de modo que asegúrese de que las máquinas virtuales blindadas se ejecutarán solo en hosts de Hyper-V aprobados y en buen Estados. 
- Aprobado y en buen Estados de los hosts de Hyper-V en el que pueden ejecutar las máquinas virtuales blindadas (y las máquinas virtuales normales), estos se conocen como hosts protegidos.

![HGS y un host protegido](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>Decisión #1: Nivel en el tejido de confianza

Cómo implementar el servicio guardián de Host y los hosts de Hyper-V protegidos dependerán principalmente fortaleza de la relación de confianza que desea para conseguir en el tejido. El nivel de confianza se rige por el modo de atestación. Hay dos opciones mutuamente excluyentes:

1. Atestación de TPM de confianza

    Si su objetivo es ayudar a proteger las máquinas virtuales de un tejido comprometido o de administradores malintencionados, a continuación, utilizará atestación de TPM de confianza. Esta opción funciona bien para varios inquilinos escenarios de hospedaje, así como para los activos de gran valor en entornos empresariales, como controladores de dominio o servidores de contenido, como SQL o SharePoint.
    Directivas de integridad (HVCI) de código protegido por el hipervisor se miden y su validez exigido HGS antes de que el host tiene permiso para ejecutar máquinas virtuales blindadas. 

2. Atestación de clave de host

    Si sus requisitos se basa principalmente en cumplimiento que requiere virtuales machines cifrarse tanto en reposo, así como sobre la marcha, a continuación, utilizará atestación de clave de host. Esta opción funciona bien para los centros de datos de uso general que está familiarizado con los administradores de host y el tejido de Hyper-V tener acceso a los sistemas operativos invitados de máquinas virtuales para las operaciones y mantenimiento diario. 

    En este modo, el administrador del tejido es el único responsable de garantizar el estado de los hosts de Hyper-V. 
    Puesto que HGS no representa ningún papel en decidir lo que es o no se puede ejecutar, malware y depuradores funcionarán según lo previsto. 
    
    Sin embargo, se bloquean los depuradores que intentan conectarse directamente a un proceso (por ejemplo, WinDbg.exe) para máquinas virtuales blindadas porque el proceso de trabajo de la máquina virtual (VMWP.exe) es una luz de proceso protegido (PPL). 
    Técnicas de depuración de alternativas, como las utilizadas por LiveKd.exe, no se bloquean. 
    A diferencia de las máquinas virtuales blindadas, el proceso de trabajo para máquinas virtuales admitido el cifrado no se ejecuta como un PPL los depuradores tradicionales como WinDbg.exe seguirá funcionando con normalidad.

    Un modo de atestación similar denominada atestación de administrador de confianza (basada en Active Directory) está en desuso a partir de Windows Server 2019. 

El nivel de confianza que elija determinará los requisitos de hardware para los hosts de Hyper-V, así como las directivas que se apliquen en el tejido. Si es necesario, puede implementar a su tejido protegido mediante hardware existente y la atestación de administrador de confianza y, a continuación, convertirlo a la atestación de TPM de confianza cuando se ha actualizado el hardware y necesita reforzar la seguridad de fabric.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>Decisión #2: Tejido de Hyper-V existente frente a un tejido de Hyper-V independiente nuevo

Si tiene una estructura existente (Hyper-V o no), es muy probable que puede usarlo para ejecutar máquinas virtuales blindadas junto con las máquinas virtuales normales. Algunos clientes eligen integrar las máquinas virtuales blindadas en sus tejidos y herramientas existentes mientras que otros usuarios separan al tejido por motivos empresariales.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Administrador de HGS planeación para el servicio de protección de Host

Implementar el servicio de protección de Host (HGS) en un entorno altamente seguro, ya sea en un servidor físico dedicado, una máquina virtual blindada, una máquina virtual en un host de Hyper-V aislado (separados del tejido que se protege de), o uno separados lógicamente con una Azure diferente suscripción.   

| Área | Detalles |
|------|---------|
| Requisitos de instalación | <ul><li>Un servidor (recomendado para la alta disponibilidad de clúster de tres nodos)</li><li>Para la reserva, se requieren al menos dos servidores HGS</li><li>Los servidores pueden ser virtuales o físicos (servidores físicos con TPM 2.0 recomendado; TPM 1.2 también admitidos)</li><li>Instalación de Server Core de Windows Server 2016 o posterior</li><li>Línea de visión de red al permitir HTTP tejido o [configuración de reserva](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Certificado HTTPS recomendado para la validación de acceso</li></ul> |
| Ajuste de tamaño | Cada nodo de servidor HGS de tamaño mediano (8 núcleos y 4 GB) puede administrar 1.000 hosts de Hyper-V. |
| Management | Designar a usuarios específicos que administrarán el HGS. Deben ser distintos de los administradores de fabric. Para la comparación, los clústeres de HGS pueden considerarse de la misma manera como una entidad de certificación (CA) en cuanto a aislamiento administrativa, implementaciones físicas y nivel de sensibilidad de seguridad global. |
| Servicio de protección de host Active Directory | De forma predeterminada, HGS instala su propio Active Directory interno para la administración. Esto es un bosque independiente, autoadministrado y es la configuración recomendada para ayudar a aislar el HGS de su tejido.<br><br>Si ya tiene un bosque de Active Directory con privilegios elevados que usa para el aislamiento, puede usar ese bosque en lugar del bosque HGS de forma predeterminada. Es importante que HGS no está unido a un dominio en el mismo bosque que los hosts de Hyper-V o las herramientas de administración de tejido. Si lo hace, podría permitir que un administrador de tejido a tener control sobre el HGS. |
| Recuperación ante desastres | Existen tres opciones:<br><ol><li>Instalar un clúster independiente de HGS en cada centro de datos y autorizar a las máquinas virtuales blindadas para ejecutarse en el servidor principal y los centros de datos de copia de seguridad. Esto evita la necesidad de expandir el clúster a través de una WAN y le permite aislar las máquinas virtuales que se ejecutan solo en su sitio designado.</li><li>Instale HGS en un clúster extendido entre dos (o más) centros de datos. Esto proporciona resistencia si la red WAN deja de funcionar, pero inserta los límites de agrupación en clústeres de conmutación por error. No se puede aislar las cargas de trabajo a un sitio; una máquina virtual tiene autorizada para ejecutar en un sitio puede ejecutar en cualquier otro.</li><li>Registrar el host de Hyper-V con HGS otro como conmutación por error.</li></ol>También debe hacer copia de seguridad de cada HGS mediante la exportación de su configuración de modo que siempre se puede recuperar localmente. Para obtener más información, consulte [exportación HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/export-hgsserverstate) y [importación HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/import-hgsserverstate). |
| Claves del servicio de protección de host | Un servicio de protección de Host usa dos pares de claves asimétricos, una clave de cifrado y una clave de firma, cada uno representado por un certificado SSL. Hay dos opciones para generar estas claves:<br><ol><li>Entidad de certificación interna, puede generar estas claves mediante la infraestructura de PKI interna. Esto es adecuado para un entorno de centro de datos.</li><li>Certificación de confianza pública: usar un conjunto de claves obtenido de una entidad de certificación de confianza pública. Esta es la opción que deben usar los proveedores de hospedaje.</li></ol>Tenga en cuenta que, aunque es posible usar certificados autofirmados, no se recomienda para escenarios de implementación que no sea de laboratorios de prueba de concepto.<br><br>Además de tener las claves HGS, un proveedor de hospedaje puede usar "traiga su propia clave," donde los inquilinos pueden proporcionar sus propias claves para que algunos (o todos) los inquilinos pueden tener su propia clave específica de HGS. Esta opción es adecuada para los proveedores de hospedaje que pueden proporcionar un proceso fuera de banda para los inquilinos cargar sus claves. |
| Almacenamiento de claves de servicio de protección de host | Para la máxima seguridad posible, se recomienda que las claves HGS se crean y almacenan exclusivamente en un módulo de seguridad de Hardware (HSM). Si no usa HSM, aplicar BitLocker en los servidores HGS se recomienda encarecidamente. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Planeación de administración de fabric para hosts protegidos

| Área | Detalles |
|------|---------|
| Hardware | <ul><li>Atestación de clave de host: Puede usar cualquier hardware existente como el host protegido. Hay algunas excepciones (para asegurarse de que el host puede usar los nuevos mecanismos de seguridad a partir de Windows Server 2016, vea [hardware Compatible con la protección basada en virtualización de Windows Server 2016 de integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Atestación de TPM de confianza: Puede usar cualquier hardware que tiene el [calificación adicional de seguridad de Hardware](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) siempre y cuando está configurado correctamente (consulte [configuraciones de servidor que son compatibles con las máquinas virtuales blindadas y basada en virtualización protección de integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) para la configuración específica). Esto incluye el TPM 2.0 y UEFI versión 2.3. 1C y versiones posteriores.</li></ul> |
| Sistema operativo | Se recomienda usar la opción Server Core para el host de Hyper-V del sistema operativo. |
| Implicaciones de rendimiento | En función de las pruebas de rendimiento, prevemos un aproximadamente densidad de 5% de diferencia entre ejecutar blindadas las máquinas virtuales y las máquinas virtuales no blindadas. Esto significa que si un determinado host de Hyper-V puede ejecutar 20 máquinas virtuales no blindadas, esperamos que pueden ejecutar máquinas virtuales blindadas 19.<br><br>Asegúrese de comprobar el ajuste de tamaño con las cargas de trabajo típicas. Por ejemplo, puede haber algunos valores atípicos con orientada a servicios de escritura E/S cargas de trabajo intensivas que más afectan a la diferencia de densidad. |
| Consideraciones sobre la sucursal | A partir de Windows Server versión 1709, puede especificar una dirección URL de reserva para un servidor HGS virtualizado que se ejecuta localmente como una máquina virtual blindada de la sucursal. La dirección URL de reserva se puede usar cuando la sucursal pierde conectividad con servidores HGS en el centro de datos. En versiones anteriores de Windows Server, un host de Hyper-V que se ejecuta en una conectividad de necesidades de las sucursales para el servicio de protección de Host a encendido o migrar en vivo de máquinas virtuales blindadas. Para obtener más información, consulte [rama consideraciones sobre office](guarded-fabric-manage-branch-office.md). |
