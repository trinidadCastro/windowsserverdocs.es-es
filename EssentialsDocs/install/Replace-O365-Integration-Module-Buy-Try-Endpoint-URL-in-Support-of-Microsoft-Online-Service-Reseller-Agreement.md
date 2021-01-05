---
title: Reemplazar la dirección URL de punto de conexión de compra/prueba del módulo de integración de Microsoft 365 en cumplimiento del contrato de revendedor de servicio en línea de Microsoft
description: Obtenga información acerca de cómo reemplazar las direcciones URL de extremo que usa el módulo de integración de Microsoft 365 de Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4d01b7dbea4566bae962c73220c389c3a4e2c19a
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755371"
---
# <a name="replace-microsoft-365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Reemplazar la dirección URL de punto de conexión de compra/prueba del módulo de integración de Microsoft 365 en cumplimiento del contrato de revendedor de servicio en línea de Microsoft

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>
 Si es socio comercial del acuerdo de revendedor de servicios en línea de Microsoft (MOSRA), para asegurarse de que las transacciones de suscripción de clientes se procesan a través de su portal, deberá reemplazar las direcciones URL de extremo que usa el módulo de integración de Microsoft 365 de Windows Server Essentials.

 El módulo de integración utiliza las cuatro direcciones URL de extremo siguientes:

1.  Un extremo de compra de suscripción de Microsoft 365 Enterprise.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG SZ

    -   Nombre de clave = MOSRASTDBUY

    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/enterprisebuy.html

2.  Un punto de conexión de prueba de suscripción Microsoft 365 Enterprise.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG SZ

    -   Nombre de clave = MOSRASTDTRY

    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/enterprisetry.html

3.  Un punto de conexión de compra de suscripción de Microsoft 365 pequeña empresa Premium.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG SZ

    -   Nombre de clave = MOSRALITEBUY

    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/smallbizbuy.html

4.  Un punto de conexión de prueba de suscripción de Microsoft 365 Small Business Premium.

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   Tipo = REG SZ

    -   Nombre de clave = MOSRALITETRY

    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/smallbiztry.html

#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Para agregar una clave de dirección URL de extremo al Registro

1.  En el equipo de referencia. haga clic en **Inicio**, escriba **regedit** y, a continuación, presione ENTRAR.

2.  En el panel izquierdo, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** y, finalmente, **MSO**.

3.  Si MSO no existe, haga clic con el botón secundario en **Windows Server**, seleccione **Nuevo**, haga clic en **Clave** y después escriba **MSO** como nombre de la clave.

4.  Haga clic con el botón secundario en MSO y, a continuación, haga clic en **Valor de cadena**. Escriba uno de los siguientes nombres de cadena de extremo como nombre de la cadena:

    -   MOSRASTDBUY

    -   MOSRASTDTRY

    -   MOSRALITEBUY

    -   MOSRALITETRY

5.  Haga clic con el botón secundario sobre la nueva cadena en el panel derecho y a continuación haga clic en **Modificar**.

6.  Escriba la nueva dirección URL de extremo en el cuadro de texto **Información del valor** y haga clic en **Aceptar**.

7.  Repita los pasos 4 a 6 para cada nombre de cadena que se enumera en el paso 4.

## <a name="see-also"></a>Consulte también

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)

