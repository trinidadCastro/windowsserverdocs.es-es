---
title: Opciones de actualización y conversión para Windows Server 2016
description: Explica todas las rutas de actualización admitidas para Windows Server 2016.
ms.date: 01/18/2017
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 68a684ad9c99014f8b0bab922de52319e99354b4
ms.sourcegitcommit: 2365a7b23e2eccd13be350306c622d2ad9d36bc8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96788109"
---
# <a name="upgrade-and-conversion-options-for-windows-server-2016"></a>Opciones de actualización y conversión para Windows Server 2016

>Se aplica a: Windows Server 2019 y Windows Server 2016

En este tema se incluye información sobre cómo actualizar a Windows Server&reg; 2016 desde una variedad de sistemas operativos anteriores y mediante diversos métodos.

El proceso de cambiar a Windows Server 2016 puede variar enormemente en función del sistema operativo desde el que se parte y el método elegido para hacerlo. Se usan los siguientes términos para distinguir las diferentes acciones que podrían tener lugar en una nueva implementación de Windows Server 2016.

- **Instalación** es el concepto básico de implantar un nuevo sistema operativo en el hardware. Una **instalación limpia** , en concreto, requiere que se elimine el sistema operativo anterior. Para obtener información sobre la instalación de Windows Server 2016, vea [Requisitos del sistema e información de instalación de Windows Server 2016](./system-requirements.md). Para obtener información sobre la instalación de otras versiones de Windows Server, consulte el tema sobre la [instalación y actualización de Windows Server](./installation-and-upgrade.md).

- **Migración** significa mover el sistema operativo existente a Windows Server 2016 transfiriéndolo a un conjunto de hardware o a una máquina virtual diferentes. La migración, que puede variar de forma notable según los roles de servidor que haya instalados, se aborda en profundidad en [Windows Server Installation, Upgrade, and Migration](./installation-and-upgrade.md) (Instalación, actualización y migración de Windows Server).

- La **actualización gradual del sistema operativo del clúster** es una nueva característica de Windows Server 2016 que permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a Windows Server 2016 sin detener la función Hyper-V o las cargas de trabajo del Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización gradual del sistema operativo del clúster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).

- **Conversión de licencia** en algunas versiones de los sistemas operativos: con un solo paso puede convertir una edición particular de la versión que usted tenga a otra edición de la misma versión. Para ello, solo necesita un sencillo comando y la clave de licencia correspondiente. Este proceso se denomina conversión de licencia. Por ejemplo, si ejecuta Windows Server 2016 Standard, puede realizar la conversión a Windows Server 2016 Datacenter.

- **Actualizar** significa pasar de la versión actual del sistema operativo a una versión más reciente sin cambiar de hardware. (A esto a veces se hace referencia como actualización local). Por ejemplo, si su servidor ejecuta Windows Server 2012 o Windows Server 2012 R2, puede actualizarlo a Windows Server 2016. Es posible actualizar de una versión de evaluación del sistema operativo a una versión comercial, de una versión comercial antigua a una nueva o, en algunos casos, de una edición de licencia por volumen a una edición comercial normal.

> [!IMPORTANT]
> La actualización funciona mejor en máquinas virtuales donde los controladores de hardware específicos de OEM no son necesarios para una actualización correcta.

> [!IMPORTANT]
> Para las versiones de Windows Server 2016 anteriores a 14393.0.161119-1705.RS1_REFRESH, **solo puedes hacer esta conversión de evaluación a comercial** con Windows Server 2016 instalado con la opción de Experiencia de escritorio (no la opción Server Core). A partir de la versión 14393.0.161119-1705.RS1_REFRESH y versiones posteriores, puedes convertir las ediciones de evaluación en comerciales, independientemente de la opción de instalación usada.

> [!IMPORTANT]
> Si el servidor usa la formación de equipos NIC, deshabilite la formación de equipos NIC antes de la actualización y después vuelva a habilitarla cuando termine la actualización. Consulte [Introducción a la formación de equipos NIC](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831648(v=ws.11)) para obtener más información.

## <a name="upgrading-previous-retail-versions-of-windows-server-to-windows-server-2016"></a>Actualizar versiones comerciales anteriores de Windows Server a Windows Server 2016

En la tabla siguiente se resume brevemente qué sistemas operativos de Windows **con licencia** (es decir, no de evaluación) pueden actualizarse a qué ediciones de Windows Server 2016.

Tenga en cuenta las siguientes orientaciones generales para las rutas compatibles:

- No se admiten actualizaciones de arquitecturas de 32 a 64 bits. Todas las ediciones de Windows Server 2016 son solo de 64 bits.
- No se admiten actualizaciones de un idioma a otro.
- Si el servidor es un controlador de dominio, vea [Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012](../identity/ad-ds/deploy/upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012.md) para obtener información importante.
- No se admiten las actualizaciones desde versiones preliminares (vista preliminar) de Windows Server 2016. Realice una instalación limpia a Windows Server 2016.
- No se admiten actualizaciones que cambian de la instalación Server Core al modo Servidor con Experiencia de escritorio (o viceversa).
- No se admiten las actualizaciones desde una instalación de Windows Server anterior a una copia de evaluación de Windows Server. Las versiones de evaluación deben instalarse como una instalación limpia.

Si no ve su versión actual en la columna de la izquierda, significa que no se admite la actualización a esta versión de Windows Server 2016.

Si ve más de una edición en la columna de la derecha, se admite la actualización a **cualquier** edición desde la misma versión inicial.

|Si ejecuta esta edición:|Puede realizar una actualización a estas ediciones:|
|-------------------|----------|
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|


## <a name="per-server-role-considerations-for-upgrading"></a>Consideraciones por rol de servidor para la actualización

Aún en rutas de actualización admitidas desde versiones comerciales anteriores a Windows Server 2016, ciertos roles de servidor que ya están instalados probablemente requieren de acciones o preparación adicional para que el rol siga funcionando después de la actualización. Consulte en la biblioteca de TechNet los temas específicos correspondientes a cada rol de servidor que quiera actualizar para conocer en detalle cualquier paso adicional que sea necesario.

## <a name="converting-a-current-evaluation-version-to-a-current-retail-version"></a>Convertir la versión de evaluación actual a una versión comercial

Puede convertir la versión de evaluación de Windows Server 2016 Standard a Windows Server 2016 Standard (versión comercial) o Datacenter (versión comercial). Asimismo, puede convertir la versión de evaluación de Windows Server 2016 Datacenter a su versión comercial.

> [!IMPORTANT]
> Para las versiones de Windows Server 2016 anteriores a 14393.0.161119-1705.RS1_REFRESH, solo puedes realizar esta conversión de evaluación a comercial con Windows Server 2016 instalado con la opción de Experiencia de escritorio (no la opción Server Core). A partir de la versión 14393.0.161119-1705.RS1_REFRESH y versiones posteriores, puedes convertir las ediciones de evaluación en comerciales, independientemente de la opción de instalación usada.

Antes de intentar convertir una versión de evaluación a comercial, compruebe que el servidor esté ejecutándose en una versión de evaluación. Para hacer esto, realice una de las acciones siguientes:

- Desde un símbolo del sistema con privilegios elevados, ejecute **slmgr.vbs /dlv**. Las versiones de evaluación incluirán EVAL en la salida.

- En la pantalla Inicio , abra el **Panel de control**. Abra **Sistema y seguridad** y después **Sistema**. Vea el estado de activación de Windows en el área de activación del Windows de la página del **sistema**. Haga clic en **Ver los detalles** en Activación de Windows para obtener más información sobre el estado de activación de Windows.

Si ya activó Windows, el escritorio muestra el tiempo restante en el período de evaluación.

Si el servidor ejecuta una versión comercial en vez de una versión de evaluación, consulta la sección Actualizar versiones comerciales anteriores de Windows Server a Windows Server 2016 de este tema para obtener instrucciones para actualizar a Windows Server 2016.

Para **Windows Server 2016 Essentials**: puedes realizar la conversión a la versión comercial completa proporcionando una clave OEM de licencia por volumen o comercial en el comando **slmgr.vbs**.

Si el servidor está ejecutando una versión de evaluación de Windows Server 2016 Standard o Windows Server 2016 Datacenter, puede convertirla en una versión comercial de la siguiente manera:

1.    Si el servidor es un **controlador de dominio**, no puede convertirlo en una versión comercial. En este caso, instale un controlador de dominio adicional en un servidor que ejecute una versión comercial y quite AD DS del controlador de dominio que se ejecuta en la versión de evaluación. Para más información, vea [Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012](../identity/ad-ds/deploy/upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012.md).
2.    Lea los términos de licencia.
3.    Desde un símbolo del sistema con privilegios elevados, determine el nombre de la edición actual con el comando **DISM /online /Get-CurrentEdition**. Tome nota del identificador de la edición, una forma abreviada del nombre de la edición. A continuación, ejecute **DISM /online /Set-Edition:\<edition ID\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula** y proporcione el identificador de edición y una clave de producto comercial. El servidor se reiniciará dos veces.

Para la versión de evaluación de Windows Server 2016 Standard, también puede convertir la versión comercial de Windows Server 2016 Datacenter en un paso usando este mismo comando y la clave de producto correspondiente.

> [!TIP]
> Para obtener más información sobre Dism.exe, consulta [Opciones de línea de comandos de DISM](https://go.microsoft.com/fwlink/?LinkId=192466).

## <a name="converting-a-current-retail-edition-to-a-different-current-retail-edition"></a>Convertir la edición comercial actual a una edición comercial distinta

En cualquier momento después de instalar Windows Server 2016, puedes ejecutar el programa de instalación para repararla (conocido a veces como reparación en contexto) o, en algunos casos, para convertir a una edición diferente.
Puedes ejecutar el programa de instalación para realizar una reparación en contexto en cualquier edición de Windows Server 2016. El resultado tendrá la misma edición con la que iniciaste.

Para Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter como se indica a continuación: Desde un símbolo del sistema con privilegios elevados, determine el nombre de la edición actual con el comando **DISM /online /Get-CurrentEdition**. Para Windows Server 2016 Standard será `ServerStandard`. Ejecuta el comando **DISM /online /Get-TargetEditions** para obtener el id. de la edición a la que se puede actualizar. Toma nota del id. de edición, una forma abreviada del nombre de la edición. A continuación, ejecute **DISM /online /Set-Edition:\<edition ID\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula** y proporcione el identificador de edición del destino y su clave de producto comercial. El servidor se reiniciará dos veces.

## <a name="converting-a-current-retail-version-to-a-current-volume-licensed-version"></a>Convertir la versión comercial actual a una versión de licencia por volumen actual

En cualquier momento después de instalar Windows Server 2016, puedes convertirla entre una versión comercial, una versión de licencia por volumen o una versión OEM. La edición no cambia durante la conversión. Si empiezas con una versión de evaluación, primero conviértela a la versión comercial y luego podrás convertirla entre las otras ediciones como se describe en este documento.

Para hacerlo, desde un símbolo del sistema con privilegios elevados, ejecute: **slmgr /ipk \<key\>** .

\<key\> es la clave de producto correspondiente de la versión de licencia por volumen, comercial u OEM.
