---
title: Solucionar problemas del firewall en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: d1a36ecbd62e658c6361c004cf0166a295641b35
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625048"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Solucionar problemas del firewall en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Si experimenta problemas en el acceso remoto, ejecute el Asistente para reparación del Acceso desde cualquier lugar.

### <a name="to-run-the-repair-anywhere-access-wizard"></a>Para ejecutar el Asistente para reparación del Acceso desde cualquier lugar

1. Abra el Panel.

2. Haga clic en **Configuración**, en la pestaña **Acceso desde cualquier lugar** y en **Reparar**.

3. Siga las instrucciones del Asistente para reparación del Acceso desde cualquier lugar.

   Si está usando una configuración de red avanzada o un firewall que no es de Microsoft, puede que necesite abrir puertos adicionales en el firewall. Los puertos de la tabla siguiente están registrados en la Autoridad de asignación de números de Internet (IANA).

|Número de puerto|Descripción|
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

