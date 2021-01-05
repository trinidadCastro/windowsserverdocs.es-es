---
title: Habilitar todos los servicios de integración en máquinas virtuales
description: Obtenga información acerca de qué hacer cuando uno o más servicios de integración están deshabilitados o no funcionan en una máquina virtual.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
ms.date: 8/16/2016
ms.openlocfilehash: 59ee108cbe2c6b3ff62c4cb6a9d57d1ec8c8ca65
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845930"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Habilitar todos los servicios de integración en máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Uno o más servicios de integración están deshabilitados o no funcionan en una máquina virtual.*

## <a name="impact"></a>Impacto

*Es posible que el servicio o la característica de integración no funcionen correctamente para las siguientes máquinas virtuales:*

\<list of virtual machine names>

## <a name="resolution"></a>Solución

*Use el complemento servicios o la herramienta de línea de comandos SC config para comprobar que el servicio está configurado para iniciarse automáticamente y no se ha detenido.*

#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Para configurar el modo en que se inicia un servicio con el complemento Servicios

1.  Use Servicios de Escritorio remoto o conexión a máquina virtual para conectarse a la máquina virtual e iniciar sesión en el sistema operativo invitado.

2.  Abra Servicios. (Haga clic en **Inicio**, haga clic en el cuadro **Iniciar búsqueda** , escriba **Services. msc** y, a continuación, presione Entrar).

3.  En el panel de detalles, haga clic con el botón secundario en el servicio que desee configurar y, a continuación, haga clic en **Propiedades**.

4.  En la pestaña **General** , en tipo de **Inicio** , haga clic en **automático**.

#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Para configurar el modo en que se inicia un servicio con SC config

1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).

2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.

3.  Reemplace <nombre de servicio> por el nombre del servicio y, a continuación, escriba:

    ```
    sc config <service-name> start=auto
    ```



