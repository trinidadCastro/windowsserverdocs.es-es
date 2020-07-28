---
title: Configuración de Kerberos para la dirección IP
description: Compatibilidad con Kerberos para SPN basados en IP
author: daveba
ms.author: daveba
ms.openlocfilehash: 16feb7045508a854657834fcbb4ab850f9b8ac3b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181921"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Los clientes Kerberos permiten nombres de host de direcciones IPv4 e IPv6 en nombres de entidad de seguridad de servicio (SPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

A partir de Windows 10 versión 1507 y Windows Server 2016, los clientes Kerberos se pueden configurar para admitir nombres de host IPv4 e IPv6 en SPN.

De forma predeterminada, Windows no intentará la autenticación Kerberos para un host si el nombre de host es una dirección IP. Se revertirá a otros protocolos de autenticación habilitados como NTLM. Sin embargo, las aplicaciones a veces están codificadas para usar direcciones IP, lo que significa que la aplicación revertirá a NTLM y no usará Kerberos. Esto puede provocar problemas de compatibilidad a medida que los entornos se mueven para deshabilitar NTLM.

Para reducir el impacto de la deshabilitación de NTLM, se presentó una nueva funcionalidad que permite a los administradores usar direcciones IP como nombres de host en los nombres de entidad de seguridad de servicio. Esta funcionalidad está habilitada en el cliente a través de un valor de clave del registro.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Para configurar la compatibilidad con los nombres de host de direcciones IP en los SPN, cree una entrada TryIPSPN. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. Este valor del registro tendrá que establecerse en cada equipo cliente que necesite tener acceso a los recursos protegidos por Kerberos por dirección IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configuración de un nombre de entidad de seguridad de servicio como dirección IP

Un nombre de entidad de seguridad de servicio es un identificador único que se usa durante la autenticación Kerberos para identificar un servicio en la red. Un SPN se compone de un servicio, un nombre de host y, opcionalmente, un puerto en forma de, `service/hostname[:port]` por ejemplo, `host/fs.contoso.com` . Windows registrará varios SPN en un objeto de equipo cuando una máquina esté unida a Active Directory.

Normalmente no se usan direcciones IP en lugar de nombres de host porque las direcciones IP suelen ser temporales. Esto puede dar lugar a conflictos y errores de autenticación, ya que las concesiones de direcciones expiran y se renuevan. Por lo tanto, el registro de un SPN basado en direcciones IP es un proceso manual y solo se debe usar cuando es imposible cambiar a un nombre de host basado en DNS.

El enfoque recomendado es usar la herramienta [Setspn.exe](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) . Tenga en cuenta que un SPN solo se puede registrar en una cuenta única en Active Directory a la vez, por lo que se recomienda que las direcciones IP tengan concesiones estáticas si se usa DHCP.

```
Setspn -s <service>/ip.address> <domain-user-account>
```

Ejemplo:

```
Setspn -s host/192.168.1.1 server01
```
