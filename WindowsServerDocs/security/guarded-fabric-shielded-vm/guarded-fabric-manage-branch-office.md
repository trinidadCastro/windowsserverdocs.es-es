---
title: Consideraciones sobre sucursales
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 5a07553e6662fd79230d566ba2049c5e8997f4d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403573"
---
# <a name="branch-office-considerations"></a>Consideraciones sobre la sucursal

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), 

En este artículo se describen los procedimientos recomendados para ejecutar máquinas virtuales blindadas en sucursales y otros escenarios remotos en los que los hosts de Hyper-V pueden tener períodos de tiempo con conectividad limitada a HGS.

## <a name="fallback-configuration"></a>Configuración de reserva

A partir de la versión 1709 de Windows Server, puede configurar un conjunto adicional de direcciones URL del servicio de protección de host en los hosts de Hyper-V para su uso cuando el HGS principal no responde.
Esto le permite ejecutar un clúster de HGS local que se usa como servidor principal para mejorar el rendimiento con la capacidad de revertir al HGS del centro de datos corporativo si los servidores locales están inactivos.

Para usar la opción de reserva, debe configurar dos servidores HGS. Pueden ejecutar Windows Server 2019 o Windows Server 2016 y formar parte de los mismos clústeres o de uno diferente. Si son clústeres diferentes, querrá establecer prácticas operativas para asegurarse de que las directivas de atestación estén sincronizadas entre los dos servidores. Ambos deben ser capaces de autorizar correctamente el host de Hyper-V para ejecutar máquinas virtuales blindadas y tener el material de clave necesario para iniciar las máquinas virtuales blindadas. Puede optar por tener un par de certificados de cifrado y firma compartidos entre los dos clústeres, o bien usar certificados independientes y configurar la máquina virtual blindada HGS para autorizar a ambos tutores (pares de certificados de cifrado y firma) en los datos de blindaje. filesystem.

Después, actualice los hosts de Hyper-V a Windows Server versión 1709 o Windows Server 2019 y ejecute el siguiente comando:
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Para quitar la configuración de un servidor de reserva, simplemente omita ambos parámetros de reserva:
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Para que el host de Hyper-V pase la atestación con los servidores principal y de reserva, deberá asegurarse de que la información de atestación esté actualizada con ambos clústeres HGS.
Además, los certificados que se usan para descifrar el TPM de la máquina virtual deben estar disponibles en ambos clústeres de HGS.
Puede configurar cada HGS con distintos certificados y configurar la máquina virtual para que confíe en ambos, o bien agregar un conjunto compartido de certificados a ambos clústeres de HGS.

Para obtener más información sobre la configuración de HGS en una sucursal con direcciones URL de reserva, consulte la entrada de blog sobre la [compatibilidad con sucursales mejorada para máquinas virtuales blindadas en Windows Server, versión 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Modo sin conexión

El modo sin conexión permite que la máquina virtual blindada se active cuando no se pueda alcanzar HGS, siempre y cuando la configuración de seguridad del host de Hyper-V no haya cambiado.
El modo sin conexión funciona mediante el almacenamiento en caché de una versión especial del protector de clave de TPM de VM en el host de Hyper-V.
El protector de clave se cifra a la configuración de seguridad actual del host (mediante la clave de identidad de seguridad basada en la virtualización).
Si el host no puede comunicarse con HGS y su configuración de seguridad no ha cambiado, podrá usar el protector de clave almacenada en caché para iniciar la máquina virtual blindada.
Cuando la configuración de seguridad cambia en el sistema, por ejemplo, cuando se aplica una nueva Directiva de integridad de código o se deshabilita el arranque seguro, se invalidarán los protectores de clave almacenados en caché y el host tendrá que atestar con un HGS antes de que las máquinas virtuales blindadas se puedan iniciar sin conexión de nuevo.

El modo sin conexión requiere Windows Server Insider Preview compilación 17609 o posterior para el clúster del servicio de protección de host y el host de Hyper-V.
Se controla mediante una directiva en HGS, que está deshabilitada de forma predeterminada.
Para habilitar la compatibilidad con el modo sin conexión, ejecute el siguiente comando en un nodo HGS:

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Dado que los protectores de clave que se pueden almacenar en caché son únicos para cada máquina virtual blindada, tendrá que apagar completamente (no reiniciar) e iniciar las máquinas virtuales blindadas para obtener un protector de clave en caché después de habilitar esta opción en HGS.
Si la máquina virtual blindada se migra a un host de Hyper-V que ejecuta una versión anterior de Windows Server, u obtiene un nuevo protector de clave de una versión anterior de HGS, no podrá iniciarse en el modo sin conexión, pero puede seguir ejecutándose en el modo en línea cuando el acceso a HGS esté disponible. permite.
