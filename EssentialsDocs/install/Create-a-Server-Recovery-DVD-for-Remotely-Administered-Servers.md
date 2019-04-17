---
title: "Crear un DVD de recuperación de servidor para servidores administrados de forma remota"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7bfe1686ac84962cdb4ab1cde8d6ca5226cb9d44
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Crear un DVD de recuperación de servidor para servidores administrados de forma remota

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a>Crear un DVD de recuperación del servidor para servidores administrados de forma remota  
 Hay dos modelos para la recuperación de restablecimiento rápido y el servidor de fábrica y se diferencian en el hardware que el cliente recibe.  
  
 **Servidor de administración remota**  
  
 Esta opción de recuperación de servidor requiere que el proceso se ejecuta desde un equipo cliente en la misma red. Dado que el restablecimiento de fábrica requiere que una imagen específica del hardware deben suministrarse con el servidor, el partner debe crear el DVD de recuperación del servidor.  
  
 **Administra el servidor de forma local**  
  
 Este es el modelo clásico donde el servidor se administra en la consola del servidor. Los medios de instalación del servidor se usan para ejecutar una recuperación. Esto requiere que el servidor se incluye con la capacidad para ver la salida de vídeo además de incluir un lector de DVDs. El cliente inicia desde ese medios de instalación del servidor y, a continuación, elige el método de recuperación adecuadas. No es necesario crear un DVD de recuperación del servidor para los servidores que se administra de forma local.  
  
> [!NOTE]
>  Para el modelo de servidor administrado de forma local, el cliente puede llevar a cabo una fábrica de restablecer, realizando una instalación nueva. Sin embargo, perderá al partner personalizaciones.  
  
 Hay dos tipos de recuperación del servidor.  
  
 **Restablecimiento de fábrica**  
  
 Esta recuperación devuelve el servidor a su estado original que existía cuando el servidor se envíe de fábrica. Después de un restablecimiento de fábrica, se le pedirá que realizar la configuración inicial del servidor, al igual que fuera la primera vez que lo encendiste y se perderán todas las configuraciones y personalizaciones. ¿Esto también se conoce como día 0.? Dado que el restablecimiento de fábrica requiere que una imagen específica del hardware deben suministrarse con el servidor, el partner debe crear el DVD de recuperación del servidor.  
  
 **Reconstrucción completa**  
  
 Esta recuperación se supone que has configurado una copia de seguridad del servidor y que la copia de seguridad del servidor se completó correctamente al menos una vez antes del error del servidor. Restauración de reconstrucción completa (BMR) admite la recuperación de las unidades de sistema y de inicio solo desde una copia de seguridad de servidor anterior.  
  
 Después de un BMR, el servidor se devuelve al estado que existían en el momento de la copia de seguridad que se usa para la restauración. Esto suele suceder la copia de seguridad más reciente, pero en algunos casos, puede ser una copia de seguridad anterior. Recuperación de datos se realiza después de restaura el sistema mediante el restaurar archivos y carpetas asistente. BMR es el método de recuperación de servidor preferido porque se devuelven todas las opciones y configuración, mientras que un restablecimiento de fábrica devuelve el servidor a un estado de día 0.  
  
### <a name="remotely-administered-server-recovery"></a>Recuperación del servidor de administración remota  
 Esta sección describe las personalizaciones necesarias que debe realizar el partner y los medios finales que deben enviarse con cada servidor. Antes de profundizar en los detalles, veamos la experiencia del cliente.  
  
 En este escenario, el cliente "¢ s servidor deja de funcionar. Esto podría deberse a un virus, un error de disco duro o cualquier otra causa. El cliente inserta el DVD de recuperación de servidor en un equipo cliente que se encuentra en la misma red que el servidor. La aplicación de recuperación del servidor guiará tres pasos para recuperar su servidor:  
  
1.  Crea una unidad flash USB de arranque que se usa para reiniciar el servidor en modo de recuperación. La unidad flash USB debe ser de 8 GB o más grande.  
  
2.  Detecta el servidor que está en modo de recuperación.  
  
3.  Permite al cliente elegir un restablecimiento de fábrica o una restauración de reconstrucción completa y, a continuación, devuelve el servidor a un estado de funcionamiento.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Soporte técnico de los pasos para crear un DVD de recuperación del servidor para varios idiomas  
 Hay seis pasos principales para crear un DVD de recuperación del servidor  
  
1.  [(Opcional) Actualizar WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Recopilar los archivos XML y las imágenes de restablecimiento de fábrica](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Crear el DVD de recuperación del servidor](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [El Asistente para personalizar](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Crear el archivo ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Probar la recuperación de DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a>Paso 1: (Opcional) actualizar WinPE  
 El ADK incluye una copia de Windows PE personalizada. Cuando se arranca en esta imagen, se inicia automáticamente la señal que se usa la aplicación de la recuperación de cliente para conectarse a un servidor en modo de recuperación.  
  
 Windows PE debe personalizarse aún más agregando controladores de controladora de disco ni controladores específicos de hardware, como la red. Después de arrancar desde WinPE, los discos duros en el sistema necesita ser reconocible y la red debe estar trabajando.  
  
####  <a name="BKMK_Collecting"></a>Paso 2: Recopilar los archivos XML y las imágenes de restablecimiento de fábrica  
 Para restablecer un servidor de valores predeterminados de fábrica, necesitan capturar las siguientes dos imágenes:  
  
-   La imagen del sistema  
  
-   La partición del sistema reservado  
  
 Para capturar estas imágenes, se proporciona la herramienta GenDiskXML.exe. GenDiskXML.exe también recopila un archivo denominado disk.xml que se usa en el proceso de recuperación para volver a crear la configuración del disco.  
  
1.  Después de Sysprep, reiniciar el sistema mediante el uso de cualquier versión de 64 bits de Windows PE.  
  
2.  Para generar los archivos .xml y .wim a un origen externo, ejecutar `GenDiskXML /outputdir:<path>`como salida de los archivos .xml y .wim a cualquier fuente externa. Los archivos se agregan en el DVD en el siguiente paso.  
  
    > [!NOTE]
    >  El archivo .wim del sistema se dividirá para cumplir los requisitos de FAT32 de ningún archivo más de 4 GB. Durante el proceso, la capacidad del objetivo que se usa para capturar los archivos .wim requerida debe ser más de 8 GB para acomodar el proceso de división.  
  
####  <a name="BKMK_Creating"></a>Paso 3: Crear el DVD de recuperación del servidor  
 Cada servidor desde la fábrica debe estar acompañado de un DVD de recuperación del servidor. El DVD de herramientas ADK incluye los archivos necesarios para crear el DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Para crear el DVD de recuperación del servidor  
  
1.  Crea una carpeta de trabajo que se usará como la ubicación para almacenar la imagen ISO del final.  
  
2.  Desde el CD de partner, copia el contenido de la **recuperación del servidor** a la carpeta de trabajo que creaste en el paso 1.  
  
3.  Copia el archivo disk.xml que se creó cuando se ejecutó GenDiskXML.exe a la **restablecer** carpeta.  
  
4.  Copia los archivos de imagen que se crearon cuando se ejecutó **GenDiskXML.exe** a la **restablecer** carpeta. Los archivos son los archivos .wim y .swm, y puede variar el número de archivos.  
  
5.  Quitar GenDiskXML.exe de la carpeta. Se usa solo para fines de fábrica y no debe incluirse en el DVD que se envía al cliente.  
  
####  <a name="BKMK_Customizing"></a>Paso 4: Personalizar el Asistente  
 La aplicación de recuperación del servidor debe personalizarse con una imagen del dispositivo y el texto que describe cómo iniciar el dispositivo específico en modo de recuperación. Dado que esta página del Asistente de carpetas y restaurar archivos es específica de hardware, los pasos para iniciar el servidor en modo de recuperación variará.  
  
> [!NOTE]
>  Los nombres de archivo que se enumeran deben coincidir exactamente.  
  
1.  La página del asistente tiene un vínculo para obtener más ayuda. Si existe este archivo chm, reemplaza el elemento FWLink para Ayuda de la web. Se encuentra en el archivo de ayuda:  
  
     < DVD Root\ > \\$OEM$\Customization\\ < equipo\ referencia cultural > \RestartHelp.chm  
  
2.  Este archivo contiene el texto que el cliente ve en la página del asistente. El texto debe explicar cómo arrancar el servidor en modo de recuperación. El control es desplazable, que ubica un límite práctico a la cantidad de texto que se pueden agregar.  
  
     El siguiente archivo se usa para reemplazar la imagen de muestra en el asistente y se trata principalmente de personalización de marca. Debe ser un archivo .png. El tamaño del archivo debe ser 256 píxeles x 256 píxeles, o se recortarán cuando se muestra en el asistente.  
  
     < DVD Root\ > \\$OEM$\Customization\\ < equipo\ referencia cultural > \RestartInstructions.rtf  
  
3.  < DVD Root\ > \\$OEM$\Customization\\ < equipo\ referencia cultural > \ServerImage.png  
  
 Cuando vas a convertir la recuperación de servidor DVD para admitir varios idiomas, asegúrate de que hagas lo siguiente:  
  
1.  Siempre debe tener la en-us carpeta. Si la aplicación de recuperación del servidor no encuentra los archivos específicos de la referencia cultural que coincida con el equipo cliente se está ejecutando en, se retrocede a en-us.  
  
2.  En cada carpeta de referencia cultural que crees, agrega los archivos de personalización tres (.png, chm y .rtf).  
  
3.  Copia de las carpetas de la referencia cultural \Server idioma Packs\\ < CultureName\ > recuperación a la raíz de la recuperación del servidor DVD. Por ejemplo: las carpetas ES y ES-ES se copian a la raíz del DVD para admitir el español.  
  
4.  Finalizar el archivo ISO.  
  
 Nombres de referencia cultural admitidos incluyen:  

|-|-|  
|-Cs-CZ<br /><br /> -De-DE<br /><br /> -En-US<br /><br /> -Es-ES<br /><br /> -Fr-FR<br /><br /> -Hu-HU<br /><br /> -It-IT<br /><br /> -Ja-JP<br /><br /> -Ko-KR<br /><br /> -Nl-NL |-pl-PL<br /><br /> -Pt-BR<br /><br /> -Pt-PT<br /><br /> -Ru-RU<br /><br /> -Sv-SE<br /><br /> -Tr-TR<br /><br /> -Zh-CN<br /><br /> -Zh-HK<br /><br /> -Zh-TW
  
####  <a name="BKMK_CreatingISO"></a>Paso 5: Crear el archivo ISO  
 La carpeta que se creó y todo el contenido se puede grabar en un DVD. Este es el DVD que se proporcionan a los clientes con el servidor de nuevo.  
  
####  <a name="BKMK_Testing"></a>Paso 6: Probar el DVD de recuperación  
 Después de completar la instalación del servidor, configurar la copia de seguridad del servidor, ejecuta una copia de seguridad del servidor y, a continuación, probar la recuperación de DVD.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Para configurar y ejecutar una copia de seguridad del servidor  
  
1.  Abra el panel, haz clic en el **dispositivos** pestaña y, a continuación, haz clic en **Configurar copia de seguridad del servidor** en la **tareas** panel.  
  
2.  Sigue las instrucciones del Asistente para configurar una copia de seguridad de la copia de seguridad del servidor. Debes tener un disco duro externo para la copia de seguridad.  
  
3.  Haz clic en **iniciar una copia de seguridad del servidor** en la **tareas** panel y, a continuación, sigue las instrucciones del asistente.  
  
4.  Cuando termine la copia de seguridad, comprueba que se realizó correctamente.  
  
###### <a name="to-restore-a-server"></a>Para restaurar un servidor  
  
1.  Inserta la recuperación de DVD que creaste en un equipo cliente que está conectado a la misma red que el servidor a través de un concentrador o un conmutador.  
  
2.  Haz doble clic en **setup.exe**. El Asistente para la recuperación de servidor te pedirá que el mismo proceso que seguirá el cliente.  
  
3.  Haz clic en **restaurar el servidor de una copia de seguridad**y, a continuación, sigue las instrucciones del asistente.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)