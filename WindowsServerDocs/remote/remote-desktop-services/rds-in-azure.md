---
title: Implementar sin problemas RDS con ARM y Azure Marketplace
description: Obtenga información sobre cómo crear una pequeña implementación de RDS en Azure mediante plantillas ARM y Azure Marketplace.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: e4de3d4ac14a0dbc5500fd7ab8bd8f1568f3da53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823086"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Implementar sin problemas RDS con ARM y Azure Marketplace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows Server 2012 R2

Servicios de escritorio remoto (RDS) es la plataforma de elección para hospedar de forma rentable aplicaciones y escritorios de Windows. Puede usar un [oferta de Azure Marketplace](#basic-rds-through-the-azure-marketplace) o un [plantilla de inicio rápido](#Customized-RDS-using-Quickstart-templates) para crear rápidamente una RDS en la implementación de IaaS de Azure. Azure marketplace crea automáticamente un dominio de prueba, lo que un mecanismo simple y fácil para las pruebas y pruebas de concepto. Las plantillas de inicio rápido, por otro lado, permiten usar un dominio existente, haciéndolos una excelente herramienta para crear un entorno de producción. Una vez configurado, puede conectarse a las aplicaciones y escritorios publicados desde varias plataformas y dispositivos, con las aplicaciones de escritorio remoto de Microsoft para Windows, Mac, iOS y Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>RDS básica a través de Azure Marketplace

Creación de la implementación a través de Azure Marketplace es la forma más rápida para poner en marcha y ejecución. Cuando todo se ha completado, el entorno será similar a la [arquitectura RDS básica](desktop-hosting-logical-architecture.md#basic-deployment). La oferta crea todos los componentes RDS que necesita: todo lo que necesita hacer es proporcionar cierta información. 

Deberá proporcionar la siguiente información al implementar la oferta de Marketplace:
- Nombre de usuario administrador y la contraseña. Se trata de un nuevo usuario que se va a administrar la implementación.
- Nombre DNS y nombre de dominio de AD. Estos son los recursos nuevos que se crean. Asegúrese de que los nombres son significativos.
- Tamaño de máquina virtual. Podrá elegir el tamaño de las máquinas virtuales que se usará para los extremos RDSH. También manualmente, puede cambiar los tamaños tras la implementación inicial, que le ayudarán a optimizar las máquinas virtuales para las cargas de trabajo y coste.

Siga estos pasos para crear la implementación de RDS pequeño desde Azure Marketplace: 

1. Iniciar la implementación de RDS de Azure Marketplace:
   1. Inicie sesión en el [portal Azure](https://portal.azure.com).
   2. Haga clic en **New** para agregar la implementación.
   3. Escriba "RDS" en el campo de búsqueda y presione ENTRAR.
   4. Haga clic en **desarrollo/pruebas de servicios de escritorio remoto (RDS) - Basic -** y, a continuación, haga clic en **crear**.
   5. Siga los pasos descritos en el portal para crear e implementar RDS. Agregue los detalles de configuración de la clave, como la información enumerada anteriormente. 
2. Conectarse a la implementación. Cuando finalice la implementación, consulte la sección de salidas para los pasos finales completar y conectarse a la implementación.
   1. Descargue y ejecute [este script de PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) en el dispositivo de prueba para instalar los certificados necesarios para conectarse a la implementación de RDS. 
   
      Este paso sólo es necesario durante la fase de pruebas. Al implementar RDS en Azure en producción, asegúrese de seguir los procedimientos recomendados, como compras y utilizando un certificado SSL de confianza pública en los servidores web.

   2. Cuando se le solicite, inicie sesión en su cuenta de Azure. Seleccione la suscripción de Azure, el grupo de recursos y la dirección IP pública creada para esta nueva implementación.
   3. Cuando finalice la secuencia de comandos, inicie la página Web de escritorio remoto en el explorador predeterminado. Puede comprobar de nuevo la página Web de escritorio remoto mediante la comparación de la dirección URL de la página a la dirección DNS que proporcionó durante la implementación. 
   
      Inicie sesión con las credenciales de administrador que creó durante la implementación para ver el escritorio predeterminada publicado automáticamente. También puede enviar a los usuarios en el sitio Web de escritorio remoto para probar sus aplicaciones y escritorios.

      > [!TIP]
      > ¿Ha olvidado el nombre o el Administrador de usuario del dominio? Puede ir al nuevo grupo de recursos en el portal, haga clic en **implementaciones**y, a continuación, ver los parámetros que especificó.

Ahora que tiene una implementación de RDS, puede [agregar y administrar usuarios](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>RDS personalizada con plantillas de inicio rápido

Puede usar plantillas de Azure Resource Manager para implementar RDS en Azure. Esto es especialmente útil si desea una implementación de RDS básica pero tiene los componentes existentes (por ejemplo, AD) que desea usar. A diferencia de la oferta de Marketplace, puede realizar más personalizaciones, como el uso de una instancia de AD existente en una red virtual, con una imagen de sistema operativo personalizada para las máquinas virtuales RDSH y disposición en capas en la alta disponibilidad para los componentes RDS. Después de agregar en la alta disponibilidad para cada componente, el entorno será similar a la [altamente disponible arquitectura RDS](desktop-hosting-logical-architecture.md#highly-available-deployment).

Siga estos pasos para crear la implementación de RDS de tamaño reducido con una plantilla de RDS de Azure: 

1. Seleccione la plantilla de inicio rápido de Azure:
   1. Vaya a la [RDS Azure Quickstart Templates](https://aka.ms/rdautomation) sitio.
   2. Elija la plantilla que coincida con lo que está intentando hacer. Asegúrese de que cumple los requisitos previos para la plantilla específica. (Por ejemplo, si desea usar una imagen personalizada para las máquinas virtuales, asegúrese de que ya ha cargado esa imagen a una cuenta de almacenamiento de Azure.)
   3. Haga clic en **implementar en Azure**.
   4. Deberá proporcionar algunos detalles (por ejemplo, el nombre de usuario de administrador, nombre de dominio de AD) en el portal de Azure. Esto varía en función de la plantilla que elija.
   5. Haga clic en **compra**.
2. Conectarse a la implementación. 
   1. Descargue y ejecute [este script de PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) en el dispositivo de prueba para instalar los certificados necesarios para conectarse a la implementación de RDS. 
   
      Este paso sólo es necesario durante la fase de pruebas. Al implementar RDS en Azure en producción, asegúrese de seguir los procedimientos recomendados, como compras y utilizando un certificado SSL de confianza pública en los servidores web.

   2. Cuando se le solicite, inicie sesión en su cuenta de Azure. Seleccione la suscripción de Azure, el grupo de recursos y la dirección IP pública creada para esta nueva implementación.
   3. Cuando finalice la secuencia de comandos, inicie la página Web de escritorio remoto en el explorador predeterminado. Puede comprobar de nuevo la página Web de escritorio remoto mediante la comparación de la dirección URL de la página a la dirección DNS que proporcionó durante la implementación. 
   
      Inicie sesión con las credenciales de administrador que creó durante la implementación para ver el escritorio predeterminada publicado automáticamente. También puede enviar a los usuarios en el sitio Web de escritorio remoto para probar sus aplicaciones y escritorios.

      > [!TIP]
      > ¿Ha olvidado el nombre o el Administrador de usuario del dominio? Puede ir al nuevo grupo de recursos en el portal, haga clic en **implementaciones**y, a continuación, ver los parámetros que especificó.

Ahora que tiene una implementación de RDS, puede [agregar y administrar usuarios](rds-user-management.md).
