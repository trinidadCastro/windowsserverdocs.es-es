---
description: 'Más información sobre: tejido protegido y guía de planificación de máquinas virtuales blindadas para proveedores de hospedaje'
title: Guía de planificación de máquinas virtuales blindadas y tejido protegido para proveedores de hospedaje
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.author: nirb
ms.date: 08/29/2018
ms.openlocfilehash: 58d29de19959bad617c9db3e07c12b316624056c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043953"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Guía de planificación de máquinas virtuales blindadas y tejido protegido para proveedores de hospedaje

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se tratan las decisiones de planeación que deben realizarse para permitir que las máquinas virtuales blindadas se ejecuten en el tejido. Tanto si actualiza un tejido de Hyper-V existente como si crea un nuevo tejido, la ejecución de máquinas virtuales blindadas consta de dos componentes principales:

- El servicio de protección de host (HGS) proporciona atestación y protección de claves para que pueda asegurarse de que las máquinas virtuales blindadas se ejecutarán solo en hosts de Hyper-V aprobados y en buen estado. 
- Hosts de Hyper-V aprobados y en buen estado en los que se pueden ejecutar máquinas virtuales blindadas (y máquinas virtuales normales): estos se conocen como hosts protegidos.

![HGS y un host protegido](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>#1 de decisión: nivel de confianza en el tejido

La forma de implementar el servicio de protección de host y los hosts de Hyper-V protegidos dependerá principalmente del nivel de confianza que quiera lograr en el tejido. El nivel de confianza se rige por el modo de atestación. Hay dos opciones mutuamente excluyentes:

1. Atestación de confianza de TPM

    Si el objetivo es ayudar a proteger las máquinas virtuales de administradores malintencionados o de un tejido en peligro, usará la atestación de confianza de TPM. Esta opción funciona bien para escenarios de hospedaje de varios inquilinos, así como para activos de gran valor en entornos empresariales, como controladores de dominio o servidores de contenido como SQL o SharePoint.
    Las directivas de integridad de código protegido por hipervisor (HVCI) se miden y su validez se aplica mediante HGS antes de que el host tenga permiso para ejecutar máquinas virtuales blindadas.

2. Atestación de clave de host

    Si sus requisitos se han controlado principalmente por el cumplimiento que requiere que las máquinas virtuales se cifren tanto en reposo como en vuelo, usará la atestación de clave de host. Esta opción funciona bien para los centros de trabajo de uso general en los que se siente cómodo con los administradores de host y tejido de Hyper-V que tienen acceso a los sistemas operativos invitados de las máquinas virtuales para realizar tareas de mantenimiento y operaciones cotidianas.

    En este modo, el administrador del tejido es el único responsable de garantizar el estado de los hosts de Hyper-V.
    Dado que HGS no juega nada en la decisión de lo que se puede ejecutar, el malware y los depuradores funcionarán según lo previsto.

    Sin embargo, los depuradores que intentan conectarse directamente a un proceso (como WinDbg.exe) están bloqueados para las máquinas virtuales blindadas porque el proceso de trabajo de la máquina virtual (VMWP.exe) es una luz de proceso protegido (PPL).
    Las técnicas de depuración alternativas, como las utilizadas por LiveKd.exe, no se bloquean.
    A diferencia de las máquinas virtuales blindadas, el proceso de trabajo para las máquinas virtuales compatibles con el cifrado no se ejecuta como PPL, por lo que los depuradores tradicionales como WinDbg.exe seguirán funcionando con normalidad.

    Un modo de atestación similar denominado atestación de confianza de administrador (basado en Active Directory) está en desuso a partir de Windows Server 2019.

El nivel de confianza que elija determinará los requisitos de hardware para los hosts de Hyper-V, así como las directivas que se aplican en el tejido. Si es necesario, puede implementar el tejido protegido mediante el hardware existente y la atestación de confianza de administrador y, después, convertirlo en atestación de confianza de TPM cuando se haya actualizado el hardware y necesite reforzar la seguridad del tejido.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>#2 de decisión: tejido de Hyper-V existente frente a un nuevo tejido de Hyper-V independiente

Si tiene un tejido existente (Hyper-V o de otro tipo), es muy probable que se pueda usar para ejecutar máquinas virtuales blindadas junto con máquinas virtuales normales. Algunos clientes optan por integrar máquinas virtuales blindadas en sus tejidos y herramientas existentes, mientras que otras separan el tejido por motivos empresariales.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Planeación de administración de HGS para el servicio de protección de host

Implemente el servicio de protección de host (HGS) en un entorno de alta seguridad, ya sea en un servidor físico dedicado, en una máquina virtual blindada, en una máquina virtual en un host de Hyper-V aislado (separado del tejido que protege) o en una separada lógicamente con una suscripción de Azure diferente.

| Área | Detalles |
|------|---------|
| Requisitos de instalación | <ul><li>Un servidor (clúster de tres nodos recomendado para alta disponibilidad)</li><li>Para la reserva, se necesitan al menos dos servidores HGS</li><li>Los servidores pueden ser virtuales o físicos (se recomienda un servidor físico con el TPM 2,0). TPM 1,2 también compatible)</li><li>Instalación Server Core de Windows Server 2016 o posterior</li><li>Línea de vista de red al tejido que permite la [configuración](guarded-fabric-manage-branch-office.md#fallback-configuration) de http o de reserva</li><li>Certificado HTTPS recomendado para la validación de acceso</li></ul> |
| Ajuste de tamaño | Cada nodo de servidor HGS de tamaño medio (8 núcleos/4 GB) puede administrar hosts de Hyper-V 1.000. |
| Administración | Designe usuarios específicos que administrarán HGS. Deben ser independientes de los administradores del tejido. Para la comparación, los clústeres de HGS se pueden considerar de la misma manera que una entidad de certificación (CA) en lo que respecta al aislamiento administrativo, la implementación física y el nivel total de confidencialidad de seguridad. |
| Active Directory del servicio de protección de host | De forma predeterminada, HGS instala su propia Active Directory interna para la administración. Se trata de un bosque autoadministrado independiente y es la configuración recomendada para ayudar a aislar HGS del tejido.<br><br>Si ya tiene un bosque de Active Directory con privilegios elevados que se usa para el aislamiento, puede usar ese bosque en lugar del bosque predeterminado de HGS. Es importante que HGS no esté unido a un dominio del mismo bosque que los hosts de Hyper-V o las herramientas de administración del tejido. Si lo hace, puede permitir que un administrador de tejido adquiera control sobre HGS. |
| Recuperación ante desastres | Hay tres opciones:<br><ol><li>Instale un clúster de HGS independiente en cada centro de recursos y autorice que las máquinas virtuales blindadas se ejecuten en los centros de seguridad principal y de reserva. Esto evita la necesidad de expandir el clúster a través de una WAN y permite aislar las máquinas virtuales de modo que se ejecuten solo en el sitio designado.</li><li>Instale HGS en un clúster extendido entre dos (o más) centros de recursos. Esto proporciona resistencia si la WAN deja de funcionar, pero empuja los límites de los clústeres de conmutación por error. No puede aislar las cargas de trabajo en un sitio; una máquina virtual autorizada para ejecutarse en un sitio puede ejecutarse en otro.</li><li>Registre el host de Hyper-V con otro HGS como conmutación por error.</li></ol>También debe hacer una copia de seguridad de cada HGS mediante la exportación de su configuración para que siempre se pueda recuperar localmente. Para obtener más información, vea [Export-HgsServerState](/powershell/module/hgsserver/export-hgsserverstate) e [Import-HgsServerState](/powershell/module/hgsserver/import-hgsserverstate). |
| Claves de servicio de protección de host | Un servicio de protección de host usa dos pares de claves asimétricas (una clave de cifrado y una clave de firma), cada uno representado por un certificado SSL. Existen dos opciones para generar estas claves:<br><ol><li>Entidad de certificación interna: puede generar estas claves mediante la infraestructura de PKI interna. Esto es adecuado para un entorno de centro de recursos.</li><li>Entidades de certificación de confianza pública: usan un conjunto de claves obtenidas de una entidad de certificación de confianza pública. Esta es la opción que los anfitriones deben usar.</li></ol>Tenga en cuenta que, aunque es posible usar certificados autofirmados, no se recomienda para escenarios de implementación que no sean laboratorios de prueba de concepto.<br><br>Además de tener claves HGS, un anfitrión puede usar "traiga su propia clave", donde los inquilinos pueden proporcionar sus propias claves para que algunos (o todos) los inquilinos puedan tener su propia clave de HGS específica. Esta opción es adecuada para los proveedores de hospedaje que pueden proporcionar un proceso fuera de banda para que los inquilinos carguen sus claves. |
| Almacenamiento de claves del servicio de protección de host | Para obtener la máxima seguridad posible, se recomienda crear las claves HGS y almacenarlas exclusivamente en un módulo de seguridad de hardware (HSM). Si no usa HSM, se recomienda encarecidamente aplicar BitLocker en los servidores de HGS. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Planeación de administración de tejido para hosts protegidos

| Área | Detalles |
|------|---------|
| Hardware | <ul><li>Atestación de clave de host: puede usar cualquier hardware existente como host protegido. Hay algunas excepciones (para asegurarse de que el host puede usar nuevos mecanismos de seguridad a partir de Windows Server 2016, consulte [hardware compatible con la protección basada en la virtualización de Windows server 2016 de integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Atestación de confianza de TPM: puede usar cualquier hardware que tenga la [calificación adicional de hardware Assurance](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) siempre que se configure de forma adecuada (vea [configuraciones de servidor que son compatibles con máquinas virtuales blindadas y protección basada en virtualización de la integridad de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) para la configuración específica). Esto incluye TPM 2,0 y UEFI versión 2.3.1 c y versiones posteriores.</li></ul> |
| SO | Se recomienda usar la opción Server Core para el sistema operativo del host de Hyper-V. |
| Implicaciones de rendimiento | En función de las pruebas de rendimiento, se prevé una diferencia de densidad del 5% aproximadamente entre la ejecución de máquinas virtuales blindadas y máquinas virtuales no blindadas. Esto significa que si un host de Hyper-V determinado puede ejecutar 20 máquinas virtuales no blindadas, esperamos que pueda ejecutar 19 máquinas virtuales blindadas.<br><br>Asegúrese de comprobar el tamaño con las cargas de trabajo típicas. Por ejemplo, puede haber algunos valores atípicos con cargas de trabajo intensivas de e/s orientadas a escritura que afectarán aún más a la diferencia de densidad. |
| Consideraciones sobre la sucursal | A partir de la versión 1709 de Windows Server, puede especificar una dirección URL de reserva para un servidor de HGS virtualizado que se ejecute localmente como una máquina virtual blindada en la sucursal. La dirección URL de reserva se puede usar cuando la sucursal pierde la conectividad con los servidores HGS del centro de recursos. En las versiones anteriores de Windows Server, un host de Hyper-V que se ejecuta en una sucursal necesita conectividad con el servicio de protección de host para el encendido o la migración en vivo de máquinas virtuales blindadas. Para obtener más información, consulte [consideraciones de sucursales](guarded-fabric-manage-branch-office.md). |
