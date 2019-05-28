---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importar un certificado de autenticación de servidor al sitio web predeterminado
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: da91a3e8c34c86f7fd03ca875b3800fdb6001750
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192125"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importar un certificado de autenticación de servidor al sitio web predeterminado

Después de obtener un servidor de certificados de autenticación de una entidad de certificación \(CA\), debe instalar manualmente ese certificado en el sitio Web predeterminado para cada servidor de federación o servidor proxy de federación en una granja de servidores.  
  
En el caso de los servidores web, debes instalar manualmente el certificado de autenticación de servidor en el sitio web correspondiente o en el directorio virtual donde se encuentra la aplicación federada.  
  
Si estás configurando una granja, asegúrate de realizar este procedimiento de forma idéntica (usando exactamente la misma configuración) en todos los servidores de la granja.  
  
> [!NOTE]  
> El complemento de administración de AD FS\-en hace referencia a certificados de autenticación de servidor para los servidores de federación como certificados de comunicación de servicio.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Para importar un certificado de autenticación de servidor al sitio web predeterminado  
  
1.  En el **iniciar** , escriba**Internet Information Services \(IIS\) Manager**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, haz clic en **nombreDeEquipo**.  
  
3.  En el panel central, haga doble\-haga clic en **certificados de servidor**.  
  
4.  En el panel **Acciones** , haz clic en **Importar**.  
  
5.  En el **importar certificado** cuadro de diálogo, haga clic en el **...** de puntos suspensivos (...).  
  
6.  Ve a la ubicación del archivo de certificado pfx, resáltalo y haz clic en **Abrir**.  
  
7.  Escribe la contraseña del certificado y, después, haz clic en **Aceptar**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para servidores proxy de federación](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

