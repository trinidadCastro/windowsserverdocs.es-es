---
title: Actualizaciones de seguridad extendidas de Windows Server 2008 y 2008 R2
description: Aprenda a usar las actualizaciones de seguridad extendidas (ESU) de Windows Server 2008 y 2008 R2 una vez que haya finalizado su ciclo de vida de soporte técnico.
ms.prod: windows-server
ms.technology: server-general
ms.mktglfcycl: manage
author: iainfoulds
ms.author: iainfou
ms.topic: get-started-article
ms.localizationpriority: high
ms.date: 12/16/2019
ms.openlocfilehash: 83ab3663b2c03017ba1bf613a49c394be0511002
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162506"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Uso de las actualizaciones de seguridad extendidas (ESU) de Windows Server 2008 y 2008 R2

>Se aplica a: Windows Server 2008/2008 R2

Windows Server 2008 y Windows Server 2008 R2 llegan al final del ciclo de vida de soporte técnico el 14 de enero de 2020. El Canal de mantenimiento a largo plazo (LTSC) de Windows Server tiene un mínimo de diez años de soporte técnico: cinco de soporte técnico estándar y otros cinco de soporte extendido. Este soporte incluye actualizaciones de seguridad periódicas.

El final del soporte técnico también significa el final de las actualizaciones de seguridad. Este escenario puede provocar problemas de seguridad o de cumplimiento, así como poner en peligro las aplicaciones empresariales. Para disfrutar de la seguridad avanzada, el máximo rendimiento y la mayor innovación posible, Microsoft recomienda [actualizarse a la versión actual de Windows Server](modernize-windows-server-2008.md).

Si no puede actualizar todos los servidores antes de alcanzar la fecha límite del ciclo de vida de soporte técnico, las siguientes opciones pueden servir de ayuda para las aplicaciones y los datos durante la transición de la actualización:

* Migre las cargas de trabajo existentes de Windows Server 2008 y 2008 R2 tal cual a las máquinas virtuales de Azure.
    * Esta migración a Azure proporciona automáticamente otros tres años de actualizaciones de seguridad extendidas (ESU). Estas actualizaciones no suponen cargo adicional alguno sobre el costo de las máquinas virtuales de Azure y, además, no se requiere ninguna configuración adicional.
* Si compra una suscripción de actualización de seguridad extendida para los servidores, estará protegido hasta que esté preparado para realizar actualizar a una versión más reciente de Windows Server.
    * Estas actualizaciones se proporcionan para un máximo de tres años después de la fecha de finalización del ciclo de vida de soporte técnico.

Tras ese período hay ninguna otra opción para que los equipos reciban actualizaciones adicionales.

## <a name="what-are-extended-security-updates-for-windows-server"></a>¿Qué son las actualizaciones de seguridad extendidas para Windows Server?

Las actualizaciones de seguridad extendidas (ESU) para Windows Server incluyen actualizaciones de seguridad y boletines calificados como *críticos* e *importantes* durante un máximo de tres años después del 14 de enero de 2020. Las actualizaciones de seguridad extendidas no incluyen lo siguiente:

* Nuevas características
* Revisiones que no sean de seguridad solicitadas por el cliente
* Solicitudes de cambio de diseño

Para más información, consulte [Preguntas más frecuentes sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="register-for-extended-security-updates"></a>Registro para actualizaciones de seguridad extendidas

Para usar las actualizaciones de seguridad extendidas, cree una clave de activación múltiple (MAK) y aplíquela a los equipos con Windows Server 2008 y 2008 R2. Esta clave permite que los servidores de Windows Update sepan que puede seguir recibiendo actualizaciones de seguridad. Regístrese para las actualizaciones de seguridad extendidas y administre estas claves desde Azure Portal, aunque solo use equipos locales.

> [!NOTE]
> Si ejecuta máquinas virtuales con Windows Server 2008/2008 R2 en Azure, no es preciso que realice los pasos siguientes. Las máquinas virtuales de Azure se habilitan automáticamente para las actualizaciones de seguridad extendidas. No es necesario crear una clave y un recurso de actualización de seguridad extendida, y no se realiza ningún cargo adicional por el uso de actualizaciones de seguridad extendidas con máquinas virtuales de Azure.

> [!NOTE]
> Antes de seguir los pasos que se indican a continuación, envía un mensaje de correo electrónico a [winsvresuchamps@microsoft.com](mailto:winsvresuchamps@microsoft.com) con esta información para que se incluya como elemento aprobado en la lista de permitidos:
> * Nombre del cliente:
> * Suscripción de Azure:
> * Número de contrato EA (para ESU):
> * Número de servidores ESU:
> 
> El equipo revisará la información proporcionada y agregará el usuario o la suscripción a la lista de permitidos.
> 
> Si el solicitante no está en la lista de permitidos, es posible que se devuelva este error: [No se pudo encontrar el tipo de recurso en el espacio de nombres "Microsoft.WindowsESU"](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version).

Para registrar máquinas virtuales que no sean de Azure para actualizaciones de seguridad extendidas y crear una clave, siga estos pasos en Azure Portal:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. En el cuadro de búsqueda de la parte superior de Azure Portal, busque y seleccione **Actualizaciones de seguridad extendidas**.

    ![Buscar actualizaciones de seguridad extendidas en Azure Portal](media/extended-security-updates/esu-portal-search.png)

    Si es la primera vez que usa este tipo de actualizaciones, primero elija **+ Crear** un recurso de actualizaciones de seguridad extendidas. Si ya lo ha usado, seleccione el recurso en la lista.

1. En **Register for Extended Service Updates** (Registro para actualizaciones de servicio extendidas), seleccione **Get started** (Comenzar).

    ![Introducción a las actualizaciones de seguridad extendidas en Azure Portal](media/extended-security-updates/get-started-with-esu.png)

1. Para crear la primera clave, seleccione **Get key** (Obtener clave).

    ![Elija crear una clave en Azure Portal](media/extended-security-updates/get-key.png)

    > [!NOTE]
    > Para crear el recurso y la clave de la actualización de seguridad extendida, necesita una suscripción de Azure asociada a su cuenta. Si no la tiene, inicie sesión con otra cuenta de usuario o cree una suscripción de Azure mediante los pasos guiados que se muestran en el portal.

1. En **Detalles de Azure**, seleccione su suscripción de Azure, un grupo de recursos y una ubicación para la clave.

    En **Detalles de registro**, escriba la siguiente información:

    | Valor             | Value |
    |---------------------|-------|
    | Nombre de clave            | Un nombre para mostrar para la clave, como *Agreement01*. |
    | Número de contrato    | El número de contrato que ha generado el sistema de administración de contratos de licencias por volumen o MSLicense para programas Contrato Enterprise. |
    | Número de equipos | Elija el número de equipos en los que desea instalar las actualizaciones de seguridad extendidas con esta clave. |
    | Sistema operativo    | Elija el sistema operativo con el que se va a usar esta clave, como Windows Server 2008 o Windows Server 2008 R2. |

    Cuando esté preparado, seleccione **Examinar y registrar**.

1. Una vez que la validación ha finalizado correctamente, se muestra un resumen de las opciones del nuevo recurso del registro. Si fuera necesario, corrija los errores de validación o actualice la opción de configuración. Están disponibles los [términos de uso ](https://azure.microsoft.com/support/legal/) y la [directiva de privacidad](https://privacy.microsoft.com/privacystatement) de Azure.

    Active la casilla para confirmar que tiene equipos aptos y que la clave solo se va a usar dentro de su organización:

    ![Confirmar que la clave solo va a usarla la organización](media/extended-security-updates/confirm-key-usage.png)

    Cuando esté preparado, seleccione **Crear** para generar la clave de activación múltiple.

El registro de las actualizaciones de seguridad extendidas ya está disponible para que lo use con los equipos. La clave creada debe aplicarse a los equipos con Windows Server 2008 y 2008 R2 que desee que sigan siendo aptos para las actualizaciones de seguridad.

## <a name="download-and-apply-extended-security-updates"></a>Descarga y aplicación de actualizaciones de seguridad extendidas

La entrega, descarga y aplicación de actualizaciones de seguridad extendidas en Windows Server no se diferencian en nada de las de los procesos de implementación existentes. Las actualizaciones que se proporcionan a través de las actualizaciones de seguridad extendidas solo son para *Seguridad* y se publican el segundo martes de cada mes, que se denomina Patch Tuesday.

Las actualizaciones se pueden instalar con las herramientas y los procesos que ya estén en vigor. La única diferencia es que para que las actualizaciones se descarguen e instalen el sistema se debe registrar mediante la clave generada en la sección anterior.

En el caso de las máquinas virtuales de Azure, el proceso de habilitar el equipo para las actualizaciones de seguridad extendidas se completa automáticamente. Las actualizaciones se deben descargar e instalar sin necesidad de configuración adicional.
