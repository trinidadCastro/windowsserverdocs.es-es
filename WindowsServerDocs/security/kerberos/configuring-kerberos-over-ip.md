---
title: Configuración de Kerberos para la dirección IP
description: Compatibilidad de Kerberos para los SPN basado en IP
ms.openlocfilehash: 30741f7a0f1978fcaa6ac83c98a54c07e1ef25c5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391526"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Los clientes Kerberos permiten los nombres de host de las direcciones IPv4 e IPv6 en los nombres de entidad de servicio (SPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

A partir de Windows 10 versión 1507 y Windows Server 2016, los clientes Kerberos pueden configurarse para admitir los nombres de host IPv4 e IPv6 en los SPN.

De forma predeterminada Windows no intentará la autenticación Kerberos para un host si el nombre de host es una dirección IP. Volverá a otros protocolos de autenticación habilitados como NTLM. Sin embargo, las aplicaciones son a veces se utilice NTLM y no utilizar Kerberos codificados para usar direcciones IP, lo que significa que la aplicación. Esto puede causar problemas de compatibilidad como mover entornos deshabilitar NTLM.

Para reducir el impacto de deshabilitar una nueva funcionalidad NTLM se introdujo que permite a los administradores usar direcciones IP como nombres de host en los nombres de entidad de servicio. Esta funcionalidad está habilitada en el cliente a través de un valor de clave del registro.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Para configurar la compatibilidad con nombres de host de dirección IP en los SPN, cree una entrada TryIPSPN. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. Este valor del registro debe establecerse en cada equipo cliente que necesita para tener acceso a recursos protegidos con Kerberos mediante la dirección IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configuración de un nombre de entidad de servicio como dirección IP

Un nombre de entidad de servicio es un identificador único usado durante la autenticación Kerberos para identificar un servicio en la red. Un SPN está formado por un servicio, nombre de host y, opcionalmente, un puerto en forma de `service/hostname[:port]` como `host/fs.contoso.com`. Windows registrarán varios SPN para un objeto de equipo cuando un equipo esté unido a Active Directory.

Las direcciones IP no se usan normalmente en lugar de los nombres de host porque las direcciones IP suelen ser temporales. Esto puede provocar conflictos y errores de autenticación como concesiones de direcciones del punto de expirar y renovación. Por lo tanto, registrar un SPN de basadas en direcciones IP es un proceso manual y solo debe usarse cuando es imposible cambiar a un nombre de host basado en DNS.

El enfoque recomendado es usar el [Setspn.exe](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) herramienta. Tenga en cuenta que un SPN solo se puede registrar en una sola cuenta en Active Directory a la vez por lo que se recomienda que las direcciones IP tienen concesiones estáticas si se usa DHCP.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Por ejemplo:

```
Setspn -s host/192.168.1.1 server01
```
