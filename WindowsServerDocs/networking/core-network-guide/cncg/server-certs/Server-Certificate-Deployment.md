---
title: Implementación de certificados de servidor
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a10a9bafa6a8c9fddecac799ec8e837bf339d0e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment"></a>Implementación de certificados de servidor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Sigue estos pasos para instalar una entidad de certificación de raíz (CA) de empresa y para implementar certificados de servidor para su uso con PEAP y EAP.  
  
> [!IMPORTANT]  
> Antes de instalar servicios de certificados de Active Directory, debes nombre al equipo, configurar el equipo con una dirección IP estática y unir el equipo al dominio. Después de instalar AD CS, no puedes cambiar el nombre del equipo o la pertenencia a dominio del equipo, sin embargo, puedes cambiar la dirección IP si es necesario. Para obtener más información sobre cómo realizar estas tareas, consulta el servidor de Windows&reg; 2016 [guía básica de red](../../Core-Network-Guide.md).  

  
-   [Instalar el WEB1 de servidor Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Crear un registro de alias (CNAME) en DNS para WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurar WEB1 para distribuir listas de revocación de certificados (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparar el archivo inf CAPolicy](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Instalar la entidad de certificación](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurar las extensiones CDP y AIA en CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copia el certificado de CA y CRL en el directorio virtual](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurar la plantilla de certificado de servidor](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurar la inscripción automática de certificado de servidor](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Directiva de grupo de actualización](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Comprobar el servidor de inscripción de un certificado de servidor](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Los procedimientos descritos en esta guía no incluyen instrucciones para casos en que la **Control de cuentas de usuario** abre el cuadro de diálogo para solicitar su permiso para continuar. Si se abre este cuadro de diálogo mientras realizan los procedimientos descritos en esta guía y si el cuadro de diálogo se abrió en respuesta a las acciones, haz clic en **continuar**.  
  


