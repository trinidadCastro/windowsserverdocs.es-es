---
title: Implementación de certificados de servidor
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63ae9ef71b913feeeb28e9838f636b316ea2969f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318199"
---
# <a name="server-certificate-deployment"></a>Implementación de certificados de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para instalar una entidad de certificación (CA) raíz de empresa y para implementar certificados de servidor para su uso con PEAP y EAP.  
  
> [!IMPORTANT]  
> Antes de instalar Active Directory servicios de Certificate Server, debe asignar un nombre al equipo, configurar el equipo con una dirección IP estática y unir el equipo al dominio. Después de instalar AD CS, no puede cambiar el nombre del equipo o la pertenencia al dominio del equipo; sin embargo, puede cambiar la dirección IP si es necesario. Para obtener más información sobre cómo realizar estas tareas, vea la [Guía de red principal](../../Core-Network-Guide.md)de Windows Server&reg; 2016.  

  
-   [Instalar la WEB1 de servidor web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Crear un registro de alias (CNAME) en DNS para WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurar WEB1 para distribuir listas de revocación de certificados (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparar el archivo INF de CAPolicy](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Instalar la entidad de certificación](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configuración de las extensiones de CDP y AIA en CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copiar el certificado de entidad de certificación y la CRL en el directorio virtual](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurar la plantilla de certificado de servidor](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurar la inscripción automática de certificados de servidor](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Actualizar directiva de grupo](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Comprobar la inscripción del servidor de un certificado de servidor](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Los procedimientos de esta guía no incluyen instrucciones para los casos en que se abre el cuadro de diálogo **Control de cuentas de usuario** para solicitar permiso para continuar. Si aparece este cuadro de diálogo en respuesta a sus acciones mientras realiza los procedimientos de esta guía, haga clic en **Continuar**.  
  


