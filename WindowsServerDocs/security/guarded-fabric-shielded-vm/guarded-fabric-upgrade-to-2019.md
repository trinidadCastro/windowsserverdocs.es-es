---
title: Actualizar un tejido protegido a Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 39974806c02e55b37d3d16748c4ca0e3f361ee45
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284106"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Actualizar un tejido protegido a Windows Server 2019

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este artículo se describe los pasos necesarios para actualizar un tejido protegido existente de Windows Server 2016 versión 1709 de Windows Server o Windows Server versión 1803 para Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Novedades de Windows Server 2019

Al ejecutar un tejido protegido en Windows Server 2019, puede aprovechar varias características nuevas:

**Atestación de la clave de host** es nuestro modo de atestación más reciente, diseñado para que sea más fácil de ejecutar máquinas virtuales blindadas cuando los hosts de Hyper-V no tienen TPM 2.0 dispositivos disponibles para la atestación de TPM. Atestación de clave de host utiliza pares de claves para autenticar hosts con HGS, eliminando el requisito para los hosts se unan a un dominio de Active Directory, lo que elimina la confianza de AD entre HGS y el bosque corporativo y reducir el número de puertos de firewall abierto. Atestación de clave de host reemplaza la atestación de Active Directory, que está en desuso en Windows Server 2019.

**Versión de atestación v2** - para admitir la atestación de clave de Host y las nuevas características en el futuro, hemos presentado las versiones de HGS. En el servidor con la atestación de v2, lo que significa que se puede admitir la atestación de clave de Host para hosts de Windows Server 2019 y todavía se admite hosts v1 en Windows Server 2016, se producirá una instalación nueva de HGS en Windows Server 2019. Actualizaciones en contexto para 2019 permanecerá en la versión v1 hasta que habilite manualmente v2. Mayoría de los cmdlets tiene ahora un parámetro de - HgsVersion que le permite especificar si desea trabajar con directivas de atestación modernas o heredados.

**Compatibilidad con Linux blindadas** -hosts de Hyper-V que ejecutan Windows Server 2019 puede ejecutar máquinas virtuales blindadas Linux. Aunque Linux blindada máquinas virtuales están disponibles desde Windows Server versión 1709, Windows Server 2019 es la primera versión de canal de servicio de largo plazo para admitirlos.

**Mejoras de office de rama** -nos hemos puesto fácil para ejecutar máquinas virtuales blindadas en sucursales con soporte técnico para máquinas virtuales blindadas sin conexión y las configuraciones de reserva en hosts de Hyper-V.

**Enlace a host TPM** -para la mayoría proteger las cargas de trabajo, donde desea que una máquina virtual blindada para ejecutarse solo el primer host donde se ha creado, pero ninguna otra, ahora puede enlazar la máquina virtual a ese host mediante el TPM del host. Esto se utiliza mejor para estaciones de trabajo con privilegios de acceso y las sucursales, en lugar de las cargas de trabajo general del centro de datos que necesitan para migrar entre hosts.

## <a name="compatibility-matrix"></a>Matriz de compatibilidad

Antes de actualizar a Windows Server 2019 su tejido protegido, revise la siguiente matriz de compatibilidad para ver si se admite la configuración.

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hyper-V Host** | Se admite | Admite<sup>1</sup>|
|**WS2019 Hyper-V Host** | No compatible<sup>2</sup> | Se admite|

<sup>1</sup> hosts de Windows Server 2016 solo pueden atestiguar contra los servidores de Windows Server 2019 HGS mediante el protocolo de atestación v1. Nuevas características que están disponibles exclusivamente en el protocolo de atestación de v2, incluida la atestación de clave de Host, no se admiten para los hosts de Windows Server 2016.

<sup>2</sup> Microsoft tiene constancia de un problema que impedía a hosts de Windows Server 2019 con la atestación de TPM de avalar correctamente en un servidor de Windows Server 2016 HGS. Esta limitación se corregirá en una futura actualización para Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>HGS actualización a Windows Server de 2019

Se recomienda actualizar el clúster HGS para Windows Server 2019 antes de actualizar los hosts de Hyper-V para asegurarse de que todos los hosts, si se están ejecutando Windows Server 2016 o 2019, pueden continuar dar fe correctamente.

Actualización del clúster HGS requerirá temporalmente quitar un nodo del clúster en un momento mientras se actualiza. Esto reducirá la capacidad del clúster de responder a las solicitudes de los hosts de Hyper-V y podría dar lugar a tiempos de respuesta lentos o interrupciones del servicio para los inquilinos. Asegúrese de que tiene suficiente capacidad para controlar la atestación y solicitudes de publicación clave antes de actualizar un servidor HGS.

Para actualizar el clúster HGS, realice los pasos siguientes en cada nodo del clúster, un nodo a la vez:

1.  Eliminación de un servidor HGS del clúster mediante la ejecución de `Clear-HgsServer` en un símbolo del sistema con privilegios elevados de PowerShell. Este cmdlet quitará el almacén replicado de HGS, sitios Web HGS y nodo del clúster de conmutación por error.
2.  Si el servidor HGS es un controlador de dominio (configuración predeterminada), deberá ejecutar `adprep /forestprep` y `adprep /domainprep` en el primer nodo que se está actualizando para preparar el dominio para actualizar el sistema operativo. Consulte la [documentación de actualización de servicios de dominio de Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) para obtener más información.
3.  Realizar una [actualización in situ](../../get-started-19/install-upgrade-migrate-19.md) a Windows Server 2019.
4.  Ejecute [Initialize HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) para unir el nodo nuevo al clúster.

Una vez que todos los nodos se han actualizado a Windows Server 2019, opcionalmente, puede actualizar la versión HGS a v2 para admitir nuevas características, como la atestación de clave de Host.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Actualizar los hosts de Hyper-V para Windows Server 2019

Antes de actualizar los hosts de Hyper-V para Windows Server 2019, asegúrese de que el clúster HGS ya está actualizado a Windows Server 2019 y que se han movido todas las máquinas virtuales fuera del servidor de Hyper-V.

1.  Si usa directivas de integridad de código de Windows Defender Application Control en el servidor (siempre es el caso cuando se usa la atestación de TPM), asegúrese de que la directiva está en modo auditoría o deshabilitado antes de intentar actualizar el servidor. [Obtenga información sobre cómo deshabilitar una directiva WDAC](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Siga las instrucciones de la [centro de actualización de Windows Server](http://aka.ms/upgradecenter) para actualizar el host de Windows Server 2019. Si el host de Hyper-V forma parte de un clúster de conmutación por error, considere el uso de un [actualización gradual de clúster del sistema operativo](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Probar y volver a habilitar](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) la directiva de Windows Defender Application Control, si hubiera una habilitado antes de la actualización.
4.  Ejecute `Get-HgsClientConfiguration` para comprobar si **IsHostGuarded = True**, lo que significa que el host está pasando correctamente la atestación con el servidor HGS.
5.  Si usa la atestación de TPM, es posible que deba [volver a capturar la directiva de integridad de código o de línea de base TPM](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) después de la actualización para pasar la atestación.
6.  ¡Ejecución de máquinas virtuales blindadas en el host nuevo!

## <a name="switch-to-host-key-attestation"></a>Conmutador para hospedar la atestación de clave

Siga los pasos siguientes si se están ejecutando actualmente atestación basada en Active Directory y desea actualizar a la atestación de clave de Host. Tenga en cuenta que la atestación basada en Active Directory está en desuso en Windows Server 2019 y puede quitarse en una versión futura.

1.  Asegúrese de que el servidor HGS está funcionando en modo de atestación de v2, ejecute el comando siguiente. Hosts v1 existentes seguirán FE incluso cuando el servidor HGS se actualiza a v2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Generar claves de host](guarded-fabric-create-host-key.md) desde cada uno de su Hyper-V hospeda y registrarlas con HGS. Porque HGS siguen funcionando en modo Active Directory, recibirá una advertencia de que las nuevas claves de host no son efectivas inmediatamente. Esto es intencionado, ya que no desea cambiar a modo de clave de host hasta que todos los hosts pueden atestiguar correctamente con las claves de host.

3.  Una vez que se han registrado las claves de host para cada host, puede configurar HGS para usar el modo de atestación de clave de host:

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Si experimenta problemas con el modo de clave de host y necesita revertir a la atestación basada en Active Directory, ejecute el siguiente comando en el HGS:

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```