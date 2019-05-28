---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Exportar la parte de la clave privada de un certificado de autenticación de servidor
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c3a39f9d51ed8243118522ae37bc7d205a7ea416
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192142"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exportar la parte de la clave privada de un certificado de autenticación de servidor

Cada servidor de federación en un Active Directory Federation Services \(AD FS\) granja de servidores debe tener acceso a la clave privada del certificado de autenticación del servidor. Si está implementando una granja de servidores de federación o servidores Web, debe tener un certificado de autenticación único. Este certificado debe ser emitido por una entidad de certificación empresarial \(CA\), y debe tener una clave privada exportable. La clave privada del certificado de autenticación de servidor debe ser exportable para que pueda estar disponible para todos los servidores de la granja.  
  
Este mismo concepto es aplicable de granjas de servidores proxy de federación en el sentido de que todos los servidores proxy de federación en una granja de servidores deben compartir la parte de la clave privada del certificado de autenticación del servidor mismo.  
  
> [!NOTE]  
> El complemento de administración de AD FS\-en hace referencia a certificados de autenticación de servidor para los servidores de federación como certificados de comunicación de servicio.  
  
Según el rol que se reproducirá este equipo, utilice este procedimiento en el equipo del servidor de federación o equipo proxy de servidor de federación que se instaló el certificado de autenticación de servidor con la clave privada. Cuando termines el procedimiento, puedes importar este certificado en el sitio web predeterminado de cada servidor de la granja. Para obtener más información, consulte [importar un certificado de autenticación de servidor al sitio Web predeterminado](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Para exportar la parte de la clave privada de un certificado de autenticación de servidor  
  
1.  En el **iniciar** , escriba**Internet Information Services \(IIS\) Manager**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, haz clic en **nombreDeEquipo**.  
  
3.  En el panel central, haga doble\-haga clic en **certificados de servidor**.  
  
4.  En el panel central, derecho\-haga clic en el certificado que desea exportar y, a continuación, haga clic en **exportar**.  
  
5.  En el **Exportar certificado** cuadro de diálogo, haga clic en el **...** de puntos suspensivos (...).  
  
6.  En **nombre de archivo**, tipo **C:\\*** Nombredecertificado*y, a continuación, haga clic en **abierto**.  
  
7.  Escribe la contraseña del certificado, confírmala y haz clic en **Aceptar**.  
  
8.  Comprueba si el archivo que has especificado se ha creado en la ubicación especificada para confirmar que la exportación se ha realizado correctamente.  
  
    > [!IMPORTANT]  
    > Para que este certificado se pueda importar al almacén de certificados local del nuevo servidor, debes transferir el archivo a un medio físico y proteger su seguridad durante el transporte al nuevo servidor. Es extremadamente importante proteger la seguridad de la clave privada. Si esta clave está en peligro, la seguridad de toda la implementación de AD FS \(incluidos los recursos dentro de su organización y en las organizaciones asociadas\) está en peligro.  
  
9. Importa el certificado de autenticación de servidor exportado en el almacén de certificados del nuevo servidor antes de instalar el Servicio de federación. ¿Para obtener información sobre cómo importar el certificado, consulte Importar un certificado de servidor \( [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para servidores proxy de federación](https://technet.microsoft.com/library/dd807054.aspx)  
  

