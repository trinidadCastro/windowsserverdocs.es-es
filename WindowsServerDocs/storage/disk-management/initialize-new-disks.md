---
title: Inicializar nuevos discos
description: "En este artículo se describe cómo inicializar nuevos discos"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 587553746d45eceab654efd4d120038088d32991
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="initialize-new-disks"></a>Inicializar nuevos discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

## <a name="to-initialize-new-disks"></a>Para inicializar nuevos discos
1.  En Administración de discos, haz clic con el botón derecho en el disco que quieres inicializar y luego haz clic en **Inicializar disco**.

2.  En el cuadro de diálogo **Inicializar disco**, selecciona el disco o los discos que quieras inicializar. Puedes seleccionar el estilo de partición de registro de arranque maestro (MBR) o de tabla de particiones GUID (GPT).

## <a name="additional-considerations"></a>Consideraciones adicionales

-   Los discos nuevos aparecen como **No inicializados**. Antes de poder usar un disco, primero debes inicializarlo. Si inicias la Administración de discos después de agregar un disco, aparece el Asistente para inicializar disco para que puedas inicializarlo.

> [!NOTE]
> El nuevo disco se inicializa como un disco básico.

