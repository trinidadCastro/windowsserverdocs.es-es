---
title: Asistente para la limpieza de servidor
description: 'Tema de Windows Server Update Service (WSUS): uso del Asistente para la limpieza del servidor para administrar el espacio en disco'
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85713dc245e8e812fa0c715d037738feaeb1ca0e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891679"
---
# <a name="the-server-cleanup-wizard"></a>Asistente para la limpieza de servidor

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El Asistente para la limpieza del servidor está integrado en la interfaz de usuario y se puede usar para ayudarle a administrar el espacio en disco. Este asistente puede realizar las siguientes operaciones:

- Quitar las actualizaciones sin usar y las revisiones de actualización Quite todas las actualizaciones anteriores y actualice las revisiones que no se hayan aprobado.

- Eliminar equipos que no se pongan en contacto con el servidor elimine todos los equipos cliente que no se han conectado al servidor en treinta días o más.

- Eliminar archivos de actualización innecesarios elimine todos los archivos de actualización que no son necesarios para las actualizaciones o los servidores que siguen en la dirección.

- Rechazar las actualizaciones expiradas rechaza todas las actualizaciones que ha expirado Microsoft.

- Rechazar las actualizaciones reemplazadas rechaza todas las actualizaciones que cumplen todos los criterios siguientes:

  -   La actualización reemplazada no es obligatoria

  -   La actualización reemplazada se ha realizado en el servidor durante treinta días o más

  -   La actualización reemplazada no está informada actualmente según sea necesario por ningún cliente

  -   La actualización reemplazada no se implementó explícitamente en un grupo de equipos durante 90 días o más

  -   La actualización de reemplazo debe aprobarse para su instalación en un grupo de equipos

  > [!WARNING]
  >  En una jerarquía de WSUS, se recomienda encarecidamente que primero ejecute el proceso de limpieza en el servidor WSUS de nivel inferior, de baja o de réplica y, a continuación, suba la jerarquía. La ejecución incorrecta de la limpieza en cualquier servidor que precede en la cadena antes de ejecutar la limpieza en cada servidor que sigue en la cadena puede producir una incoherencia entre los datos que se encuentran en las bases de datos ascendentes y en las bases de datos de nivel inferior. Los datos no coincidentes pueden provocar errores de sincronización entre los servidores de nivel superior y descendente.
  >
  > [!IMPORTANT]
  >  Si quita contenido innecesario mediante el Asistente para la limpieza del servidor, también se quitan todos los archivos de actualización privados que ha descargado del sitio de catálogo de Microsoft Update. Debe volver a importar estos archivos después de ejecutar el Asistente para la limpieza del servidor.

Si las actualizaciones se aprueban mediante una regla de aprobación automática, pueden seguir estando en el estado aprobado y no se quitarán mediante el Asistente para la limpieza del servidor. Para quitar las actualizaciones aprobadas automáticamente que se encuentran en un estado aprobado, el administrador de WSUS debe, como mínimo, establecer manualmente el estado de aprobación de las actualizaciones reemplazadas en no aprobadas, de modo que se puedan rechazar mediante el Asistente para la limpieza del servidor. El Asistente para la limpieza del servidor garantizará que se aprueba una actualización más reciente y que ningún sistema cliente todavía está informando de esa actualización según sea necesario antes de marcar la actualización como rechazada.




