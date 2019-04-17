---
title: Reemplazar la lista de proveedores de nombre de dominio
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ebaef0f88456f61fa229c9a18ee8014987fe7fa7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="replace-the-list-of-domain-name-providers"></a>Reemplazar la lista de proveedores de nombre de dominio

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes reemplazar la lista de proveedores de nombre de dominio que se muestra en el asistente Configurar nombre de dominio al completar las siguientes tareas:  
  

-   [Crear los archivos de servicio referencia](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Agrega una entrada al registro en el equipo de referencia](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Crear los archivos de servicio referencia](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Agrega una entrada al registro en el equipo de referencia](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a>Crear los archivos de servicio referencia  
 La herramienta de administración del servicio de referencia crea un conjunto de archivos que se usan para definir la lista de proveedores de nombre de dominio que se muestran en el asistente Configurar nombre de dominio. Un archivo con formato de XML se crea para cada región mundial y contiene información para los proveedores de nombre de dominio que especifiques en la herramienta. Los archivos creados por la herramienta deben ubicarse en una carpeta que se puede acceder a través de un vínculo seguro (HTTPS) que administres en Internet.  
  
##### <a name="to-create-the-referral-files"></a>Para crear los archivos de referencia  
  
1.  Abre la herramienta de administración del servicio de referencia.  
  
2.  Haz clic en **agregar**.  
  
3.  En el agregar un cuadro de diálogo de proveedor de nombre de dominio, escribe el nombre del proveedor de nombre de dominio.  
  
4.  Agrega los dominios de nivel superior que son compatibles con el proveedor de nombre de dominio. Para ello, haz clic en **agregar**, escribe el identificador de dominio de nivel superior y, a continuación, selecciona las regiones admitidas. Puedes seleccionar **todas las regiones**.  
  
5.  Escribe la descripción del proveedor de nombre de dominio.  
  
6.  Agrega las direcciones URL de todos los sitios Web asociados al proveedor de nombre de dominio.  
  
7.  Si un logotipo está disponible para el proveedor de nombre de dominio, agrégalo haciendo clic en **Cambiar logotipo**.  
  
8.  Haz clic en **guardar**.  
  
9. Repite los pasos 2a 8 para cada proveedor de nombre de dominio que quieras incluir en el asistente.  
  
10. Después de agregar todos los proveedores de nombre de dominio, elige la carpeta donde se ubicarán los archivos de referencia. Ten en cuenta al elegir una carpeta que los archivos de referencia deben tener acceso a través de un vínculo HTTPS.  
  
11. Haz clic en **generar archivos de sistema de archivos**.  
  
###  <a name="BKMK_AddRegistry"></a>Agrega una entrada al registro en el equipo de referencia  
 Debe agregarse una entrada al registro para especificar la ubicación donde el sistema operativo puede encontrar la referencia archivos de servicio.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Para agregar una clave al registro  
  
1.  En el equipo de referencia, haz clic en **inicio**, escribe **regedit**y, a continuación, presiona **ENTRAR**.  
  
2.  En el panel izquierdo, expande **HKEY_LOCAL_MACHINE**, expande **SOFTWARE**, expande **Microsoft**, expande **Windows Server**, expanda **administradores de dominios**y, a continuación, expande **proveedores**.  
  
3.  Haz clic en el **E423C85D-6B1F-4583-95E0-449D8263BAC4** clave y, a continuación, haz clic en **valor de cadena**.  
  
4.  Tipo **ReferralServerHttpsUri** el nombre de la cadena y después presiona **ENTRAR**.  
  
5.  Haz clic en el nuevo **ReferralServerHttpsUri** de cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  

6.  Escribe la dirección URL HTTPS que se usa para acceder a los archivos de referencia que creaste en [crear los archivos de servicio referencia](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)y, a continuación, haz clic en **Aceptar**.  

6.  Escribe la dirección URL HTTPS que se usa para acceder a los archivos de referencia que creaste en [crear los archivos de servicio referencia](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)y, a continuación, haz clic en **Aceptar**.  

  
    > [!IMPORTANT]
    >  Una barra diagonal (/) se requiere al final de la dirección URL.  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a>Problemas de estado de nombres de dominio  
 Si eres un partner agrega proveedores de nombre de dominio y usa una interfaz de programación de aplicaciones (API) en el SDK de Windows Server Essentials para establecer los Estados Unknown, Failed y CertificateRequestNotSubmitted para el certificado, el cliente recibe un mensaje incorrecto y el resultado de la configuración. Esto es porque los casos son administrados por excepciones en lugar de devolver un estado.  
  
 Los siguientes estados de dominio son errores y deberían informarse de ellos como un error:  
  
-   No se pudo  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 Los siguientes estados de dominio son correctos y deberían informarse de ellos como tales:  
  
-   Listo  
  
-   Pendiente  
  
-   InRenewal  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

