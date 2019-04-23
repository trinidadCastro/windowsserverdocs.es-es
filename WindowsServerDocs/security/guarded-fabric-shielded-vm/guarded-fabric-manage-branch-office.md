---
title: Consideraciones de Branch Office
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: d93c37227af1eb62368fbcd4ec5d6a48374b45ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877066"
---
# <a name="branch-office-considerations"></a>Consideraciones sobre la sucursal

> Se aplica a: Windows Server, Windows Server (canal semianual), de 2019 

En este artículo se describe prácticas recomendadas para ejecutar máquinas virtuales blindadas en las sucursales y otros escenarios remotos en los hosts de Hyper-V pueden tener los períodos de tiempo con conectividad limitada a HGS.

## <a name="fallback-configuration"></a>Configuración de reserva

A partir de Windows Server versión 1709, puede configurar un conjunto adicional de URL de servicio de protección de Host en hosts de Hyper-V para su uso cuando su HGS principal no responde.
Esto le permite ejecutar un clúster local de HGS que se usa como servidor principal para mejorar el rendimiento con la capacidad de retroceder a HGS del centro de datos corporativo, si los servidores locales están inactivos.

Para usar la opción de reserva, deberá configurar dos servidores HGS. Puede ejecutar Windows Server 2019 o Windows Server 2016 y, o bien ser parte de los clústeres de la mismos u otro. Si son diferentes clústeres, desea establecer las prácticas operativas para garantizar las directivas de atestación están sincronizadas entre los dos servidores. Ambos deben poder autorizar correctamente el host de Hyper-V para ejecutar máquinas virtuales blindadas y que el material de clave necesario para iniciar las máquinas virtuales blindadas. Puede elegir tener un par de cifrado compartido y la firma de certificados entre los dos clústeres, o utilizar certificados independientes y configurar el HGS blindadas VM para autorizar a ambos protecciones (pares de certificado de cifrado o firma) de los datos de blindaje archivo.

A continuación, actualizar los hosts de Hyper-V en Windows Server versión 1709 o Windows Server 2019 y ejecute el siguiente comando:
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Para quitar la configuración de un servidor de reserva, simplemente omita los dos parámetros de reserva:
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

En orden para el host de Hyper-V pasar la atestación con los servidores principales y de reserva, deberá asegurarse de que su información de atestación está al día con ambos clústeres HGS.
Además, los certificados usados para descifrar el TPM de la máquina virtual deben estar disponibles en ambos clústeres HGS.
Puede configurar cada HGS con certificados diferentes y configurar la máquina virtual para que confíe en ambos o agregar un conjunto compartido de certificados en ambos clústeres HGS.

Para obtener más información sobre cómo configurar HGS en una sucursal con direcciones URL de reserva, consulte la entrada de blog [mejorado soporte para sucursales para máquinas virtuales blindadas en Windows Server, versión 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Modo sin conexión

Modo sin conexión permite que la máquina virtual blindada activar cuando no se puede alcanzar HGS, siempre y cuando la configuración de seguridad de su host de Hyper-V no ha cambiado.
Modo sin conexión funciona al almacenar en caché una versión especial del protector de clave de VM TPM en el host de Hyper-V de.
El protector de clave se cifra en la configuración de seguridad actual del host (con la clave de identidad de seguridad basada en la virtualización).
Si el host no puede comunicarse con HGS y no ha cambiado su configuración de seguridad, podrá usar el protector de clave almacenada en caché para iniciar la máquina virtual blindada.
Cuando se cambia la configuración de seguridad en el sistema, como una nueva directiva de integridad de código que se va a aplicar o va a deshabilitar el arranque seguro, se invalidarán los protectores de clave almacenada en caché y el host tendrá dar fe con un HGS antes que las máquinas virtuales blindadas pueden iniciarse sin conexión nuevo.

Modo sin conexión requiere compilación de Windows Server Insider Preview 17609 o versiones más reciente para el clúster del servicio guardián de Host y el host de Hyper-V.
Se controla mediante una directiva de HGS, que está deshabilitada de forma predeterminada.
Para habilitar la compatibilidad para el modo sin conexión, ejecute el siguiente comando en un nodo de HGS:

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Puesto que los protectores de clave almacenables en caché son únicos en cada máquina virtual blindada, deberá apagar (no reiniciar) completamente e iniciar las máquinas virtuales blindadas para obtener un protector de clave almacenables en caché una vez que se habilita esta configuración de HGS.
Si la máquina virtual blindada migra a un host de Hyper-V que ejecuta una versión anterior de Windows Server, u Obtiene un nuevo protector de clave de una versión anterior de HGS, no podrá iniciar en modo sin conexión, pero puede seguir ejecutándose en modo en línea cuando el acceso a HGS está disponible se puede.
