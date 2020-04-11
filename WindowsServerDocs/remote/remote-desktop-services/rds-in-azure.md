---
title: Implementación sin problemas de RDS con ARM y Azure Marketplace
description: Aprenda cómo crear una pequeña implementación de RDS en Azure mediante plantillas de ARM y Azure Marketplace.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: 55f43e90aabd4c8ace3a333e3e6d86a49fb69210
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855088"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Implementación sin problemas de RDS con ARM y Azure Marketplace

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Servicios de Escritorio remoto (RDS) es la plataforma elegida para hospedar de forma rentable escritorios y aplicaciones Windows. Puede usar una [oferta de Azure Marketplace](#basic-rds-through-the-azure-marketplace) o una [plantilla de inicio rápido](#customized-rds-using-quickstart-templates) para crear rápidamente Servicios de Escritorio remoto en la implementación de IaaS de Azure. Azure Marketplace crea un dominio de prueba para usted, lo que lo convierte en un mecanismo simple y fácil para las pruebas de conceptos. Las plantillas de inicio rápido, por otro lado, te permiten utilizar un dominio existente, lo que las convierte en una gran herramienta para crear un entorno de producción. Una vez configurado, puede conectarse a los escritorios y aplicaciones publicados desde varias plataformas y dispositivos, mediante las aplicaciones del Escritorio remoto de Microsoft para Windows, Mac, iOS y Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>RDS básicos mediante Azure Marketplace

Crear la implementación mediante Azure Marketplace es la forma más rápida de ponerlo en funcionamiento. Cuando todo se haya completado, el entorno será similar a la [arquitectura de RDS básicos](desktop-hosting-logical-architecture.md#basic-deployment). La oferta crea todos los componentes de RDS que necesitas: todo lo que necesitas hacer es proporcionar cierta información. 

Deberás proporcionar la siguiente información al implementar la oferta de Marketplace:
- Nombre de usuario y contraseña del administrador. Se trata de un nuevo usuario que va a administrar la implementación.
- Nombre DNS y nombre de dominio de AD. Estos son NUEVOS recursos que se crean. Asegúrate de que los nombres son significativos.
- Tamaño de la máquina virtual. Podrás elegir el tamaño de las máquinas virtuales que usarás para los puntos de conexión RDSH. También puedes cambiar manualmente los tamaños después de la implementación inicial para ayudarte a optimizar las máquinas virtuales para tus cargas de trabajo y para el costo.

Sigue estos pasos para crear la implementación de RDS de tamaño reducido desde Azure Marketplace: 

1. Iniciar la implementación de RDS de Azure Marketplace:
   1. Inicia sesión en [Azure Portal](https://portal.azure.com).
   2. Haz clic en **Nuevo** para agregar la implementación.
   3. Escribe "RDS" en el campo de búsqueda y presiona ENTRAR.
   4. Haz clic en **Remote Desktop Services (RDS) - Basic - Dev/Test** y, a continuación, haz clic en **Crear**.
   5. Sigue los pasos descritos en el portal para crear e implementar los RDS. Vas a agregar los detalles de configuración principales, como la información mencionada arriba. 
2. Conéctate a la implementación. Cuando finalice la implementación, comprueba en la sección de salidas los pasos finales para completar y conectarse a la implementación.
   1. Descarga y ejecuta [este script de PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) en el dispositivo de prueba para instalar los certificados necesarios para conectarte a la implementación de RDS. 
   
      Este paso solo es necesario durante la fase de pruebas. Cuando implementes RDS en Azure en producción, asegúrate de seguir los procedimientos recomendados como la compra y el uso de un certificado SSL de confianza pública en los servidores web.

   2. Cuando se te solicite, inicia sesión en tu cuenta de Azure. Selecciona la suscripción, el grupo de recursos y la dirección IP pública de Azure creados para esta nueva implementación.
   3. Cuando finalices el script, la página web de Escritorio remoto se iniciará en tu explorador predeterminado. Para volver a comprobar la página web de Escritorio remoto, compara la dirección URL de la página con la dirección DNS que has proporcionado durante la implementación. 
   
      Inicia sesión con las credenciales de administrador que has creado durante la implementación para ver el escritorio predeterminado publicado para ti. También puedes enviar a los usuarios el sitio web de Escritorio remoto para probar sus escritorios y aplicaciones.

      > [!TIP]
      > ¿Has olvidado el nombre de dominio o el usuario administrador? Puedes volver al nuevo grupo de recursos del portal, hacer clic en **Implementaciones** y, después, consultar los parámetros que has introducido.

Ahora que tienes una implementación de RDS, puedes [agregar y administrar usuarios](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>RDS personalizado con plantillas de inicio rápido

Puedes usar las plantillas de Azure Resource Manager para implementar RDS en Azure. Esto es especialmente útil si quieres una implementación básica de RDS pero tienes componentes existentes (como AD) que quieres utilizar. A diferencia de la oferta de Marketplace, puedes realizar más personalizaciones, como utilizar una instancia de AD existente en una red virtual, utilizar una imagen de sistema operativo personalizada para las máquinas virtuales RDSH y aprovechar la alta disponibilidad de los componentes de RDS. Después de agregar alta disponibilidad a cada componente, tu entorno se verá como la [arquitectura RDS altamente disponible](desktop-hosting-logical-architecture.md#highly-available-deployment).

Sigue estos pasos para crear la implementación de RDS de tamaño reducido con una plantilla de RDS de Azure: 

1. Selecciona la plantilla de inicio rápido de Azure:
   1. Ve al sitio de [plantillas de inicio rápido de Azure para RDS](https://aka.ms/rdautomation).
   2. Elige la plantilla que coincida con lo que estás intentando hacer. Asegúrate de que cumple los requisitos previos para la plantilla específica. (Por ejemplo, si quieres usar una imagen personalizada para las máquinas virtuales, asegúrate de que ya has cargado esa imagen a una cuenta de almacenamiento de Azure).
   3. Haz clic en **Implementar en Azure**.
   4. Tendrás que proporcionar algunos detalles (como nombre de usuario de administrador o nombre de dominio de AD) en Azure Portal. Esto varía en función de la plantilla que elijas.
   5. Haz clic en **Comprar**.
2. Conéctate a la implementación. 
   1. Descarga y ejecuta [este script de PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) en el dispositivo de prueba para instalar los certificados necesarios para conectarte a la implementación de RDS. 
   
      Este paso solo es necesario durante la fase de pruebas. Cuando implementes RDS en Azure en producción, asegúrate de seguir los procedimientos recomendados como la compra y el uso de un certificado SSL de confianza pública en los servidores web.

   2. Cuando se te solicite, inicia sesión en tu cuenta de Azure. Selecciona la suscripción, el grupo de recursos y la dirección IP pública de Azure creados para esta nueva implementación.
   3. Cuando finalices el script, la página web de Escritorio remoto se iniciará en tu explorador predeterminado. Para volver a comprobar la página web de Escritorio remoto, compara la dirección URL de la página con la dirección DNS que has proporcionado durante la implementación. 
   
      Inicia sesión con las credenciales de administrador que has creado durante la implementación para ver el escritorio predeterminado publicado para ti. También puedes enviar a los usuarios el sitio web de Escritorio remoto para probar sus escritorios y aplicaciones.

      > [!TIP]
      > ¿Has olvidado el nombre de dominio o el usuario administrador? Puedes volver al nuevo grupo de recursos del portal, hacer clic en **Implementaciones** y, después, consultar los parámetros que has introducido.

Ahora que tienes una implementación de RDS, puedes [agregar y administrar usuarios](rds-user-management.md).
