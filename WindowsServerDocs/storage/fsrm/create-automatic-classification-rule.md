---
title: Crear una regla de clasificación automática
description: En este artículo se describe cómo crear una regla de clasificación para una propiedad.
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c472949228184c6202681d257412c046bbc90d37
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812306"
---
# <a name="create-an-automatic-classification-rule"></a>Crear una regla de clasificación automática

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El siguiente procedimiento te guiará por el proceso de crear una regla de clasificación. Cada regla define el valor de una sola propiedad. De forma predeterminada, una regla solo se ejecuta una vez y omite los archivos que ya tienen un valor de propiedad asignado. Pero puedes configurar una regla para evaluar archivos independientemente de si un valor ya está asignado a la propiedad.

## <a name="to-create-a-classification-rule"></a>Para crear una regla de clasificación

1.  En **Administración de clasificaciones** haga clic en el nodo **Reglas de clasificación**.

2.  Haz clic con el botón derecho en **Reglas de clasificación**y luego haz clic en **Crear una nueva regla** (o haz clic en **Crear una nueva regla** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Definiciones de regla de clasificación**.

3.  En la pestaña **Configuración de regla**, introduce la siguiente información:

    -   **Nombre de la regla**. Escriba un nombre para la regla.
    -   **Habilitado**. Esta regla solo se aplicará si se selecciona la casilla de verificación Habilitada. Para deshabilitar la regla, desactive esta casilla.
    -   **Descripción**. Escriba una descripción opcional de la regla.
    -   **Ámbito**. Haz clic en **Agregar** para seleccionar una ubicación en la que se aplicará esta regla. Puedes agregar varias ubicaciones o quitar una ubicación si haces clic en **Quitar**. La regla de clasificación se aplicará a todas las carpetas y a sus subcarpetas de esta lista.

4.  Escriba la información siguiente en la ficha **Clasificación**:

    -   **Mecanismo de clasificación**. Elige un método para asignar el valor de propiedad.
    -   **Nombre de propiedad**. Selecciona la propiedad a la que se asignará esta regla.
    -   **Valor de propiedad**. Selecciona el valor de propiedad al que se asignará esta regla.

5.  También puedes hacer clic en el botón **Avanzadas** para seleccionar más opciones. En la pestaña **Tipo de evaluación**, la casilla de verificación **Volver a evaluar los archivos** está desactivada de forma predeterminada. Las opciones que pueden seleccionarse aquí son las siguientes:

    -   **Volver a evaluar archivos** desactivada: Una regla se aplica a un archivo si y solo si no se ha establecido la propiedad especificada por la regla en cualquier valor en el archivo.
    -   Opción **Volver a evaluar los archivos** activada y opción **Sobrescribir el valor existente** seleccionada: la regla se aplicará a los archivos cada vez que se ejecute el proceso de clasificación automática. Por ejemplo, si un archivo tiene una propiedad booleana que está establecida en **Sí**, una regla que use el clasificador de carpetas para establecer todos los archivos en **No** con esta opción establecida dejará la propiedad establecida en **No**.
    -   **Volver a evaluar archivos** activada y el **los valores de agregado** opción seleccionada: La regla se aplicará a los archivos de cada vez que se ejecuta el proceso de clasificación automática. Pero cuando la regla ha decidido qué valor establecer en el archivo de propiedad, agrega ese valor con el que ya está establecido en el archivo. Por ejemplo, si un archivo tiene una propiedad booleana que está establecida en **Sí**, una regla que use el clasificador de carpetas para establecer todos los archivos en **No** con esta opción establecida dejará la propiedad establecida en **Sí**.

    En la pestaña **Parámetros de clasificación adicionales**, puedes especificar parámetros adicionales reconocidos por el método de clasificación seleccionado si introduces el nombre y el valor y haces clic en el botón **Insertar**.

6.  Haz clic en **Aceptar** o **Cancelar** para cerrar el cuadro de diálogo **Avanzadas**.

7.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Crear una propiedad de clasificación](create-classification-property.md)
-   [Administración de clasificaciones](classification-management.md)