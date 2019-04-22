---
title: Paso 3 al planear la implementación del certificado OTP
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb6be03ed5319a56f9507859e753c88e020ceea9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813876"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Paso 3 al planear la implementación del certificado OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear el servidor RADIUS, debe planear para los requisitos de certificación emisora (CA), incluidos la CA que emitirá certificados de la contraseña de un solo uso (OTP), la plantilla de certificado OTP y el certificado de autoridad de registro utilizado por el servidor remoto Servidor de acceso para firmar todas las solicitudes de certificado OTP de cliente de DirectAccess. Estos certificados se usan como sigue:  
  
1.  El cliente de DirectAccess solicita un certificado OTP, y el servidor de acceso remoto recibe la solicitud.  
  
2.  El servidor de acceso remoto comprueba las credenciales de OTP y si son válidos, el servidor actúa como una autoridad de registro y firma la solicitud de inscripción de certificado OTP con un certificado de firma de corta duración.  
  
3.  El servidor de acceso remoto envía la solicitud de inscripción de certificado firmado al cliente de DirectAccess  
  
4.  El cliente, a continuación, inscriba el certificado OTP desde la CA con las solicitudes de inscripción de certificado firmadas por el servidor.  
  
5.  La CA comprueba las credenciales y la solicitud.  
  
|Tarea|Descripción|  
|----|--------|  
|[3.1 planear la CA de OTP](#bkmk_3_1_CA)|Planee la entidad de certificación (CA) a usar para emitir certificados a los clientes de DirectAccess para la autenticación de OTP.|  
|[3.2 Planear la plantilla de certificado OTP](#bkmk_3_2_OTP_Cert)|Planear la plantilla de certificado OTP.|
|[3.3 planear el certificado de autoridad de registro](#bkmk_33RACert)|Planear el certificado de autoridad de registro para firmar todas las solicitudes de certificado de autenticación de OTP.|

## <a name="bkmk_3_1_CA"></a>3.1 planear la CA de OTP  
Para implementar DirectAccess con autenticación de contraseña de un solo uso (OTP), requieren una CA interna emitir los certificados de autenticación de OTP a equipos cliente de DirectAccess. Para ello, puede usar la misma CA interna que usan para emitir los certificados que se usan para la autenticación de equipo IPsec normal.  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 Planear la plantilla de certificado OTP  
Cada cliente de DirectAccess requiere un certificado de autenticación de OTP con el fin de obtener acceso a la red interna. Debe configurar una plantilla en la CA interna para el certificado OTP. Al configurar la plantilla de certificado OTP, tenga en cuenta lo siguiente:  
  
-   Deben tener todos los usuarios que necesitan para realizar la autenticación de OTP de lectura y permisos de inscripción para esta plantilla.  
  
-   El nombre de sujeto debe generarse a partir de la información de Active Directory, para asegurarse de que el nombre del sujeto coincida con el nombre de usuario OTP y no el nombre del servidor de acceso remoto que realiza la solicitud de certificado. El nombre de sujeto debe tener el formato de nombre completo y el nombre alternativo del firmante debe estar en formato UPN. Esto garantiza que el certificado OTP inscrito es válido para la autenticación Smartcard Kerberos.  
  
-   La finalidad prevista del certificado debe ser el inicio de sesión de tarjeta inteligente  
  
-   Emisión debe exigir una firma autorizada. La firma debe configurarse con la directiva de aplicación de OTP de DirectAccess predefinidos establecida en la plantilla de certificado de firma de autoridad de registro.  
  
-   El período de validez debe establecerse en una hora.  
  
    > [!NOTE]  
    > En situaciones donde el servidor de CA es un equipo con Windows Server 2003, a continuación, la plantilla debe configurarse en un equipo diferente. Esto es debido a que establecer el **período de validez** en horas no es posible cuando ejecuten versiones de Windows anteriores a Vista/2008. Si el equipo que use para configurar la plantilla no tiene instalada la función del servicio de certificación, o es un equipo cliente, es posible que deba instalar el complemento de plantillas de certificado. Para obtener más información sobre este tema, haga clic en [aquí](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   El período de renovación debe establecerse en 0.  
  
-   (Opcional) Los certificados y las solicitudes no se almacenan en la base de datos de CA.  
  
-   El parámetro de uso mejorado de clave del certificado debe establecerse correctamente, como sigue:  
  
    -   Para la plantilla de certificado de firma de registro de DirectAccess, use la clave 1.3.6.1.4.1.311.81.1.1.  
  
    -   Para la plantilla de certificado de autenticación de OTP, use la clave 1.3.6.1.4.1.311.20.2.2.  
  
## <a name="bkmk_33RACert"></a>3.3 planear el certificado de autoridad de registro  
Cuando los clientes de DirectAccess solicitan un certificado OTP, el servidor de acceso remoto recibe la solicitud del cliente. El servidor de acceso remoto inicia todas las solicitudes de certificado OTP de clientes que usen el certificado de autoridad de registro. La entidad de certificación emite certificados sólo si la solicitud está firmada por el certificado de autoridad de registro en el servidor de acceso remoto. El certificado debe emitirlo una CA interna, el certificado no es autofirmado. No tiene que ser emitido por la CA que emitió los certificados OTP, pero la CA que emite los certificados OTP debe confiar en la entidad de certificación que emite el certificado de firma de autoridad de registro.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 4: Planear la OTP para el servidor de acceso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


