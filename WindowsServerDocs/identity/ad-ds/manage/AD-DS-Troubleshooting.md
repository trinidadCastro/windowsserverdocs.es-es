---
ms.assetid: fd3bc84a-48eb-4f00-9dc2-846bf2c2668b
title: Solución de problemas de AD DS
description: Información general de la sección de solución de problemas de AD DS
ms.author: daveba
author: iainfoulds
manager: dcscontentpm
ms.date: 11/22/2019
ms.topic: article
ms.openlocfilehash: 923b2442d68590821dfdfccee15f732c47829f3a
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068117"
---
# <a name="ad-ds-troubleshooting"></a>Solución de problemas de AD DS

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

En esta sección se incluyen recomendaciones y procedimientos de solución de problemas para diagnosticar y corregir los problemas que pueden producirse durante la replicación de Active Directory. Se centra en cómo responder a las entradas del registro de eventos del servicio de directorio y cómo interpretar los mensajes que pueden notificar herramientas como Repadmin.exe y Dcdiag.exe.

Repadmin.exe y Dcdiag.exe están disponibles en todos los controladores de dominio que ejecutan Windows Server 2012 R2 o versiones posteriores. Para obtener más información sobre cómo usar estas herramientas para solucionar problemas, vea los siguientes artículos.

- [Configuración de un equipo para la solución de problemas Active Directory](../manage/troubleshoot/Configuring-a-Computer-for-Troubleshooting.md)
- [Solución de problemas de replicación de Active Directory](../manage/troubleshoot/Troubleshooting-Active-Directory-Replication-Problems.md)

Otra tecnología útil es el seguimiento de eventos para Windows (ETW). Puede usar ETW para solucionar problemas de comunicaciones LDAP entre los controladores de dominio. Para obtener más información, vea [usar ETW para solucionar problemas de conexiones LDAP](../manage/troubleshoot/troubleshoot-ldap-using-etw.md).

También puede instalar Herramientas de administración remota del servidor (RSAT) en un servidor miembro que ejecute Windows 10. Para obtener información sobre cómo instalar RSAT, consulte [herramientas de administración remota del servidor](../../../remote/remote-server-administration-tools.md).
