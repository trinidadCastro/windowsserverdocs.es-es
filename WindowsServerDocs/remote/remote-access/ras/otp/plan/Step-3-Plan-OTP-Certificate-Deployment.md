---
title: Paso 3 planear la implementación de certificados OTP
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d34630b4faa8012eee73967a99bc0541f1305a09
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313519"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Paso 3 planear la implementación de certificados OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear el servidor RADIUS, debe planear los requisitos de entidad de certificación (CA), incluida la CA que emitirá certificados de contraseña de un solo uso (OTP), la plantilla de certificado OTP y el certificado de entidad de registro que usa el remoto. Acceso al servidor para firmar todas las solicitudes de certificado OTP de cliente de DirectAccess. Estos certificados se utilizan de la siguiente manera:  
  
1.  El cliente de DirectAccess solicita un certificado OTP y el servidor de acceso remoto recibe la solicitud.  
  
2.  El servidor de acceso remoto comprueba las credenciales de OTP y, si son válidas, el servidor actúa como una entidad de registro y firma la solicitud de inscripción de certificado OTP mediante un certificado de firma de corta duración.  
  
3.  El servidor de acceso remoto envía la solicitud de inscripción de certificado firmada de nuevo al cliente de DirectAccess  
  
4.  Después, el cliente inscribe el certificado OTP de la entidad de certificación mediante las solicitudes de inscripción de certificados firmadas por el servidor.  
  
5.  La entidad de certificación comprueba las credenciales y la solicitud.  
  
|Tarea|Descripción|  
|----|--------|  
|[3,1 planear la CA de OTP](#bkmk_3_1_CA)|Planear la entidad de certificación (CA) que se va a usar para emitir certificados a los clientes de DirectAccess para la autenticación de OTP.|  
|[3,2 planeamiento de la plantilla de certificado OTP](#bkmk_3_2_OTP_Cert)|Planee la plantilla de certificado OTP.|
|[3,3 planear el certificado de la autoridad de registro](#bkmk_33RACert)|Planifique el certificado de la autoridad de registro para firmar todas las solicitudes de certificado de autenticación de OTP.|

## <a name="31-plan-the-otp-ca"></a><a name="bkmk_3_1_CA"></a>3,1 planear la CA de OTP  
Para implementar DirectAccess con la autenticación de contraseña de un solo uso (OTP), necesita una CA interna para emitir los certificados de autenticación de OTP a los equipos cliente de DirectAccess. Para ello, puede usar la misma entidad de certificación interna que usa para emitir los certificados que se usan para la autenticación normal de equipos con IPsec.  
  
## <a name="32-plan-the-otp-certificate-template"></a><a name="bkmk_3_2_OTP_Cert"></a>3,2 planeamiento de la plantilla de certificado OTP  
Cada cliente de DirectAccess requiere un certificado de autenticación de OTP para obtener acceso a la red interna. Debe configurar una plantilla en la entidad de certificación interna para el certificado OTP. Tenga en cuenta lo siguiente al configurar la plantilla de certificado OTP:  
  
-   Todos los usuarios que necesiten realizar la autenticación de OTP deben tener permisos de lectura e inscripción para esta plantilla.  
  
-   El nombre del firmante se debe crear a partir de Active Directory información, para asegurarse de que el nombre del sujeto coincide con el nombre de usuario de OTP, y no con el nombre del servidor de acceso remoto que realiza la solicitud de certificado. El nombre de sujeto debe tener el formato de nombre completo y el nombre alternativo del sujeto debe estar en formato UPN. Esto garantiza que el certificado de OTP inscrito sea válido para la autenticación de la tarjeta inteligente de Kerberos.  
  
-   La finalidad prevista del certificado debe ser el inicio de sesión de tarjeta inteligente  
  
-   La emisión debe requerir una firma autorizada. La firma se debe configurar con la Directiva de aplicación OTP de DirectAccess predefinida en la plantilla de certificado de firma de autoridad de registro.  
  
-   El período de validez debe establecerse en una hora.  
  
    > [!NOTE]  
    > En situaciones en las que el servidor de CA es un equipo con Windows Server 2003, la plantilla debe estar configurada en un equipo diferente. Esto se debe al hecho de que no es posible establecer el **período de validez** en horas cuando se ejecutan versiones de Windows anteriores a 2008/vista. Si el equipo que utiliza para configurar la plantilla no tiene instalado el rol de servicio de certificación o es un equipo cliente, es posible que tenga que instalar el complemento plantillas de certificado. Para obtener más información sobre este tema, haga clic [aquí](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   El período de renovación debe establecerse en 0.  
  
-   Opta Los certificados y las solicitudes no deben almacenarse en la base de datos de CA.  
  
-   El parámetro uso mejorado de clave de certificado debe establecerse correctamente, como se indica a continuación:  
  
    -   Para la plantilla de certificado de firma de registro de DirectAccess, use la clave 1.3.6.1.4.1.311.81.1.1.  
  
    -   Para la plantilla de certificado de autenticación OTP, use la clave 1.3.6.1.4.1.311.20.2.2 de clave.  
  
## <a name="33-plan-the-registration-authority-certificate"></a><a name="bkmk_33RACert"></a>3,3 planear el certificado de la autoridad de registro  
Cuando los clientes de DirectAccess solicitan un certificado OTP, el servidor de acceso remoto recibe la solicitud del cliente. El servidor de acceso remoto firma todas las solicitudes de certificados OTP de los clientes que usan el certificado de la entidad de registro. La CA emite certificados solo si la solicitud está firmada por el certificado de la autoridad de registro en el servidor de acceso remoto. El certificado debe estar emitido por una CA interna, el certificado no se puede firmar automáticamente. No es necesario que lo emita la CA que emitió los certificados OTP, pero la CA que emite los certificados OTP debe confiar en la CA que emite el certificado de firma de autoridad de registro.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vea también  
  
-   [Paso 4: planear la OTP para el servidor de acceso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


