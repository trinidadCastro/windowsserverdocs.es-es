---
title: "Crear una cuota automática"
description: "En este artículo se describe cómo crear cuotas automáticas en base a una plantilla de cuota"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2837df448434252470d783a6c06f0690ba09021
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="create-an-auto-apply-quota"></a>Crear una cuota automática

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Mediante el uso de cuotas automáticas, puedes asignar una plantilla de cuota a un volumen o carpeta principales. El Administrador de recursos del servidor de archivos genera automáticamente las cuotas que se basan en dicha plantilla. Las cuotas se generan para cada una de las subcarpetas existentes y las subcarpetas que crees en el futuro.

Por ejemplo, puedes definir una cuota automática para subcarpetas que se crean a petición, para usuarios de perfiles móviles o para nuevos usuarios. Cada vez que se crea una subcarpeta, se genera automáticamente una nueva entrada de cuota mediante el uso de la plantilla de la carpeta principal. Estas entradas de cuota generadas automáticamente pueden visualizarse como cuotas individuales en el nodo **Cuotas**. Cada entrada de cuota se puede mantener por separado.

## <a name="to-create-an-auto-apply-quota"></a>Para crear una cuota automática

1.  En **Administración de cuotas** haz clic en el nodo **Cuotas**.

2.  Haz clic con el botón derecho en **Cuotas** y luego haz clic en **Crear cuota** (o selecciona **Crear cuota** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear cuota**.

3.  En **Ruta de acceso de cuota**, escribe el nombre o busca la carpeta principal a la que se aplicará el perfil de cuota. La cuota automática se aplicará a cada una de las subcarpetas (actuales y futuras) de esta carpeta.

4.  Haz clic en **Aplicar plantilla automáticamente para crear cuotas en subcarpetas nuevas y existentes**.

5.  En **Derivar propiedades de esta plantilla de cuota**, selecciona la plantilla de cuota que quieras aplicar en la lista desplegable. Ten en cuenta que las propiedades de cada plantilla se muestran en **Resumen de las propiedades de la cuota**.

6.  Haz clic en **Crear**.

> [!Note]
> Puedes comprobar todas las cuotas generadas automáticamente si seleccionas el nodo **Cuotas** y luego seleccionas **Actualizar**. Se muestran una cuota individual para cada subcarpeta y el perfil de cuota automática del volumen o carpeta principales.

## <a name="see-also"></a>Consulta también

-   [Administración de cuotas](quota-management.md)
-   [Editar propiedades de cuota automática](edit-auto-apply-quota-properties.md)