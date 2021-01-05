---
title: Solucionar problemas del firewall en Windows Server Essentials
description: Obtenga información acerca de cómo usar el Asistente para reparación del acceso desde cualquier lugar si tiene problemas con el acceso remoto.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 3cf84844a0bbca8128df1429c843d3e565318f95
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810072"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Solucionar problemas del firewall en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Si experimenta problemas en el acceso remoto, ejecute el Asistente para reparación del Acceso desde cualquier lugar.

### <a name="to-run-the-repair-anywhere-access-wizard"></a>Para ejecutar el Asistente para reparación del Acceso desde cualquier lugar

1. Abra el Panel.

2. Haga clic en **Configuración**, en la pestaña **Acceso desde cualquier lugar** y en **Reparar**.

3. Siga las instrucciones del Asistente para reparación del Acceso desde cualquier lugar.

   Si está usando una configuración de red avanzada o un firewall que no es de Microsoft, puede que necesite abrir puertos adicionales en el firewall. Los puertos de la tabla siguiente están registrados en la Autoridad de asignación de números de Internet (IANA).

|Número de puerto|Description|
|-----------------|-----------------|
|65500|Servicio web de certificados|
|65510 y 65515|Sitio web de implementación de equipos cliente|
|65520|Servicio web para equipos cliente Mac|
|65532|Marco de proveedores para las comunicaciones de bucle invertido de servidor|
|6602|Marco de proveedores para la comunicación entre los equipos cliente y servidor|

## <a name="see-also"></a>Consulte también

-   [Usar Acceso web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Administrar Acceso web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Administrar Acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)

-   [Soporte técnico de Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

