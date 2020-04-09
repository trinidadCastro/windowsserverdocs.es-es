---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Exportar la parte de la clave privada de un certificado de autenticación de servidor
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6baa734e3fc346d94f4387e2ed54d3e707e5af75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855428"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exportar la parte de la clave privada de un certificado de autenticación de servidor

Cada servidor de Federación de una Servicios de federación de Active Directory (AD FS) \(AD FS\) granja debe tener acceso a la clave privada del certificado de autenticación del servidor. Si está implementando una granja de servidores de servidores de Federación o servidores Web, debe tener un único certificado de autenticación. Este certificado debe ser emitido por una entidad de certificación empresarial \(CA\)y debe tener una clave privada exportable. La clave privada del certificado de autenticación de servidor debe ser exportable para que pueda estar disponible para todos los servidores de la granja.  
  
Este mismo concepto es aplicable a las granjas de servidores proxy de Federación en el sentido de que todos los servidores proxy de Federación de una granja deben compartir la parte de la clave privada del mismo certificado de autenticación de servidor.  
  
> [!NOTE]  
> El\-de administración de AD FS en se refiere a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.  
  
Según el rol que represente este equipo, use este procedimiento en el equipo del servidor de Federación o en el servidor proxy de Federación en el que instaló el certificado de autenticación del servidor con la clave privada. Cuando termines el procedimiento, puedes importar este certificado en el sitio web predeterminado de cada servidor de la granja. Para obtener más información, vea [importar un certificado de autenticación de servidor al sitio web predeterminado](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Para exportar la parte de la clave privada de un certificado de autenticación de servidor  
  
1. En la pantalla **Inicio** , escriba**Internet Information Services \(administrador de\) IIS**y, a continuación, presione Entrar.  
  
2. En el árbol de la consola, haz clic en **ComputerName**.  
  
3. En el panel central, haga doble\-haga clic en **certificados de servidor**.  
  
4. En el panel central, haga clic con el botón secundario\-en el certificado que desee exportar y, a continuación, haga clic en **exportar**.  
  
5. En el cuadro de diálogo **exportar certificado** , haga clic en **...** de puntos suspensivos (...).  
  
6. En **nombre de archivo**, escriba **C:\\** <em>nombredecertificado</em>y, a continuación, haga clic en **abrir**.  
  
7. Escribe la contraseña del certificado, confírmala y haz clic en **Aceptar**.  
  
8. Comprueba si el archivo que has especificado se ha creado en la ubicación especificada para confirmar que la exportación se ha realizado correctamente.  
  
   > [!IMPORTANT]  
   > Para que este certificado se pueda importar al almacén de certificados local del nuevo servidor, debes transferir el archivo a un medio físico y proteger su seguridad durante el transporte al nuevo servidor. Es extremadamente importante proteger la seguridad de la clave privada. Si esta clave se ve comprometida, la seguridad de toda la implementación de AD FS \(incluidos los recursos de su organización y en las organizaciones de asociados de recurso\) se ve comprometida.  
  
9. Importa el certificado de autenticación de servidor exportado en el almacén de certificados del nuevo servidor antes de instalar el Servicio de federación. Para obtener información sobre cómo importar el certificado, consulte Import a Server Certificate \([http:\/\/go.microsoft.com\/fwlink\/? LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para servidores proxy de federación](https://technet.microsoft.com/library/dd807054.aspx)  
  

