---
title: Agregue Alertas de estado
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2540c4af032d35f5be71338513030c59cb18953d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817629"
---
# <a name="add-health-alerts"></a>Agregue Alertas de estado

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Los complementos de estado ofrecen definiciones para alertas, comprobaciones de estado y reparaciones de problemas de red. Los complementos de estado están formados por archivos xml que anotan el código o los datos utilizados para evaluar la información del estado de una función específica. Los complementos de estado son creados por desarrolladores e instalados en el servidor y en el cliente por el administrador.  
  
 Consulte el [SDK de Soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648) para obtener más información acerca de cómo crear un complemento de estado.  
  
## <a name="installing-health-add-in-files"></a>Instalación de archivos de complementos de estado  
 Una vez el desarrollador ha creado los archivos XML, debe copiarlos en la ubicación correspondiente del servidor y los equipos cliente.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Para instalar los archivos xml en el servidor  
  
1. En la carpeta **%ProgramFiles%\Windows Server\Bin\Feature Definitions** , cree una nueva carpeta llamada **MyHealthAddIn**. Puede utilizar cualquier nombre para esta carpeta. Se recomienda que el nombre de la carpeta sea el mismo que el nombre de la función.  
  
2. Copie los archivo Definition.xml y Definition.xml.config en la nueva carpeta.  
  
3. Si ha creado archivos binarios para estados o acciones, debería copiar estos archivos en **%ProgramFiles%\Windows Server\Bin**.  
  
   Los equipos cliente ejecutan una tarea programada cada 6 horas que copia los archivos XML en la ubicación correspondiente. Puede forzar la sincronización entre el equipo cliente y el servidor ejecutando la tarea de forma manual.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Para instalar los achivos XML en el equipo cliente  
  
1.  Abra el Programador de tareas.  
  
2.  Ejecute **HealthDefintionUpdateTask** en el Programador de tareas.  
  
    > [!NOTE]
    >  Esta tarea no instala archivos binarios. Copie manualmente los archivos binarios en la carpeta **%ProgramFiles%\Windows Server\Bin** del equipo cliente.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)