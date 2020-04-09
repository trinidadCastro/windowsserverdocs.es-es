---
title: Velocidad de transferencia lenta de archivos SMB
description: Presenta cómo solucionar problemas de rendimiento de transferencia de archivos SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: af05daa164b5b2c5eca73eff51d97d4c25ba1ca3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815398"
---
# <a name="slow-smb-files-transfer-speed"></a>Velocidad de transferencia lenta de archivos SMB

En este artículo se proporcionan procedimientos de solución de problemas recomendados para la velocidad de transferencia de archivos lenta a través de SMB.

## <a name="large-file-transfer-is-slow"></a>La transferencia de archivos grandes es lenta

Si observa transferencias lentas de archivos grandes, tenga en cuenta los siguientes pasos:

- Pruebe el comando de copia de archivos para la e/s no almacenada en búfer (**xcopy/j** o **Robocopy/j**).

- Pruebe la velocidad de almacenamiento. Esto se debe a que las velocidades de copia de archivos están limitadas por la velocidad de almacenamiento.

- Las copias de archivos a veces se inician rápidamente y después se ralentizan. Siga estas instrucciones para comprobar esta situación:
    
  - Esto suele ocurrir cuando la copia inicial se almacena en memoria caché o se almacena en búfer (ya sea en memoria o en la memoria caché de la controladora RAID) y se agota la memoria caché. Esto obliga a que los datos se escriban directamente en el disco (escritura a través). Se trata de un proceso más lento.
    
  - Utilice los contadores del monitor de rendimiento de almacenamiento para determinar si el rendimiento del almacenamiento se degrada a lo largo del tiempo. Para obtener más información, vea [optimizar el rendimiento de los servidores de archivos SMB](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/file-server/smb-file-server).

- Use RAMMap (SysInternals) para determinar si el uso de "archivo asignado" en la memoria deja de crecer debido a un agotamiento de la memoria libre.

- Busque pérdida de paquetes en el seguimiento. Esto puede producir una limitación por parte del proveedor de congestión de TCP.

- En el caso de SMBv3 y versiones posteriores, asegúrese de que SMB multicanal está habilitado y en funcionamiento.

- En el cliente SMB, habilite la MTU grande en SMB y deshabilite la limitación de ancho de banda. Para ello, ejecute el comando siguiente:  
  
  ```PowerShell
  Set-SmbClientConfiguration -EnableBandwidthThrottling 0 -EnableLargeMtu 1
  ```

## <a name="small-file-transfer-is-slow"></a>La transferencia de archivos pequeña es lenta

La transferencia lenta de archivos pequeños a través de SMB suele producirse si hay muchos archivos. Este es un comportamiento esperado.

Durante la transferencia de archivos, la creación de archivos provoca una sobrecarga de protocolo alta y una gran sobrecarga del sistema de archivos. En el caso de las transferencias de archivos de gran tamaño, estos costos solo se producen una vez. Cuando se transfiere un gran número de archivos pequeños, el costo es repetitivo y produce transferencias lentas.

A continuación se muestran los detalles técnicos sobre este problema:

- SMB llama a un comando CREATE para solicitar que se cree el archivo. Algunos códigos comprobarán si el archivo existe y, a continuación, creará el archivo. O alguna variación del comando CREATE crea el archivo real.

- Cada comando CREATE genera actividad en el sistema de archivos.

- Una vez que se escriben los datos, el archivo se cierra.

- Todo el tiempo, el proceso se ve afectado por la latencia de red y la latencia del servidor SMB. Esto se debe a que la solicitud SMB se traduce primero a un comando del sistema de archivos y, a continuación, a la latencia real del sistema de archivos para completar la operación.

- Si se está ejecutando un programa antivirus, la transferencia ralentizará aún más. Esto se debe a que los datos normalmente se examinan una vez por el rastreador de paquetes y una segunda vez cuando se escriben en el disco. En algunos escenarios, estas acciones se repiten miles de veces. Podría observar velocidades de menos de 1 MB/s.

## <a name="opening-office-documents-is-slow"></a>La apertura de documentos de Office es lenta

Este problema se produce normalmente en una conexión WAN. Esto es habitual y normalmente se debe a la manera en que las aplicaciones de Office (Microsoft Excel, en particular) tienen acceso y leen los datos.

Se recomienda asegurarse de que los archivos binarios de la oficina y el SMB estén actualizados y, a continuación, realizar pruebas con la concesión deshabilitada en el servidor SMB. Para ello, realice los pasos siguientes:
   
1. Ejecute el siguiente comando de PowerShell en Windows 8 y Windows Server 2012 o versiones posteriores de Windows:
      
   ```PowerShell
   Set-SmbServerConfiguration -EnableLeasing $false  
   ```
      
   O bien, ejecute el siguiente comando en una ventana de símbolo del sistema con privilegios elevados:  

   ```cmd
   REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v DisableLeasing /t REG\_DWORD /d 1 /f  
   ```
      
   > [!NOTE]
   > Después de establecer esta clave del registro, ya no se conceden concesiones de SMB2, pero bloqueos oportunistas sigue estando disponibles. Esta configuración se usa principalmente para la solución de problemas.
    
2. Reinicie el servidor de archivos o reinicie el servicio de **servidor** . Para reiniciar el servicio, ejecute los siguientes comandos:

   ```cmd  
   NET STOP SERVER 
   NET START SERVER
   ```

Para evitar este problema, también puede replicar el archivo en un servidor de archivos local. Para obtener más información, consulte [uardar Office Documents to a Network Server is Slowly when using EFS](https://docs.microsoft.com/office/troubleshoot/office/saving-file-to-network-server-slow).
