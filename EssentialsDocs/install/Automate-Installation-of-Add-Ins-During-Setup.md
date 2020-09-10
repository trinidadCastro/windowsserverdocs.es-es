---
title: Automatizar la instalación de complementos durante la instalación
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ebfd0950c585e2383f736818789e8a2dce67ad4d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623963"
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automatizar la instalación de complementos durante la instalación

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="automate-installing-add-ins-during-setup"></a><a name="BKMK_AddIns"></a> Automatizar la instalación de complementos durante la instalación
 Para instalar complementos durante la instalación, use el método PostIC.cmd, que se describe en la sección [Cómo crear el archivo PostIC.cmd para ejecutar tareas posteriores a la configuración inicial](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) de este documento.

 Agregue la siguiente entrada al archivo PostIC.cmd:

```
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q
```

 El complemento es compatible ahora con los pasos de preinstalación y de desinstalación personalizada.

 El paso de preinstalación se ejecuta antes de instalar los archivos **.msi** especificados en addin.xml. Cuando se ejecuta en modo interactivo, el cuadro de diálogo de progreso se muestra sin cambiar el progreso. El botón de cancelación está deshabilitado durante la fase de preinstalación. Para implementar un paso de preinstalación, agregue el contenido siguiente en addin.xml (directamente bajo Package):

> [!NOTE]
>  El esquema xml debe seguir exactamente el siguiente:

```
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <Id>...</Id>
  <Version>...</Version>
  <Name>...</Name>
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>
  <ServerBinary>...</ServerBinary>
  <ClientBinary32>...</ClientBinary32>
  <ClientBinary64>...</ClientBinary64>
  <SupportedSkus>...</SupportUrl>
  <SupportUrl>...</SupportUrl>
  <Location>...</Location>
  <PrivacyStatement>...</PrivacyStatement>
  <OtherBinaries>...</OtherBinaries>
  <Preinstall>
<Executable>exefile</Executable>
<NormalArgs>args-for-interactive-mode</NormalArgs>
<SilentArgs>args-for-silent-mode</SilentArgs>
<IgnoreExitCode>true</IgnoreExitCode>
  </Preinstall>
  <UninstallConfirm>...</UninstallConfirm>
</Package>
<¦>
<¦>
```

 Donde **exefile** es el archivo ejecutable del paquete de complemento para realizar el paso de preinstalación y se debe especificar. **NormalArgs** especifica argumentos que se pasarán a exefile en la línea de comandos cuando se utiliza el modo interactivo. En este modo, el exefile puede mostrar algunos diálogos emergentes para la interacción del usuario. **SilentArgs** especifica argumentos que se pasarán a exefile en la línea de comandos cuando se utiliza el modo silencioso (-q se especifica al invocar installaddin.exe). El exefile no debe mostrar ninguna ventana emergente en este modo. Si se especifica **IgnoreExitCode** como verdadero, el paso de preinstalación se considera siempre correcto; en caso contrario, el código de salida 0 indica éxito, 1 indica cancelación y otros valores indican fallo. Las etiquetas **NormalArgs**, **SilentArgs** y **IgnoreExitCode** son todas opcionales.

 Un paso de desinstalación personalizado se puede utilizar para cualquiera de las acciones siguientes:

- Sustituir el cuadro de diálogo de confirmación integrado.

- Rellenar cuadros de diálogo personalizados antes de la desinstalación.

- Realizar determinadas tareas antes de la desinstalación.

  Para implementar un paso de desinstalación, agregue el contenido siguiente en addin.xml (directamente bajo Package):

```
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <Id>...</Id>
  <Version>...</Version>
  <Name>...</Name>
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>
  <ServerBinary>...</ServerBinary>
  <ClientBinary32>...</ClientBinary32>
  <ClientBinary64>...</ClientBinary64>
  <SupportedSkus>...</SupportUrl>
  <SupportUrl>...</SupportUrl>
  <Location>...</Location>
  <PrivacyStatement>...</PrivacyStatement>
  <OtherBinaries>...</OtherBinaries>
  <Preinstall>¦</Preinstall>
<UninstallConfirm>
<Executable>full-path-to-exefile</Executable>
<Arguments>command-line-arguments</Arguments>
</UninstallConfirm>
</Package>
```

 Donde **full-path-to-exefile** especifica el exefile que ya está instalado en el sistema. **Argumentos** es opcional y especifica los argumentos de la línea de comandos para el exefile. El archivo exefile se invoca antes de que se abra el cuadro de diálogo integrado para confirmación de la desinstalación.

 En esta fase, el exefile puede realizar las siguientes tareas:

- Muestra algunos cuadros de diálogo emergentes para la interacción con el usuario.

- Realizar algunas tareas de fondo.

  El código de salida de este archivo exe determina cómo avanza el proceso de desinstalación:

- 0: el proceso de desinstalación continúa sin rellenar el cuadro de diálogo de confirmación integrado, porque el usuario ya lo ha confirmado. (este enfoque se puede utilizar para sustituir el cuadro de diálogo de confirmación integrado);

- 1: el proceso de desinstalación se cancela y, por último, se muestra un mensaje de cancelación al usuario. Todo permanece intacto;

- Otro: el proceso de desinstalación continúa con el diálogo de confirmación integrado, ya que el paso de desinstalación personalizado no está presente.

  Cualquier fallo que invoque exefile llevará al mismo comportamiento, ya que exefile devuelve un código diferente de 0 o 1.

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)