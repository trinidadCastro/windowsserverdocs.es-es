---
title: "Recuperación de bosque de AD - realizar una recuperación completa del servidor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adfs
ms.openlocfilehash: 5de71cc005e9ec4c1425f9fa805b3c043ab4ea36
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Recuperación de bosque de AD - realizar una recuperación completa del servidor 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
Usa el siguiente procedimiento para realizar una recuperación completa del servidor de Windows Server 2016, 2012 R2 o 2012. 

## <a name="active-directory-full-server-recovery"></a>Recuperación completa del servidor de Active Directory
Una recuperación completa del servidor es necesaria si está restaurando en diferentes tipos de hardware o una instancia de otro sistema operativo. Ten en cuenta lo siguiente:

- El número de unidades de las necesidades de servidor de destino sea igual al número de la copia de seguridad y que necesitan para ser el mismo tamaño o mayor.
- El servidor de destino debe iniciarse desde el DVD del sistema operativo para tener acceso a la **reparar el equipo** opción. 
- Si el controlador de dominio se ejecuta en una máquina virtual en Hyper-V y la copia de seguridad de destino se almacena en una ubicación de red, debes instalar a un adaptador de red heredado.  
- Después de realizar una recuperación completa del servidor, necesitas por separado realizar una restauración de SYSVOL, como se describe en [recuperación de bosque de AD - realizando una sincronización autorizada de SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


Según el escenario, usa uno de los siguientes procedimientos para realizar una restauración completa.  
  
### <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Realizar una restauración completa del servidor con una copia de seguridad local con la imagen más reciente
  
1.  Iniciar la instalación de Windows, especificar el idioma, hora y moneda formato y opciones de teclado y haz clic en **siguiente**.  
2.  Haz clic en **reparar el equipo**.</br>
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3.  Haz clic en **solucionar**.</br>
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4.  Haz clic en **recuperación de imagen del sistema**.</br>
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5.  Haz clic en **Windows Server 2016**.  
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6.  Si va a restaurar la última copia de seguridad local, haz clic en **usar la imagen de sistema más reciente disponible (recomendada)** y haz clic en **siguiente**.
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7.  Ahora le permitirá una opción para:
    -  Formatear y volver a particionar discos
    -  Instalar controladores
    -  Cancelar la selección de la **avanzadas** características de reiniciar automáticamente y comprobación de errores de disco.  Estos están habilitados de manera predeterminada.
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Haz clic en **siguiente**.
9. Haz clic en **finalizar**.  Se te pedirá que pregunta si estás seguro de que desea continuar.  Haz clic en **Sí**.  
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Una vez se termine realizar una restauración de SYSVOL, como se describe en [recuperación de bosque de AD - realizando una sincronización autorizada de SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).
 

### <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Realizar una restauración completa del servidor con cualquier imagen local o remota
1.  Iniciar la instalación de Windows, especificar el idioma, hora y moneda formato y opciones de teclado y haz clic en **siguiente**.  
2.  Haz clic en **reparar el equipo**.</br>
3.  Haz clic en **solucionar**, haz clic en **recuperación de imagen del sistema**y haz clic en **Windows Server 2016**.  
4.  Si va a restaurar la última copia de seguridad local, haz clic en **selecciona una imagen del sistema** y haz clic en **siguiente**.

5.  Ahora puedes seleccionar la ubicación de la copia de seguridad que deseas restaurar.  Si la imagen es local puede seleccionar en la lista.  
6.  Si la imagen está en un recurso compartido de red, seleccione **avanzadas**.  También puedes seleccionar **avanzadas** si es necesario instalar un controlador.
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7.  Si está restaurando desde la red después de hacer clic **avanzadas** selecciona **busca una imagen del sistema en la red**.  Se te pedirá para restaurar la conectividad de red.  Haga clic en Aceptar. </br>
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Escribe la ruta de acceso UNC a la ubicación del recurso compartido de copia de seguridad (por ejemplo, \\\server1\backups) y haz clic en **Aceptar**.  También puedes escribir la dirección IP del servidor de destino, como \\\192.168.1.3\backups.  
![Restaurar el servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. Escribe las credenciales necesarias para acceder al recurso compartido y haz clic en Aceptar.  
11. Ahora **selecciona la fecha y hora de imagen del sistema para restaurar** y haz clic en **siguiente**.
12. Ahora le permitirá una opción para:
    1.   Formatear y volver a particionar discos
    2.   Instalar controladores
    3.   Cancelar la selección de la **avanzadas** características de reiniciar automáticamente y comprobación de errores de disco.  Estos están habilitados de manera predeterminada.
13. Haz clic en **siguiente**.
14. Haz clic en **finalizar**.  Se te pedirá que pregunta si estás seguro de que desea continuar.  Haz clic en **Sí**.   
15. Una vez se termine realizar una restauración de SYSVOL, como se describe en [recuperación de bosque de AD - realizando una sincronización autorizada de SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


### <a name="enabling-the-network-adapter-for-a-network-backup"></a>Habilitar al adaptador de red para una copia de seguridad de red
Si es necesario habilitar un adaptador de red desde el símbolo del sistema restaurar desde un recurso compartido de red, usa los siguientes pasos.

1.  Iniciar la instalación de Windows, especificar el idioma, hora y moneda formato y opciones de teclado y haz clic en **siguiente**.  
2.  Haz clic en **reparar el equipo**. Puedo
3.  Haz clic en **solucionar**, haz clic en **símbolo**.  
4.  Escribe el siguiente comando y presiona Intro:  
  
    ```  
    wpeinit  
    ```   
5.  Para confirmar el nombre del adaptador de red, escribe:  
  
    ```  
    show interfaces  
    ```  
  
     Escribe los comandos siguientes y presiona Intro después de cada comando:  
  
    ```  
    netsh  
    ```  
  
    ```  
    interface  
    ```  
  
    ```  
    tcp  
    ```  
  
    ```  
    ipv4  
    ```  
  
    ```  
    set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
    ```  
  
     Por ejemplo:  
  
    ```  
    set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
    ```  
  
     Tipo `quit`para volver a una ventana de símbolo del sistema. Tipo `ipconfig /all`para comprobar la red, adaptador tiene una dirección IP y vuelve a hacer ping a la dirección IP del servidor que hospeda el recurso compartido de copia de seguridad para confirmar la conectividad. Cuando hayas terminado, cierra la ventana de símbolo del sistema.  
  
6.  Ahora que el adaptador de red funciona, selecciona los pasos anteriores para completar la restauración.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
