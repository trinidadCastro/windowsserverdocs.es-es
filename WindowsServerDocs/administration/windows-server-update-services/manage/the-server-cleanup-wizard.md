---
title: Asistente para la limpieza de servidor
description: Tema de Windows Server Update Service (WSUS) - cómo usar el Asistente para la limpieza del servidor para administrar espacio en disco
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d9287bb8bfc0fd51c53c598ccbc1f0498942e2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439817"
---
# <a name="the-server-cleanup-wizard"></a>Asistente para la limpieza de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El Asistente para la limpieza del servidor se integra en la interfaz de usuario y puede usarse para ayudarle a administrar el espacio en disco. Este asistente puede realizar las siguientes operaciones:

- Quitar no utilizadas de las actualizaciones y revisiones de la actualización de quitar todas las actualizaciones más antiguas y las revisiones de actualización que no se han aprobado.

- Eliminar todos los equipos cliente que no se han conectado al servidor de treinta días o más de los equipos no ponerse en contacto con la eliminación del servidor.

- Elimine la eliminación de archivos de actualización no necesarios que todos los archivos que no son necesarios de actualización, las actualizaciones o los servidores de nivel inferior.

- Rechazar actualizaciones caducadas rechazar todas las actualizaciones que haya expirado por Microsoft.

- Rechaza las actualizaciones reemplazadas rechazar todas las actualizaciones que cumplen los siguientes criterios:

  -   La actualización reemplazada no es obligatoria

  -   La actualización reemplazada ha estado en el servidor durante treinta días o más

  -   La actualización reemplazada actualmente no se notifica según sea necesario para cualquier cliente

  -   La actualización reemplazada no explícitamente implementada en un grupo de equipos durante noventa días o más

  -   La actualización de reemplazo debe estar aprobada para instalar en un grupo de equipos

  > [!WARNING]
  >  En una jerarquía WSUS, se recomienda encarecidamente que ejecute primero el proceso de limpieza en el servidor WSUS de réplica/descendentes más bajo y, a continuación, mueva la jerarquía. Que se ejecuta incorrectamente Liberador de espacio en cualquier servidor que precede en antes de ejecutar la limpieza en todos los servidores de nivel inferior puede provocar una incoherencia entre los datos que se encuentra en las bases de datos de nivel superior y las bases de datos de nivel inferiores. La coincidencia de datos puede provocar errores de sincronización entre los servidores ascendentes y descendentes. 
  > 
  > [!IMPORTANT]
  >  Si quita el contenido innecesario con el Asistente para la limpieza de servidor, también se quitan todos los archivos de actualización privada que ha descargado desde el sitio del catálogo de Microsoft Update. Debe volver a importar estos archivos después de ejecutar al Asistente para la limpieza de servidor. 

Si hay actualizaciones aprobadas mediante una regla de aprobación automática, que pueden estar todavía en el estado "Aprobado" y no se quitará mediante la limpieza del servidor el Asistente para. Para quitar aprobado automáticamente las actualizaciones que están en un estado "approved", el Administrador de WSUS debe - mínimo - establecer manualmente el estado de aprobación de actualizaciones reemplazadas para "No aprobado" por lo que será aptos para el rechazo mediante el Asistente para la limpieza del servidor. Se ha aprobado la limpieza del servidor que Asistente garantizará una actualización más reciente y que ningún sistema de cliente sigue informando que actualizan según sea necesario antes de marcar la actualización como "Rechazada".




