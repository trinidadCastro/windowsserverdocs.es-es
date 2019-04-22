---
title: Funcionalidades de adición y desarrollo
description: Información del sistema le permite agregar nuevas capacidades predictivas para la información del sistema, sin necesidad de las actualizaciones del sistema operativo. Esto permite a los desarrolladores, incluidos Microsoft y terceros, para crear y entregar la nueva versión de mediados de capacidades para tratar los escenarios que le interesan. Nuevas capacidades pueden especificar datos personalizados para recopilar y analizar, y también se integran con los planos de administración de información del sistema existentes.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 8caddead774ac69a38906f3c0a0d2eaf005c1d28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817486"
---
# <a name="adding-and-developing-new-capabilities"></a>Agregar y desarrollar nuevas capacidades

>Se aplica a: Windows Server 2019

Información del sistema le permite agregar nuevas capacidades predictivas para la información del sistema, sin necesidad de las actualizaciones del sistema operativo. Esto permite a los desarrolladores, incluidos Microsoft y terceros, para crear y entregar la nueva versión de mediados de capacidades para tratar los escenarios que le interesan. 

Cualquier nueva capacidad puede integrar y ampliar la infraestructura existente de la información del sistema:

- Nuevas capacidades pueden **especificar cualquier evento del sistema o de contador de rendimiento**, que se recopilan, conserva localmente y se devolverá a la capacidad para el análisis cuando se invoca la capacidad.  
- Nuevas capacidades pueden **aprovechar el existente de Windows Admin Center y los planos de administración de PowerShell**. No sólo se nuevas capacidades pueden detectarse en el sistema de información, también se benefician de las programaciones personalizadas y las acciones de corrección. 

## <a name="manage-new-capabilities"></a>Nuevas capacidades de administración
- [Obtenga información sobre](add-remove-update-capabilities.md) cómo agregar, quitar y actualizar las funciones mediante PowerShell. 

## <a name="develop-a-capability"></a>Desarrollar una capacidad
Use los siguientes recursos para ayudarle a empezar a escribir sus propias funciones personalizadas:
- [Obtenga información sobre](data-sources.md) acerca de los orígenes de datos que puede recopilar.
- [Descargar](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) el paquete NuGet de información del sistema, que contiene las clases e interfaces necesarias para escribir una función.
- [Visite](https://aka.ms/systeminsights-api) la documentación de API para obtener información sobre las interfaces y clases de información del sistema. 
- [Use](https://aka.ms/systeminsights-samplecapability) la capacidad de ejemplo de información del sistema que le ayudarán a empezar a trabajar. Esto muestra cómo registrar una funcionalidad, especifique los orígenes de datos para recopilar y empezar a analizar los datos del sistema.

>[!NOTE]
>Se trata de funcionalidad de versión preliminar. Está sujeta a cambios, cuando se agregan nuevas funciones e incorporar comentarios.

## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Información general de información del sistema](overview.md)
- [Capacidades de descripción](understanding-capabilities.md)
- [Capacidades de administración](managing-capabilities.md)
- [Agregar, quitar y actualizar las capacidades](add-remove-update-capabilities.md)
- [Preguntas más frecuentes del sistema Insights](faq.md)