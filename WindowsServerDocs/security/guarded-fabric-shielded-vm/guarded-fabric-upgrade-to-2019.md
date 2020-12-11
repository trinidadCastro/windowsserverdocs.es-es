---
description: 'Más información sobre: actualización de un tejido protegido a Windows Server 2019'
title: Actualizar un tejido protegido a Windows Server 2019
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 11/21/2018
ms.openlocfilehash: bc7cedb2c232a61593dcce630e365b375c9d4744
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049103"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Actualizar un tejido protegido a Windows Server 2019

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este artículo se describen los pasos necesarios para actualizar un tejido protegido existente desde Windows Server 2016, Windows Server versión 1709 o Windows Server versión 1803 a Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Novedades de Windows Server 2019

Al ejecutar un tejido protegido en Windows Server 2019, puede aprovechar las ventajas de varias características nuevas:

La **atestación de clave de host** es nuestro modo de atestación más reciente, diseñado para que sea más fácil ejecutar máquinas virtuales blindadas cuando los hosts de Hyper-V no tienen dispositivos TPM 2,0 disponibles para la atestación de TPM. La atestación de clave de host usa pares de claves para autenticar hosts con HGS, lo que elimina la necesidad de que los hosts se unan a un dominio de Active Directory, lo que elimina la confianza de AD entre HGS y el bosque corporativo, y reduce el número de puertos de Firewall abiertos. La atestación de clave de host reemplaza Active Directory atestación, que está en desuso en Windows Server 2019.

**Versión de atestación de V2** : para admitir la atestación de clave de host y las nuevas características en el futuro, hemos incorporado el control de versiones a HGS. Una instalación nueva de HGS en Windows Server 2019 dará como resultado el servidor que usa la atestación V2, lo que significa que puede admitir la atestación de clave de host para hosts de Windows Server 2019 y seguir admitiendo hosts V1 en Windows Server 2016. Las actualizaciones en contexto a 2019 permanecerán en la versión v1 hasta que habilite manualmente V2. La mayoría de los cmdlets ahora tienen un parámetro-HgsVersion que le permite especificar si desea trabajar con directivas de atestación heredadas o modernas.

**Compatibilidad con máquinas virtuales blindadas Linux** : los hosts de Hyper-V que ejecutan Windows Server 2019 pueden ejecutar máquinas virtuales blindadas Linux. Mientras que las máquinas virtuales blindadas de Linux han estado en torno a la versión 1709 de Windows Server, Windows Server 2019 es la primera versión de canal de mantenimiento a largo plazo para admitirlos.

**Mejoras** en las sucursales: hemos facilitado la ejecución de máquinas virtuales blindadas en las sucursales con compatibilidad para máquinas virtuales blindadas sin conexión y configuraciones de reserva en hosts de Hyper-V.

**Enlace de host TPM** : para las cargas de trabajo más seguras, donde desea que una máquina virtual blindada se ejecute solo en el primer host en el que se creó, pero no en otra, ahora puede enlazar la máquina virtual a ese host mediante el TPM del host. Esto se utiliza mejor para las estaciones de trabajo de acceso con privilegios y las sucursales, en lugar de las cargas de trabajo generales que necesitan migrar entre hosts.

## <a name="compatibility-matrix"></a>Matriz de compatibilidad

Antes de actualizar el tejido protegido a Windows Server 2019, revise la siguiente matriz de compatibilidad para ver si se admite la configuración.

|  | HGS DE WS2016 | HGS DE WS2019|
|---|---|---|
|**Host de Hyper-V WS2016** | Compatible | Compatible<sup>1</sup>|
|**Host de Hyper-V WS2019** | No compatible<sup>2</sup> | Compatible|

<sup>1</sup> los hosts de windows Server 2016 solo pueden atestiguar en servidores de HGS de windows Server 2019 mediante el protocolo de atestación v1. Las nuevas características que están disponibles exclusivamente en el protocolo de atestación V2, incluida la atestación de clave de host, no se admiten para los hosts de Windows Server 2016.

<sup>2</sup> Microsoft es consciente de un problema que impide que los hosts de windows Server 2019 con la atestación de TPM se comparan correctamente en un servidor de HGS de windows Server 2016. Esta limitación se solucionará en una futura actualización para Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>Actualizar HGS a Windows Server 2019

Se recomienda actualizar el clúster de HGS a Windows Server 2019 antes de actualizar los hosts de Hyper-V para asegurarse de que todos los hosts, ya estén ejecutando Windows Server 2016 o 2019, pueden seguir atestando correctamente.

Para actualizar el clúster de HGS, es necesario quitar temporalmente un nodo del clúster a la vez que se actualiza. Esto reducirá la capacidad del clúster para responder a las solicitudes de los hosts de Hyper-V y podría dar lugar a tiempos de respuesta lentos o interrupciones del servicio para los inquilinos. Asegúrese de que tiene suficiente capacidad para administrar la atestación y las solicitudes de liberación de claves antes de actualizar un servidor de HGS.

Para actualizar el clúster de HGS, realice los pasos siguientes en cada nodo del clúster, un nodo a la vez:

1.  Quite el servidor HGS del clúster mediante la ejecución `Clear-HgsServer` de en un símbolo del sistema de PowerShell con privilegios elevados. Este cmdlet quitará el almacén replicado de HGS, los sitios web de HGS y el nodo del clúster de conmutación por error.
2.  Si el servidor HGS es un controlador de dominio (configuración predeterminada), tendrá que ejecutar `adprep /forestprep` y `adprep /domainprep` en el primer nodo que se va a actualizar para preparar el dominio para una actualización del sistema operativo. Consulte la [documentación de actualización de Active Directory Domain Services](../../identity/ad-ds/deploy/upgrade-domain-controllers.md#supported-in-place-upgrade-paths) para obtener más información.
3.  Realice una [actualización en contexto](../../get-started-19/install-upgrade-migrate-19.md) a Windows Server 2019.
4.  Ejecute [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) para volver a unir el nodo al clúster.

Una vez que todos los nodos se han actualizado a Windows Server 2019, opcionalmente puede actualizar la versión de HGS a v2 para admitir nuevas características, como la atestación de clave de host.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Actualizar hosts de Hyper-V a Windows Server 2019

Antes de actualizar los hosts de Hyper-V a Windows Server 2019, asegúrese de que el clúster de HGS ya esté actualizado a Windows Server 2019 y de que haya descargado todas las máquinas virtuales del servidor de Hyper-V.

1.  Si usa directivas de integridad de código de control de aplicaciones de Windows Defender en el servidor (siempre que se use la atestación de TPM), asegúrese de que la Directiva está en modo auditoría o deshabilitada antes de intentar actualizar el servidor. [Más información sobre cómo deshabilitar una directiva de WDAC](/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Siga las instrucciones del [contenido de actualización de Windows Server](../../upgrade/upgrade-overview.md) para actualizar el host a windows Server 2019. Si el host de Hyper-V forma parte de un clúster de conmutación por error, considere la posibilidad de usar una [actualización gradual del sistema operativo del clúster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Pruebe y vuelva a habilitar](/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) la Directiva de Windows Defender Application control, si tiene una habilitada antes de la actualización.
4.  Ejecute `Get-HgsClientConfiguration` para comprobar si **IsHostGuarded = true**, lo que significa que el host está pasando correctamente la atestación con el servidor HGS.
5.  Si usa la atestación de TPM, es posible que tenga que [volver a capturar la Directiva de línea de base de TPM o de integridad de código](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) después de la actualización para pasar la atestación.
6.  Vuelva a iniciar la ejecución de máquinas virtuales blindadas en el host.

## <a name="switch-to-host-key-attestation"></a>Cambiar a atestación de clave de host

Siga los pasos que se indican a continuación si está ejecutando la atestación basada en Active Directory y desea actualizar a la atestación de clave de host. Tenga en cuenta que la atestación basada en Active Directory está en desuso en Windows Server 2019 y puede quitarse en una versión futura.

1.  Ejecute el siguiente comando para asegurarse de que el servidor HGS funciona en el modo de atestación V2. Los hosts v1 existentes seguirán atestando incluso cuando el servidor HGS se actualice a la versión 2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Genere claves de host](guarded-fabric-create-host-key.md) a partir de cada uno de los hosts de Hyper-V y REGÍSTRELA con HGS. Dado que HGS sigue funcionando en modo de Active Directory, recibirá una advertencia que indica que las nuevas claves de host no son efectivas de inmediato. Esto es intencionado, ya que no desea cambiar al modo de clave de host hasta que todos los hosts puedan atestar correctamente las claves de host.

3.  Una vez que las claves de host se han registrado para cada host, puede configurar HGS para usar el modo de atestación de clave de host:

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Si experimenta problemas con el modo de clave de host y necesita revertir a la atestación basada en Active Directory, ejecute el siguiente comando en HGS:

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```
