---
title: Crear un DVD de recuperación del servidor para servidores administrados de forma remota
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a9b571c2d3e5d531d8c923741500c72675022adc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312190"
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Crear un DVD de recuperación del servidor para servidores administrados de forma remota

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a><a name="BKMK_HeadlessRecovery"></a>Crear un DVD de recuperación del servidor para servidores administrados de forma remota  
 Hay dos modelos de restablecimiento de fábrica y recuperación del servidor, y se usa uno u otro en función del hardware que el cliente haya recibido.  
  
 **Servidor administrado de forma remota**  
  
 Esta opción de recuperación del servidor requiere que el proceso se ejecute desde un equipo cliente que se encuentre en la misma red. El socio debe autorizar el DVD de recuperación del servidor, ya que para poder restablecer la configuración predeterminada de fábrica es necesario enviar una imagen específica para el hardware con el servidor.  
  
 **Servidor administrado localmente**  
  
 Éste es el modelo clásico, en el que el servidor se administra en la consola de servidor. El soporte de instalación del servidor se utiliza para ejecutar una recuperación. Esto requiere que el servidor incluya la capacidad de ver salida de vídeo, además de incluir un lector de DVD. El cliente arranca desde el soporte de instalación del servidor y, a continuación, elige el método de recuperación adecuado. No es necesario crear n DVD de recuperación de los servidores que se administren localmente.  
  
> [!NOTE]
>  En el caso del modelo de servidor administrado de forma local, el cliente no puede realizar el restablecimiento de fábrica mediante una nueva instalación. No obstante, se perderán las personalizaciones de socio.  
  
 Hay dos tipos de recuperación del servidor.  
  
 **Restablecimiento de fábrica**  
  
 Esta recuperación devuelve el servidor al estado original en el que estaba cuando se envió de fábrica. Después de un restablecimiento de fábrica, se le solicitará que realice la configuración inicial del servidor como si fuera la primera vez que lo enciende. Todas las configuraciones y personalizaciones existentes se perderán. Esto también se conoce como Day 0. El socio debe autorizar el DVD de recuperación del servidor, ya que para poder restablecer la configuración predeterminada de fábrica es necesario enviar una imagen específica para el hardware con el servidor.  
  
 **Restauración sin sistema operativo**  
  
 En este caso, se asume que se configuró una copia de seguridad del servidor y que ésta se realizó correctamente al menos una vez antes del error del servidor. La reconstrucción completa (BMR) sólo admite la recuperación del sistema y las unidades de arranque a partir de una copia de seguridad anterior del servidor.  
  
 Tras una reconstrucción completa, el servidor vuelve al estado en el que se encontraba cuando se realizó la copia de seguridad utilizada para la restauración. Normalmente, se trata de la copia de seguridad más reciente, pero en algunos casos puede ser anterior. La recuperación de datos se realiza después de restaurar el sistema mediante el Asistente para la restauración de archivos y carpetas. La reconstrucción completa es el método preferente porque restaura todas las configuraciones, mientras que con el restablecimiento de fábrica el servidor vuelve a un estado de día 0.  
  
### <a name="remotely-administered-server-recovery"></a>Recuperación de servidor administrado de forma remota  
 En esta sección se describen las personalizaciones que el socio debe realizar y los medios finales que se deben enviar con cada servidor. Antes de profundizar en los detalles, echemos un vistazo a la experiencia del cliente.  
  
 En este escenario, el cliente "¢ s Server ya no funciona. Esto podría deberse a un virus, un error del disco duro o alguna otra causa. El cliente inserta el DVD de recuperación del servidor en un equipo cliente que se encuentra en la misma red que el servidor. La aplicación de recuperación del servidor guía al cliente a través de tres pasos para restaurar el servidor:  
  
1.  Crea una unidad flash USB de arranque para reiniciar el servidor en modo de recuperación. Dicha unidad flash USB debe tener 8 GB, o más.  
  
2.  Detecta el servidor que está ahora en el modo de recuperación.  
  
3.  Permite que el cliente elija entre un restablecimiento de fábrica o una reconstrucción completa y, a continuación, devuelve el servidor a estado operativo.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Pasos para crear un DVD de recuperación de servidor para ofrecer compatibilidad con varios idiomas  
 Hay seis pasos principales para crear un DVD de recuperación de servidor  
  
1.  [Opta Actualizar WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Recopilar las imágenes de restablecimiento de fábrica y los archivos XML](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Crear el DVD de recuperación del servidor](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personalizar el asistente](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Crear el archivo ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Probar el DVD de recuperación](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="step-1-optional-update-winpe"></a><a name="BKMK_Updating"></a>Paso 1: (opcional) actualizar WinPE  
 El Kit de evaluación e implementación (ADK) incluye una copia personalizada de Windows PE. Al arrancar esta imagen, se inicia automáticamente la señalización que la aplicación de recuperación del cliente usa para conectarse a un servidor en modo de recuperación.  
  
 Windows PE debe personalizarse aún más mediante la incorporación de controladores específicos de hardware (por ejemplo, controladores de la red o de la controladora de disco). Después de arrancar desde WinPE, los discos duros del sistema deben ser reconocibles y la red debe funcionar.  
  
####  <a name="step-2-collect-the-factory-reset-images-and-xml-files"></a><a name="BKMK_Collecting"></a>Paso 2: recopilar las imágenes de restablecimiento de fábrica y los archivos XML  
 Para restablecer un servidor a sus valores predeterminados de fábrica, es necesario capturar las dos imágenes siguientes:  
  
- La imagen de la unidad del sistema  
  
- La partición reservada del sistema  
  
  Para capturar estas imágenes, se proporciona la herramienta GenDiskXML.exe. GenDiskXML.exe también recopila un archivo llamado disk.xml, que el proceso de recuperación utiliza para volver a crear la configuración del disco.  
  
1.  Después de Sysprep, reinicie el sistema con cualquier versión de 64 bits de Windows PE.  
  
2.  Para volcar los archivos .xml y .wim a un origen externo, ejecute `GenDiskXML /outputdir:<path>` . Los archivos se agregarán al DVD en el paso siguiente.  
  
    > [!NOTE]
    >  El archivo .wim del sistema se dividirá para que el tamaño supere 4 GB, con el fin de cumplir el requisito de FAT32. Durante el proceso, la capacidad requerida del destino utilizado para capturar los archivos .wim debe ser superior a 8 GB para permitir el proceso de división.  
  
####  <a name="step-3-create-the-server-recovery-dvd"></a><a name="BKMK_Creating"></a>Paso 3: crear el DVD de recuperación del servidor  
 Cada uno de los servidores enviados de fábrica debe ir acompañado de un DVD de recuperación del servidor. El DVD con herramientas del Kit de evaluación e implementación (ADK) incluye los archivos necesarios para crear el DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Para crear un DVD de recuperación del servidor:  
  
1.  Cree una carpeta de trabajo que sirva como ubicación para almacenar el ISO final.  
  
2.  Copie el contenido de la carpeta **Server recovery** desde el CD de socio a la carpeta de trabajo que creó en el Paso 1.  
  
3.  Copie el archivo disk.xml que se creó al ejecutar GenDiskXML.exe en la carpeta **Restablecer**.  
  
4.  Copie los archivos de imagen que se crearon al ejecutar **GenDiskXML.exe** en la carpeta **Restablecer**. Los archivos son del tipo .wim y .swm, y el número puede variar.  
  
5.  Quite GenDiskXML.exe de la carpeta. Sólo se usa en la fábrica y no debe incluirse en el DVD que se envía al cliente.  
  
####  <a name="step-4-customize-the-wizard"></a><a name="BKMK_Customizing"></a>Paso 4: personalizar el asistente  
 La aplicación de recuperación del servidor debe personalizarse con una imagen del dispositivo y con texto que describa cómo iniciar el dispositivo en modo de recuperación. Dado que esta página del Asistente para la restauración de archivos y carpetas es específica del hardware, los pasos para iniciar el servidor en modo de recuperación variarán.  
  
> [!NOTE]
>  Los nombres de archivo que se enumeran deben coincidir exactamente.  
  
1. La página del asistente incluye un vínculo de ayuda adicional. Si el archivo .chm existe, invalidará el vínculo redireccionable para la Ayuda web. El archivo de Ayuda se encuentra en:  
  
    <\>raíz de DVD \\$OEM $ \Customization\\< nombre de referencia cultural\>\RestartHelp.chm  
  
2. Este archivo contiene el texto que el cliente ve en la página del asistente. Dicho texto debe explicar cómo arrancar el servidor en modo de recuperación. El control se puede desplazar, lo que establece un límite práctico con respecto a la cantidad de texto que se puede agregar.  
  
    El siguiente archivo se usa para sustituir la imagen de ejemplo del asistente y tiene que ver principalmente con la personalización de marca. Debe tratarse de un archivo .png. El tamaño de dicho archivo debe ser de 256 x 256 píxeles o aparecerá recortado cuando el asistente lo muestre.  
  
    <\>raíz de DVD \\$OEM $ \Customization\\< nombre de referencia cultural\>\RestartInstructions.rtf  
  
3. <\>raíz de DVD \\$OEM $ \Customization\\< nombre de referencia cultural\>\ServerImage.png  
  
   Cuando vaya a convertir el DVD de recuperación del servidor para que admita varios idiomas, asegúrese de que:  
  
4. Siempre debe tener la carpeta en-us. Si la aplicación de recuperación del servidor no encuentra los archivos de referencias culturales específicas que coinciden con el equipo cliente en que se ejecuta, revertirá a en-us.  
  
5. En cada carpeta de referencias culturales que cree, agregue los tres archivos de personalización (.png, .chm y .rtf).  
  
6. Copie las dos carpetas de referencia cultural de los paquetes de idioma\\< CultureName\>recuperación de \Server en la raíz del DVD de recuperación del servidor. Por ejemplo, para que se pueda usar el español, se deben copiar las carpetas ES y ES-ES al directorio raíz del DVD.  
  
7. Finalice el archivo ISO.  
  
   Entre los nombres de referencias culturales admitidos, se incluyen:  

|-|-|  
|-CS-CZ<br /><br /> -de-de<br /><br /> -en-US<br /><br /> -es-ES<br /><br /> -fr-FR<br /><br /> -Hu-HU<br /><br /> -it-IT<br /><br /> -ja-JP<br /><br /> -ko-KR<br /><br /> -nl-NL |-PL-PL<br /><br /> -pt-BR<br /><br /> -pt-PT<br /><br /> -ru-RU<br /><br /> -SV-SE<br /><br /> -TR-TR<br /><br /> -zh-CN<br /><br /> -ZH-HK<br /><br /> -zh-TW
  
####  <a name="step-5-create-the-iso-file"></a><a name="BKMK_CreatingISO"></a>Paso 5: creación del archivo ISO  
 Tanto la carpeta que se creó como todo el contenido de la misma se pueden grabar en un DVD. Este es el DVD que se suministrará a los clientes con el nuevo servidor.  
  
####  <a name="step-6-test-the-recovery-dvd"></a><a name="BKMK_Testing"></a>Paso 6: probar el DVD de recuperación  
 Tras finalizar la instalación del servidor, configure la copia de seguridad del servidor, ejecute una copia de seguridad del mismo y, a continuación, pruebe el DVD de recuperación.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Para configurar y ejecutar una copia de seguridad del servidor  
  
1.  Abra el Panel, haga clic en la ficha **Dispositivos** y, a continuación, haga clic en **Configurar copias de seguridad de este servidor** en el panel **Tareas**.  
  
2.  Siga las instrucciones del asistente para configurar una copia de seguridad del servidor. Necesita un disco duro externo para la copia de seguridad.  
  
3.  Haga clic en **Iniciar una copia de seguridad del servidor** en el panel **Tareas** y siga las instrucciones del asistente.  
  
4.  Cuando la copia de seguridad finalice, compruebe que se ha realizado correctamente.  
  
###### <a name="to-restore-a-server"></a>Para restaurar un servidor  
  
1.  Inserte el DVD de recuperación que creó en un equipo cliente que esté conectado a la misma red que el servidor a través de un concentrador o un conmutador.  
  
2.  Haga doble clic en **setup.exe**. El Asistente para la recuperación del servidor le guiará por el mismo proceso que seguirá el cliente.  
  
3.  Haga clic en **Restaurar el servidor desde una copia de seguridad** y siga las instrucciones del asistente.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)