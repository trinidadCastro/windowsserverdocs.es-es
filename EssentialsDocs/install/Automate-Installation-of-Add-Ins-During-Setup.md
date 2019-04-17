---
title: "Automatizar la instalación de Add-Ins durante la instalación"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d4c547c2fec8e2b11e5c1d9bde46e55e91c9d6fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automatizar la instalación de Add-Ins durante la instalación

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_AddIns"></a>Automatizar instalar complementos durante la instalación  
 Para instalar complementos durante la instalación, usa el método PostIC.cmd descrito en el [crear el archivo PostIC.cmd para ejecutar tareas de configuración inicial de Post](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) sección de este documento.  
  
 Agrega la siguiente entrada a tu PostIC.cmd:  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 El complemento ahora admite pasos de preinstalación y desinstalación personalizados.  
  
 El paso de preinstalación se ejecuta antes de todos los instalar **.msi** archivos especificados en addin.xml. Cuando se ejecuta en el modo interactivo, el cuadro de diálogo de progreso será mostrará pero sin cambiar el progreso. Durante la fase de preinstalación, se deshabilita el botón de cancelación. Para implementar un paso de preinstalación, agrega el siguiente contenido addin.xml (directamente debajo de Package):  
  
> [!NOTE]
>  El esquema xml debe seguir exactamente lo siguiente:  
  
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
  
 Donde **exefile** es el archivo ejecutable en el paquete de complementos para realizar el paso de preinstalación y debe especificarse. **NormalArgs** especifica argumentos para pasar a exefile en modo de línea de comandos cuando interactivo se usa. En este modo, exefile puede mostrar algunos cuadros de diálogo de interacción del usuario. **SilentArgs** especifica argumentos para pasar a exefile en modo de línea de comandos cuando silenciosa se usa (-q se especifica al invocar installaddin.exe). Exefile no debería cualquier ventana emergente en este modo. Si **IgnoreExitCode** se especifica como true, el paso de preinstalación siempre se considera correcto, de lo contrario, el código de salida 0 indica éxito, 1 indica la cancelación y otros valores indican un error. Etiquetas **NormalArgs**, **SilentArgs**, y **IgnoreExitCode** son todas opcionales.  
  
 Un paso de desinstalación personalizado puede usarse para cualquiera de las siguientes acciones:  
  
-   Reemplazar el cuadro de diálogo de confirmación integrado.  
  
-   Rellenar diálogos personalizados antes de la desinstalación.  
  
-   Realizar ciertas tareas antes de la desinstalación.  
  
 Para implementar un paso de desinstalación, agrega el siguiente contenido addin.xml (directamente debajo de Package):  
  
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
  
 Donde **completa-path-to-exefile** especifica el exefile ya instalado en el sistema. **Argumentos** es opcional y especifica los argumentos de línea de comandos para exefile. Exefile se invoca antes de confirmación de desinstalación integrado cuadro de diálogo.  
  
 Exefile puede realizar las siguientes tareas en esta fase:  
  
-   Muestra varios diálogos emergentes para interacción del usuario.  
  
-   Realiza algunas tareas en segundo plano.  
  
 El código de salida de este archivo exe determina cómo el proceso de desinstalación:  
  
-   0: el proceso de desinstalación continúa sin rellenar el cuadro de diálogo de confirmación integrado, como ya ha confirmado el usuario. (este enfoque se puede usar para reemplazar el cuadro de diálogo de confirmación integrado).  
  
-   1: el proceso de desinstalación se cancela y finalmente se muestra un mensaje de cancelación al usuario. Todo permanece sin cambios;  
  
-   Other: el proceso de desinstalación continúa con el cuadro de diálogo de confirmación integrado, como el paso de desinstalación personalizado no está presente.  
  
 Cualquier error al invocar exefile conducirá al mismo comportamiento que si exefile devuelve un código distinto de 0 o 1.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)