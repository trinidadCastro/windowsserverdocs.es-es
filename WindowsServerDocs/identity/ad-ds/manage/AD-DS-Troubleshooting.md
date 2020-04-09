---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: Solución de problemas de AD DS
description: Información general de la sección de solución de problemas de AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b9e1cf812234bc3b0bd81db7045ed83208f3a0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824368"
---
# <a name="ad-ds-troubleshooting"></a>Solución de problemas de AD DS

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

En esta sección se incluyen recomendaciones y procedimientos de solución de problemas para diagnosticar y corregir los problemas que pueden producirse durante la replicación de Active Directory. Se centra en cómo responder a las entradas del registro de eventos del servicio de directorio y cómo interpretar los mensajes que podrían notificar herramientas como repadmin. exe y DCDiag. exe.

Repadmin. exe y DCDiag. exe están disponibles en todos los controladores de dominio que ejecutan Windows Server 2012 R2 o versiones posteriores. Para obtener más información sobre cómo usar estas herramientas para solucionar problemas, vea los siguientes artículos.

- [Configuración de un equipo para la solución de problemas Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [Solución de problemas de replicación de Active Directory](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

Otra tecnología útil es el seguimiento de eventos para Windows (ETW). Puede usar ETW para solucionar problemas de comunicaciones LDAP entre los controladores de dominio. Para obtener más información, vea [usar ETW para solucionar problemas de conexiones LDAP](../manage/troubleshoot/troubleshoot-ldap-using-etw.md).

También puede instalar Herramientas de administración remota del servidor (RSAT) en un servidor miembro que ejecute Windows 10. Para obtener información sobre cómo instalar RSAT, consulte [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
