---
title: PASO 2 Configurar APP1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 612d3f4e9be71f60811b4499d9bdb8ae3bd217e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847496"
---
# <a name="step-2-configure-app1"></a>PASO 2 Configurar APP1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para preparar APP1 para el soporte técnico OTP:  
  
1. Para crear e implementar una plantilla de certificado que se usa para firmar solicitudes de certificado OTP. Configurar una plantilla de certificado que se usa para firmar solicitudes de certificado OTP.  
  
2. Para crear e implementar una plantilla de certificado para OTP los certificados emitidos por la CA de empresa. Configurar una plantilla de certificado para OTP los certificados emitidos por la CA de empresa.  
  
> [!WARNING]  
> El diseño de esta guía de laboratorio de pruebas incluye servidores de infraestructura, como un controlador de dominio y una entidad de certificación (CA) que ejecutan Windows Server 2012 R2 o Windows Server 2012. Uso de esta guía de laboratorio de pruebas para configurar servidores de infraestructura que ejecutan otros sistemas operativos no se ha probado y no se incluyen instrucciones para configurar otros sistemas operativos en esta guía.  
  
## <a name="DAOTPRA"></a>Para crear e implementar una plantilla de certificado que se usa para firmar solicitudes de certificado OTP  
  
1.  Ejecute **certtmpl.msc**, y, a continuación, presione ENTRAR.  
  
2.  En la consola de plantillas de certificado, en el panel de detalles, haga clic en el **equipo** plantilla y haga clic en **Duplicar plantilla**.  
  
3.  En el **propiedades de plantilla nueva** cuadro de diálogo el **compatibilidad** ficha la **entidad de certificación** lista, seleccione el sistema operativo que desee y en el  **Los cambios resultantes** haga clic en el cuadro de diálogo **Aceptar**. En el **destinatario del certificado** lista, seleccione el sistema operativo que desee, y en el **cambios resultantes** haga clic en el cuadro de diálogo **Aceptar**.  
  
4.  En el **propiedades de plantilla nueva** cuadro de diálogo, haga clic en el **General** ficha.  
  
5.  En el **General** ficha **nombre para mostrar plantilla**, tipo **DAOTPRA**. Establecer el **período de validez** en 2 días y establezca el **período de renovación** a 1 día. Si el **plantillas de certificado** advertencia se muestra, haga clic en **Aceptar**.  
  
6.  Haga clic en la pestaña **Seguridad** y, a continuación, haga clic en **Agregar**.  
  
7.  En el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo, haga clic en **tipos de objeto**. En el **tipos de objeto** cuadro de diálogo, seleccione **equipos**y, a continuación, haga clic en **Aceptar**. En el **escriba los nombres de objeto para seleccionar** , escriba **EDGE1**, haga clic en **Aceptar**y en el **permitir** columna, seleccione el **lectura** , **Inscribir**, y **inscripción automática** casillas de verificación. Haga clic en **usuarios autenticados**, seleccione el **lectura** casilla situada bajo la **permitir** columna y desactive todas las demás casillas. Haga clic en **equipos del dominio**y desactive la opción **inscribir** bajo el **permitir** columna. Haga clic en **Admins. del dominio** y **administradores de empresas** y haga clic en **Control total** bajo el **permitir** columna para ambos. Haga clic en **Aplicar**.  
  
8.  Haga clic en el **nombre de sujeto** pestaña y, a continuación, haga clic en **construido a partir de esta información de Active Directory**. En el **formato de nombre de sujeto:** lista select **nombre DNS**, asegúrese de que el **nombre DNS** casilla está activada y haga clic en **aplicar**.  
  
9. Haga clic en el **extensiones** ficha, seleccione **las directivas de aplicación** y, a continuación, haga clic en **editar**. Quite todas las directivas de aplicación existente. Haga clic en **agregar**y en el **agregar directivas de aplicación** cuadro de diálogo, haga clic en **New**, escriba **DA OTP RA** en el **nombre:** campo y **1.3.6.1.4.1.311.81.1.1** en el **identificador de objeto:** campo y haga clic en **Aceptar**. En el **agregar directivas de aplicación** cuadro de diálogo, haga clic en **Aceptar**. En el **Editar extensión de directivas de aplicación**, haga clic en **Aceptar**. En el **propiedades de plantilla nueva** cuadro de diálogo, haga clic en **Aceptar**.  
  
## <a name="DAOTPLogon"></a>Para crear e implementar una plantilla de certificado para OTP los certificados emitidos por la CA de empresa  
  
1.  En la consola de plantillas de certificado, en el panel de detalles, haga clic en el **inicio de sesión de tarjeta inteligente** plantilla y haga clic en **Duplicar plantilla**.  
  
2.  En el **propiedades de plantilla nueva** cuadro de diálogo el **compatibilidad** pestaña en el **entidad de certificación** lista, haga clic en el sistema operativo que desea usar y en el **Cambios resultantes** cuadro de diálogo, haga clic en **Aceptar**. En el **destinatario del certificado** lista, seleccione el sistema operativo que desea usar, y en el **cambios resultantes** haga clic en el cuadro de diálogo **Aceptar**.  
  
3.  En el **propiedades de plantilla nueva** cuadro de diálogo, haga clic en el **General** ficha.  
  
4.  En el **General** ficha **nombre para mostrar plantilla**, tipo **DAOTPLogon**. En **período de validez**, en la lista desplegable, haga clic en **horas**, en el **plantillas de certificado** cuadro de diálogo, haga clic en **Aceptar**y asegúrese de que que el número de horas se establece en 1. En **período de renovación**, tipo **0**.  
  
    > [!IMPORTANT]  
    > **Windows Server 2003 CA**. En situaciones donde es la entidad de certificación (CA) en un equipo que ejecuta Windows Server 2003, a continuación, la plantilla de certificado debe configurarse en un equipo diferente. Esto es necesario porque si se establece la **período de validez** en horas no es posible cuando se ejecuta a las versiones de Windows anteriores a Windows Server 2008 y Windows Vista. Si el equipo que use para configurar la plantilla no tiene instalado el rol de servidor de servicios de certificados de Active Directory, o si es un equipo cliente, es posible que deba instalar el complemento de plantillas de certificado. Para obtener más información, consulte [instalar el complemento Plantillas de certificado](https://technet.microsoft.com/library/cc732445.aspx).  
    >   
    > **Windows Server 2008 R2 CA**. Si ya ha implementado una entidad de certificación (CA) que se está ejecutando Windows Server 2008 R2, debe configurar la plantilla de certificado **período de renovación** a 1 o 2 horas y el **período de validez** a puede superar el **período de renovación**, pero no más de 4 horas. Si configura una plantilla de certificado **período de validez** de más de 4 horas con una CA que se está ejecutando Windows Server 2008 R2, el Asistente para instalación de DirectAccess no puede detectar la plantilla de certificado y DirectAccess se produce un error en la instalación.  
  
5.  Haga clic en el **seguridad** ficha, seleccione **usuarios autenticados**, en el **permitir** columna y seleccione el **lectura** y **inscribir**  casillas de verificación. Haga clic en **Aceptar**. Haga clic en **Admins. del dominio** y **administradores de empresas**y haga clic en **Control total** bajo el **permitir** columna para ambos. Haga clic en **Aplicar**.  
  
6.  Haga clic en el **nombre de sujeto** pestaña y, a continuación, haga clic en **construido a partir de esta información de Active Directory**. En el **formato de nombre de sujeto:** lista select **un nombre completo**, asegúrese de que el **nombre principal de usuario (UPN)** casilla está activada y haga clic en **aplicar** .  
  
7.  Haga clic en el **Server** ficha, seleccione el **no almacenar certificados y solicitudes en la base de datos de CA** casilla de verificación, desactive la **no incluyen información de revocación de certificados emitidos** casilla de verificación y, a continuación, en el **propiedades de plantilla nueva** cuadro de diálogo, haga clic en **aplicar**.  
  
8.  Haga clic en el **requisitos de emisión** ficha, seleccione el **este número de firmas autorizadas:** casilla de verificación, establezca el valor en 1. En el **tipo de directiva necesario en la firma:** lista select **directiva de aplicación**y en el **directiva de aplicación** lista select **DA OTP RA**. En el **propiedades de plantilla nueva** cuadro de diálogo, haga clic en **Aceptar**.  
  
9. Haga clic en el **extensiones** ficha y en **las directivas de aplicación** haga clic en **editar**. Eliminar **autenticación de cliente**, mantener **Iniciosesióntarjetainteligente**y haga clic en **Aceptar** dos veces.  
  
10. Cierra la Consola de plantillas de certificado.  
  
11. En el **iniciar** , escriba**certsrv.msc**, y, a continuación, presione ENTRAR.  
  
12. En el árbol de consola entidad de certificación, expanda **corp-APP1-CA-1**, haga clic en **plantillas de certificado**, haga clic en **plantillas de certificado**, apunte a **New**y haga clic en **plantilla de certificado para emitir**.  
  
13. En la lista de plantillas de certificado, haga clic en **DAOTPRA** y **DAOTPLogon**y haga clic en **Aceptar**.  
  
14. En el panel de detalles de la consola debería ver el **DAOTPRA** plantilla de certificado con un **propósito planteado** de **DA OTP RA** y **DAOTPLogon** plantilla de certificado con un **propósito planteado** de **inicio de sesión de tarjeta inteligente**.  
  
15. Reinicie los servicios.  
  
16. Cierre la consola de entidad de certificación.  
  
17. Abre un símbolo del sistema con privilegios elevados. Tipo **CertUtil.exe - SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**, y presione ENTRAR.  
  
18. Deje la ventana de símbolo del sistema abierta para el siguiente paso.  
  


