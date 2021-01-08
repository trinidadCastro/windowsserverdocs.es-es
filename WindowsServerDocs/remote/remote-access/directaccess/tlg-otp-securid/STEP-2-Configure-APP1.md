---
title: Paso 2 configurar APP1
description: Obtenga información acerca de cómo crear e implementar una plantilla de certificado que se usa para firmar solicitudes de certificados OTP y otra usada para certificados OTP emitidos por la entidad de certificación corporativa.
manager: brianlic
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 84d784975cbc5acfb1350ed6840ca3beb433e7cf
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040075"
---
# <a name="step-2-configure-app1"></a>Paso 2 configurar APP1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siga estos pasos para preparar APP1 para la compatibilidad con OTP:

1. Crear e implementar una plantilla de certificado que se usa para firmar solicitudes de certificado OTP. Configure una plantilla de certificado que se use para firmar solicitudes de certificado OTP.

2. Crear e implementar una plantilla de certificado para los certificados OTP emitidos por la entidad de certificación corporativa. Configure una plantilla de certificado para los certificados OTP emitidos por la entidad de certificación corporativa.

> [!WARNING]
> El diseño de esta guía del laboratorio de pruebas incluye servidores de infraestructura, como un controlador de dominio y una entidad de certificación (CA) que ejecutan Windows Server 2012 R2 o Windows Server 2012. El uso de esta guía del laboratorio de pruebas para configurar los servidores de infraestructura que ejecutan otros sistemas operativos no se ha probado y las instrucciones para configurar otros sistemas operativos no se incluyen en esta guía.

## <a name="to-create-and-deploy-a-certificate-template-used-to-sign-otp-certificate-requests"></a><a name="DAOTPRA"></a>Para crear e implementar una plantilla de certificado usada para firmar solicitudes de certificados OTP

1.  Ejecute **certtmpl. msc** y, a continuación, presione Entrar.

2.  En la consola de plantillas de certificado, en el panel de detalles, haga clic con el botón secundario en la plantilla **equipo** y haga clic en **plantilla duplicada**.

3.  En el cuadro de diálogo **propiedades de plantilla nueva** , en la pestaña **compatibilidad** , en la lista **entidad de certificación** , seleccione el sistema operativo que desee y, en el cuadro de diálogo **cambios resultantes** , haga clic en **Aceptar**. En la lista **destinatario del certificado** , seleccione el sistema operativo que desee y, en el cuadro de diálogo **cambios resultantes** , haga clic en **Aceptar**.

4.  En el cuadro **de diálogo Propiedades de plantilla nueva** , haga clic en la pestaña **General** .

5.  En la pestaña **General** , en **nombre para mostrar** de la plantilla, escriba **DAOTPRA**. Establezca el **período de validez** en 2 días y establezca el **período de renovación** en 1 día. Si aparece la advertencia **plantillas de certificado** , haga clic en **Aceptar**.

6.  Haga clic en la pestaña **Seguridad** y, a continuación, haga clic en **Agregar**.

7.  En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , haga clic en **tipos de objeto**. En el cuadro de diálogo **tipos de objeto** , seleccione **equipos** y, a continuación, haga clic en **Aceptar**. En el cuadro **Escriba los nombres de objeto que desea seleccionar** , escriba **EDGE1**, haga clic en **Aceptar** y, en la columna **permitir** , active las casillas de verificación **leer**, **inscribir** e **inscripción automática** . Haga clic en **usuarios autenticados**, seleccione la casilla de verificación **leer** en la columna **permitir** y desactive todas las demás casillas. Haga clic en **equipos del dominio** y desactive **inscribir** en la columna **permitir** . Haga clic en **Admins** . del dominio y **administradores de organización** y haga clic en **control total** en la columna **permitir** para ambos. Haga clic en **Aplicar**.

8.  Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**. En la lista **formato de nombre de sujeto:** seleccione **nombre DNS**, asegúrese de que la casilla **nombre DNS** está activada y haga clic en **aplicar**.

9. Haga clic en la pestaña **extensiones** , seleccione **directivas de aplicación** y, a continuación, haga clic en **Editar**. Quite todas las directivas de aplicación existentes. Haga clic en **Agregar** y, en el cuadro de diálogo **Agregar Directiva de aplicación** , haga clic en **nuevo**, escriba **da OTP RA** en el campo **nombre:** y **1.3.6.1.4.1.311.81.1.1** en el campo **identificador de objeto:** y haga clic en **Aceptar**. En el cuadro de diálogo **Agregar Directiva de aplicación** , haga clic en **Aceptar**. En la **extensión editar directivas de aplicación**, haga clic en **Aceptar**. En el cuadro **de diálogo Propiedades de plantilla nueva** , haga clic en **Aceptar**.

## <a name="to-create-and-deploy-a-certificate-template-for-otp-certificates-issued-by-the-corporate-ca"></a><a name="DAOTPLogon"></a>Para crear e implementar una plantilla de certificado para certificados OTP emitidos por la entidad de certificación corporativa

1.  En la consola de plantillas de certificado, en el panel de detalles, haga clic con el botón secundario en la plantilla de **Inicio de sesión de tarjeta inteligente** y haga clic en **plantilla duplicada**.

2.  En el cuadro de diálogo **propiedades de plantilla nueva** , en la pestaña **compatibilidad** de la lista **entidad de certificación** , haga clic en el sistema operativo que desee usar y, en el cuadro de diálogo **cambios resultantes** , haga clic en **Aceptar**. En la lista **destinatario del certificado** , seleccione el sistema operativo que desee usar y, en el cuadro de diálogo **cambios resultantes** , haga clic en **Aceptar**.

3.  En el cuadro **de diálogo Propiedades de plantilla nueva** , haga clic en la pestaña **General** .

4.  En la pestaña **General** , en **nombre para mostrar** de la plantilla, escriba **DAOTPLogon**. En **período de validez**, en la lista desplegable, haga clic en **horas**, en el cuadro de diálogo **plantillas de certificados** , haga clic en **Aceptar** y asegúrese de que el número de horas está establecido en 1. En **período de renovación**, escriba **0**.

    > [!IMPORTANT]
    > **CA de Windows Server 2003**. En situaciones en las que la entidad de certificación (CA) se encuentra en un equipo que ejecuta Windows Server 2003, la plantilla de certificado debe configurarse en un equipo diferente. Esto es necesario porque no es posible establecer el **período de validez** en horas cuando se ejecutan versiones de Windows anteriores a windows Server 2008 y Windows Vista. Si el equipo que utiliza para configurar la plantilla no tiene instalado el rol de servidor de servicios de Certificate Server de Active Directory o si es un equipo cliente, es posible que tenga que instalar el complemento plantillas de certificado. Para obtener más información, vea [instalar el complemento plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732445(v=ws.11)).
    >
    > **CA de Windows Server 2008 R2**. Si ya ha implementado una entidad de certificación (CA) que ejecuta Windows Server 2008 R2, debe configurar el **período de renovación** de la plantilla de certificado en 1 o 2 horas y el **período de validez** será mayor que el período de **renovación**, pero no más de 4 horas. Si configura un **período de validez** de plantilla de certificado de más de 4 horas con una CA que ejecuta Windows Server 2008 R2, el Asistente para la instalación de DirectAccess no puede detectar la plantilla de certificado y se produce un error en la instalación de DirectAccess.

5.  Haga clic en la pestaña **seguridad** , seleccione **usuarios autenticados**, en la columna **permitir** y active las casillas **leer** e **inscribir** . Haga clic en **Aceptar**. Haga clic en **Admins** . del dominio y **administradores de organización** y haga clic en **control total** en la columna **permitir** para ambos. Haga clic en **Aplicar**.

6.  Haga clic en la pestaña **nombre de sujeto** y, a continuación, haga clic en **compilar a partir de esta información Active Directory**. En la lista **formato de nombre de sujeto:** seleccione **nombre** completo, asegúrese de que el cuadro **nombre principal de usuario (UPN)** está activado y haga clic en **aplicar**.

7.  Haga clic en la pestaña **servidor** , active la casilla no **almacenar certificados y solicitudes en la base de datos de CA** , desactive la casilla no **incluir información de revocación en los certificados emitidos** y, a continuación, en el cuadro **de diálogo Propiedades de plantilla nueva** , haga clic en **aplicar**.

8.  Haga clic en la pestaña **requisitos de emisión** , active la casilla **este número de firmas autorizadas:** , establezca el valor en 1. En el **tipo de directiva necesario en firma:** lista, seleccione **Directiva de aplicación** y, en la lista **Directiva de aplicación** , seleccione **da OTP RA**. En el cuadro **de diálogo Propiedades de plantilla nueva** , haga clic en **Aceptar**.

9. Haga clic en la pestaña **extensiones** y, en **directivas de aplicación** , haga clic en **Editar**. Elimine la **autenticación del cliente**, mantenga **iniciosesióntarjetainteligente marcando** y haga clic en **Aceptar** dos veces.

10. Cierra la Consola de plantillas de certificado.

11. En la pantalla **Inicio** , escriba **CertSrv. msc** y, a continuación, presione Entrar.

12. En el árbol de la consola entidad de certificación, expanda **Corp-app1-CA-1**, haga clic en **plantillas de certificado**, haga clic con el botón secundario en **plantillas de certificado**, seleccione **nuevo** y haga clic en **plantilla de certificado para emitir**.

13. En la lista de plantillas de certificado, haga clic en **DAOTPRA** y **DAOTPLogon** y, a continuación, haga clic en **Aceptar**.

14. En el panel de detalles de la consola de, debería ver la plantilla de certificado **DAOTPRA** con un **propósito planteado** de **das OTP RA** y la plantilla de certificado **DAOTPLogon** con un **propósito planteado** de **Inicio de sesión de tarjeta inteligente**.

15. Reinicie los servicios.

16. Cierre la consola de entidad de certificación.

17. Abra un símbolo del sistema con privilegios elevados. Escriba **CertUtil.exe-SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS** y presione Entrar.

18. Deje abierta la ventana del símbolo del sistema para el siguiente paso.

