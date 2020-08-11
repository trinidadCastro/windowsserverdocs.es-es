---
title: Actualizaciones de seguridad extendidas de Windows Server 2008 y 2008 R2
description: Aprenda a usar las actualizaciones de seguridad extendidas (ESU) de Windows Server 2008 y 2008 R2 una vez que haya finalizado su ciclo de vida de soporte técnico.
ms.mktglfcycl: manage
author: iainfoulds
ms.author: iainfou
ms.topic: get-started-article
ms.localizationpriority: high
ms.date: 02/21/2020
ms.openlocfilehash: c74c8a278612d2ca47346ad95105f1258761494a
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990480"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Uso de las actualizaciones de seguridad extendidas (ESU) de Windows Server 2008 y 2008 R2

>Se aplica a: Windows Server 2008 y Windows Server 2008 R2

Windows Server 2008 y Windows Server 2008 R2 llegaron al final del ciclo de vida de soporte técnico el 14 de enero de 2020. El Canal de mantenimiento a largo plazo (LTSC) de Windows Server ofrece un mínimo de diez años de soporte técnico: cinco de soporte técnico estándar y otros cinco de soporte extendido. Este soporte incluye actualizaciones de seguridad periódicas.

El final del soporte técnico también significa el final de las actualizaciones de seguridad. Este escenario puede provocar problemas de seguridad o de cumplimiento, así como poner en peligro las aplicaciones empresariales. Para disfrutar de la seguridad avanzada, el máximo rendimiento y la mayor innovación posible, Microsoft recomienda [actualizarse a la versión actual de Windows Server](modernize-windows-server-2008.md).

Si aún no has actualizado los servidores, las siguientes opciones te ayudarán a proteger tus aplicaciones y datos durante la transición:

* Migre las cargas de trabajo existentes de Windows Server 2008 y 2008 R2 tal cual a las máquinas virtuales de Azure.
  * Esta migración a Azure proporciona automáticamente otros tres años de actualizaciones de seguridad extendidas (ESU). Estas actualizaciones no suponen cargo adicional alguno sobre el costo de las máquinas virtuales de Azure y, además, no se requiere ninguna configuración adicional.
* Si compra una suscripción de actualización de seguridad extendida para los servidores, estará protegido hasta que esté preparado para realizar actualizar a una versión más reciente de Windows Server.
  * Estas actualizaciones se proporcionan para un máximo de tres años después de la fecha de finalización del ciclo de vida de soporte técnico.

Después del período de tres años de actualizaciones extendidas, dejaremos de actualizar Windows Server 2008 y 2008 R2. Recomendamos actualizar la versión de Windows Server a una versión más reciente lo antes posible.

## <a name="what-are-extended-security-updates-for-windows-server"></a>¿Qué son las actualizaciones de seguridad extendidas para Windows Server?

Las actualizaciones de seguridad extendidas (ESU) para Windows Server incluyen actualizaciones de seguridad y boletines calificados como *críticos* e *importantes* durante un máximo de tres años después del 14 de enero de 2020. Las actualizaciones de seguridad extendidas no incluyen lo siguiente:

* Nuevas características
* Revisiones que no sean de seguridad solicitadas por el cliente
* Solicitudes de cambio de diseño

Para más información, consulte [Preguntas más frecuentes sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="how-to-use-extended-security-updates"></a>Cómo usar las actualizaciones de seguridad extendidas

Si ejecutas máquinas virtuales de Windows Server 2008 o 2008 R2 en Azure, estas se habilitan automáticamente para las actualizaciones de seguridad extendidas. No es necesario realizar ninguna configuración y no se aplica ningún cargo adicional por el uso de las actualizaciones de seguridad extendidas con las máquinas virtuales de Azure. Las actualizaciones de seguridad extendidas se entregan automáticamente a las máquinas virtuales de Azure si están configuradas para recibir actualizaciones.

En el caso de otros entornos, como las máquinas virtuales locales o los servidores físicos, debes solicitar y configurar manualmente las actualizaciones de seguridad extendidas. Puedes adquirir actualizaciones de seguridad extendidas a través de programas de licencias por volumen, como Contrato Enterprise (EA), Contrato Enterprise Subscription (EAS), Enrollment for Education Solutions (EES) o Inscripción del servidor y la nube (SCE).

Cuando hayas adquirido actualizaciones de seguridad extendidas, puedes usar uno de los métodos siguientes para obtener sus claves:

* Si quieres obtener las claves de las actualizaciones de seguridad extendidas en Azure Portal, puedes [registrarte para las actualizaciones de seguridad extendidas en Azure Portal](#register-for-extended-security-updates-on-azure-portal).
* También puedes [iniciar sesión en el Centro de servicios de licencias por volumen de Microsoft](#sign-in-to-the-microsoft-volume-licensing-service-center) para obtener tus claves sin usar Azure Portal.

### <a name="register-for-extended-security-updates-on-azure-portal"></a>Registro para actualizaciones de seguridad extendidas en Azure Portal

Para usar las actualizaciones de seguridad extendidas en máquinas virtuales que no son de Azure, crea una clave de activación múltiple (MAK) y aplícala a los equipos con Windows Server 2008 y 2008 R2. Esta clave MAK permite que los servidores de Windows Update sepan que puedes seguir recibiendo actualizaciones de seguridad. Regístrate para las actualizaciones de seguridad extendidas y administra estas claves desde Azure Portal, aunque solo uses equipos locales.

> [!NOTE]
> No es necesario registrarse para las actualizaciones de seguridad extendidas si ejecutas Windows Server 2008 y 2008 R2 en máquinas virtuales de Azure. En el caso de otros entornos, como las máquinas virtuales locales o los servidores físicos, [adquiere actualizaciones de seguridad extendidas](https://www.microsoft.com/licensing/how-to-buy/how-to-buy) antes de registrarte y tratar de usarlas.

Para registrar la máquina virtual para las actualizaciones de seguridad extendidas y crear una clave, abre Azure Portal y sigue estas instrucciones:

1. Inicia sesión en [Azure Portal](https://portal.azure.com/).
2. En el cuadro de búsqueda de la parte superior de Azure Portal, busca y selecciona **Extended Security Updates** (Actualizaciones de seguridad extendidas).

    ![Búsqueda de actualizaciones de seguridad extendidas en Azure Portal](media/extended-security-updates/esu-portal-search.png)

    Si es la primera vez que usas este tipo de actualizaciones, primero selecciona **+ Crear** para crear un recurso de actualizaciones de seguridad extendidas. Si ya lo ha usado, seleccione el recurso en la lista.

3. En **Register for Extended Service Updates** (Registro para actualizaciones de servicio extendidas), seleccione **Get started** (Comenzar).

    ![Introducción a las actualizaciones de seguridad extendidas en Azure Portal](media/extended-security-updates/get-started-with-esu.png)

4. Para crear la primera clave, seleccione **Get key** (Obtener clave).

    ![Elección de la creación de una clave en Azure Portal](media/extended-security-updates/get-key.png)

    Para crear el recurso y la clave de la actualización de seguridad extendida, necesitas una suscripción de Azure asociada a tu cuenta. Si no la tienes, inicia sesión con otra cuenta de usuario o crea una suscripción de Azure en Azure Portal.

    También debes asignar el rol Colaborador a la suscripción de Azure para que la actualización de seguridad funcione. Para comprobar el rol, escribe "Suscripciones" en el cuadro de búsqueda. Verás una tabla que te mostrará el rol junto al identificador y el nombre de la suscripción.

    Si no eres Colaborador, puedes solicitar al propietario de la suscripción que cambie tu rol. Para averiguar quién es el propietario de tu suscripción, ve a la tabla de roles descrita en el párrafo anterior y selecciona el nombre de tu suscripción. A continuación, ve al menú del lado izquierdo de la página y selecciona **Control de acceso (IAM)**  > **Asignaciones de roles** y busca la sección "Propietarios" en la tabla.

5. Si ves una página que dice "Register to get a Multiple Activation Key" (Registrarse para obtener una clave de activación múltiple), significa que debes solicitar acceso a la versión preliminar privada antes de poder usar las actualizaciones de seguridad extendidas. Si no ves esta página, ve al paso 6.

   Para solicitar acceso, selecciona **join the private preview** (unirse a la versión preliminar privada). Se abrirá una ventana de mensaje de correo electrónico. Este correo electrónico es la solicitud de acceso al equipo del producto.

    Incluye la siguiente información en tu solicitud:

    * Nombre de cliente
    * Id. de suscripción de Azure
    * Número de contrato EA (para ESU)
    * Número de servidores ESU

    Cuando hayas terminado, envía el correo electrónico.

    El equipo revisará la información que proporciones en el correo electrónico de la solicitud. Si todo parece correcto, te agregarán a la lista de aprobados.

    Si el equipo no aprueba tu solicitud, verás el siguiente error:

    [No se pudo encontrar el tipo de recurso en el espacio de nombres "Microsoft.WindowsESU"]().

6. En **Detalles de Azure**, seleccione su suscripción de Azure, un grupo de recursos y una ubicación para la clave.

    En **Detalles de registro**, escriba la siguiente información:

    | Valor             | Value |
    |---------------------|-------|
    | Nombre de clave            | Un nombre para mostrar para la clave, como *Agreement01*. |
    | Número de contrato    | El número de contrato que ha generado el sistema de administración de contratos de licencias por volumen o MSLicense para programas Contrato Enterprise. |
    | Número de equipos | Elija el número de equipos en los que desea instalar las actualizaciones de seguridad extendidas con esta clave. |
    | Sistema operativo    | Elija el sistema operativo con el que se va a usar esta clave, como Windows Server 2008 o Windows Server 2008 R2. |

    Cuando esté preparado, seleccione **Examinar y registrar**.

    >[!NOTE]
    >Asegúrate de que has seleccionado la suscripción de Azure a cuya la versión preliminar privada te uniste en el filtro global. Selecciona el botón **Filtrar** en la cinta de opciones de Azure Portal para comprobar el filtro de suscripción global.
    >
    > ![Una imagen de la cinta de opciones de Azure Portal con el botón Filtrar seleccionado](media/azure-ribbon-filter.png)

7. Una vez que la validación ha finalizado correctamente, se muestra un resumen de las opciones del nuevo recurso del registro. Si fuera necesario, corrija los errores de validación o actualice la opción de configuración. Están disponibles los [términos de uso ](https://azure.microsoft.com/support/legal/) y la [directiva de privacidad](https://privacy.microsoft.com/privacystatement) de Azure.

    Active la casilla para confirmar que tiene equipos aptos y que la clave solo se va a usar dentro de su organización:

    ![Confirmar que la clave solo va a usarla la organización](media/extended-security-updates/confirm-key-usage.png)

    Cuando esté preparado, seleccione **Crear** para generar la clave de activación múltiple.

El registro de las actualizaciones de seguridad extendidas ya está disponible para que lo uses con los equipos. La clave creada debe aplicarse a los equipos con Windows Server 2008 y 2008 R2 que desee que sigan siendo aptos para las actualizaciones de seguridad.

### <a name="sign-in-to-the-microsoft-volume-licensing-service-center"></a>Inicio de sesión en el Centro de servicios de licencias por volumen de Microsoft

Si no tienes acceso a Azure Portal, puedes usar el Centro de servicios de licencias por volumen para ver y descargar las claves de activación.

Para obtener las claves en el Centro de servicios de licencias por volumen:

1. Ve a la [página del Centro de servicios de licencias por volumen](https://www.microsoft.com/vlsc) e inicia sesión con tus credenciales de Azure.

2. Selecciona **Licencias** > **Resumen de relación** > **Id. de licencia** > **Claves de producto**.

Para más información sobre cómo obtener actualizaciones de seguridad extendidas para dispositivos Windows válidos, consulta [esta publicación de Tech Community](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/obtaining-extended-security-updates-for-eligible-windows-devices/ba-p/1167091#).

## <a name="download-and-apply-extended-security-updates"></a>Descarga y aplicación de actualizaciones de seguridad extendidas

La entrega, descarga y aplicación de actualizaciones de seguridad extendidas en Windows Server no se diferencian en nada de las de los procesos de implementación existentes. Las actualizaciones que se proporcionan a través de las actualizaciones de seguridad extendidas solo son para *Seguridad* y se publican el segundo martes de cada mes, que se denomina Patch Tuesday.

Las actualizaciones se pueden instalar con las herramientas y los procesos que ya estén en vigor. La única diferencia es que para que las actualizaciones se descarguen e instalen el sistema se debe registrar mediante la clave generada en la sección anterior.

En el caso de las máquinas virtuales de Azure, el proceso de habilitar el equipo para las actualizaciones de seguridad extendidas se completa automáticamente. Las actualizaciones se deben descargar e instalar sin necesidad de configuración adicional.