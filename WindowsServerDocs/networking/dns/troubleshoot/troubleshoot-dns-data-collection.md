---
title: Solución de problemas del sistema de nombres de dominio (DNS)
description: En este artículo se explica cómo recopilar datos cuando se producen problemas con DNS.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: a86b1f34c3b21f5bcde710e2a98323492ea51b62
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917782"
---
# <a name="troubleshooting-domain-name-system-dns-issues"></a>Solución de problemas del sistema de nombres de dominio (DNS)
 
Los problemas de resolución de nombres de dominio se pueden dividir en problemas del lado cliente y del lado servidor. En general, debe empezar con la solución de problemas del lado cliente a menos que determine durante la fase de ámbito que el problema se está produciendo en el servidor.

- [Solución de problemas de clientes DNS](troubleshoot-dns-client.md)

- [Solución de problemas de servidores DNS](troubleshoot-dns-server.md)
 
## <a name="data-collection"></a>Recopilación de datos
 
Se recomienda recopilar los datos simultáneamente en los lados cliente y servidor cuando se produce el problema. Sin embargo, en función del problema real, puede iniciar la colección en un solo conjunto de datos en el cliente DNS o en el servidor DNS.
 
Para recopilar un diagnóstico de red de Windows desde un cliente afectado y su servidor DNS configurado, siga estos pasos:

1. Iniciar capturas de red en el cliente y el servidor:

   ```cmd
   netsh trace start capture=yes tracefile=c:\%computername%_nettrace.etl
   ```

2. Borre la memoria caché de DNS en el cliente DNS ejecutando el comando siguiente:

   ```cmd
   ipconfig /flushdns
   ```

3. Reproduzca el problema.

4. Detener y guardar seguimientos:

   ```cmd
   netsh trace stop
   ```

5. Guarde los archivos archivo NetTrace. cab de cada equipo. Esta información resultará útil cuando se ponga en contacto con Soporte técnico de Microsoft.