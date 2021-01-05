---
title: Instalar complementos
description: Obtenga información acerca de cómo incluir complementos en todos los servidores o equipos cliente mediante su instalación antes de crear una imagen.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 0160795a117c43d4fddd3dee7df8d45434ac7553
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755051"
---
# <a name="install-add-ins"></a>Instalar complementos

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para incluir complementos en todos los servidores o equipos cliente, instálelos antes de crear una imagen. Estos complementos se incluirán automáticamente en todos los equipos que se hayan replicado a partir de dicha imagen. Puede instalar un complemento ejecutando el archivo .wssx o puede instalar archivos de complementos individuales siguiendo las instrucciones de la [documentación del SDK](https://go.microsoft.com/fwlink/?LinkID=248648)para cada tipo de complemento. Si lo instala mediante un archivo .wssx, el complemento se puede desinstalar a través del Administrador de complementos. Si instala los archivos individuales, no podrá gestionar el complemento desde el Administrador de complementos.

> [!NOTE]
>  Cualquier software que se instale o descargue en el servidor (incluidas las fichas del Panel y las notificaciones de estado) debe tener una interfaz localizada que coincida con el idioma del sistema operativo que está instalado en el servidor.

#### <a name="to-install-an-add-in"></a>Para instalar un complemento

1.  (Opcional) Si instala el complemento utilizando un archivo .wssx, siga los pasos que se indican a continuación:

    1.  Guarde el archivo <AddinName \> . WSSX en el equipo de referencia.

    2.  Haga doble clic en el archivo .wssx para abrir el Asistente de instalación de complementos.

    3.  Siga las instrucciones del asistente para completar la instalación.

2.  (Opcional) Instale los archivos individuales del complemento en las ubicaciones correspondientes como se indica en el SDK de cada tipo de complemento.

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)