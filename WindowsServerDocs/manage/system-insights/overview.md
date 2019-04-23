---
title: Información general de información del sistema
description: Información del sistema es una nueva característica de análisis predictivo en Windows Server 2019. Las funcionalidades predictivas de sistema Insights - cada uno de ellos con el respaldo de un modelo de aprendizaje automático - analizar localmente datos del sistema de Windows Server, como los contadores de rendimiento y eventos, que proporcionan información detallada sobre el funcionamiento de los servidores y reducir el gastos operativos asociados con la administración de forma reactiva problemas en sus implementaciones.
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
ms.date: 5/23/2018
ms.openlocfilehash: ca471e5e747c7aaa369f5725290e16deab7a6660
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887246"
---
# <a name="system-insights-overview"></a>Información general de información del sistema

>Se aplica a: Windows Server 2019

Información del sistema es una nueva característica de análisis predictivo en Windows Server 2019. Las funcionalidades predictivas de sistema Insights - cada uno de ellos con el respaldo de un modelo de aprendizaje automático - analizar localmente datos del sistema de Windows Server, como los contadores de rendimiento y eventos, que proporcionan información detallada sobre el funcionamiento de los servidores y reducir el gastos operativos asociados con la administración de forma reactiva problemas en sus implementaciones. 

En Windows Server 2019, información del sistema incluye cuatro funciones predeterminadas centrados en la capacidad de previsión, predecir el futuro recursos de proceso, redes y almacenamiento basado en los patrones de uso anteriores. Información del sistema también se distribuye con un [infraestructura extensible](adding-and-developing-capabilities.md), por lo que Microsoft y fabricantes 3rd pueden agregar nuevas funcionalidades de predicción a información del sistema sin actualizar el sistema operativo. 

Puede administrar información del sistema a través de una intuitiva [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) extensión o [directamente a través de PowerShell](https://aka.ms/SystemInsightsPowerShell), y la información del sistema le permite configurar cada funcionalidad predictiva por separado según las necesidades de su implementación. Todos los resultados de predicción se publican en el registro de eventos, que le permite usar [Azure Monitor](https://azure.microsoft.com/services/monitor/) o [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) para agregar fácilmente y ver las predicciones de un grupo de máquinas.

![Extensión del sistema de información en Windows Admin Center, que muestra la capacidad de CPU, capacidad de previsión con un gráfico de la previsión de trazado](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Funcionalidad local
Información del sistema se ejecuta completamente localmente en Windows Server. Nueva funcionalidad incluida en Windows Server 2019 todos sus datos se recopila, persistente y analizan directamente en el equipo, que le permiten obtener capacidades de análisis predictivo sin ninguna conectividad de la nube.

Los datos del sistema se almacenan en el equipo, y estos datos se analizan mediante las funcionalidades de predicción que no requieren de reciclaje en la nube. Con los conocimientos del sistema, puede conservar los datos en el equipo y continuar beneficiándose de las funcionalidades de análisis predictivo. 

## <a name="get-started"></a>Comenzar

<iframe src="https://www.youtube-nocookie.com/embed/AJxQkx5WSaA" width="560" height="315" allowfullscreen></iframe>

>[!TIP]
>Vea estos breves vídeos para obtener información sobre la información que necesita para empezar a trabajar y con confianza administrar información del sistema: [Introducción a información del sistema en 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Requisitos
Información del sistema está disponible en cualquier instancia de Windows Server 2019. Se ejecuta en las máquinas host e invitado, en cualquier hipervisor y en cualquier nube.

### <a name="install-system-insights"></a>Instale el sistema Insights
>[!IMPORTANT]
>Sistema Insights recopila y almacena hasta un año de datos localmente. Si desea conservar los datos al actualizar el sistema operativo, **Asegúrese de usar la actualización inmediata**.

#### <a name="install-the-feature"></a>Instalar la característica
Puede instalar información del sistema mediante la extensión de Windows Admin Center:

![Experiencia de día 0 para la extensión de la información del sistema.](media/day-0-2.png)

También puede instalar directamente mediante Administrador del servidor del sistema Insights agregando el **sistema Insights** característica, o mediante el uso de PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Enviar comentarios
Nos encantaría recibir sus comentarios para ayudarnos a mejorar esta característica. Puede usar los siguientes canales para enviar comentarios:
- **Centro de opiniones**: Usar la herramienta Centro de opiniones en Windows 10 en el archivo de un error o comentarios. Al hacerlo, especifique:
    - **Categoría**: Servidor 
    - **Subcategoría**: Información del sistema
- **UserVoice**: Enviar solicitudes de características a través de nuestro [página UserVoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Compartir con sus compañeros a votar los elementos que son importantes para usted.
- **Correo electrónico**: Si desea enviar sus comentarios de forma privada en el equipo de características, envíe un correo electrónico a system-insights-feed@microsoft.com. Tenga en cuenta que puede que le sigue solicitemos usar Centro comentarios o UserVoice.

## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Capacidades de descripción](understanding-capabilities.md)
- [Capacidades de administración](managing-capabilities.md)
- [Agregar y desarrollo de capacidades](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes del sistema Insights](faq.md)