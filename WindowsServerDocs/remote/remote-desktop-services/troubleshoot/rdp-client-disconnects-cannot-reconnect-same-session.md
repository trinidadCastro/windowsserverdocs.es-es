---
title: El cliente de Escritorio remoto se desconecta y no se puede volver a conectar a la misma sesión
description: Solución de un problema por el que el cliente de Escritorio remoto se desconecta y no se puede volver a conectar a la misma sesión.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0d116c99b7c8b1daffc4ec58bd93414781eea321
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857208"
---
# <a name="remote-desktop-client-disconnects-and-cant-reconnect-to-the-same-session"></a>El cliente de Escritorio remoto se desconecta y no se puede volver a conectar a la misma sesión

Después de que el cliente de Escritorio remoto pierda su conexión con Escritorio remoto, el cliente no se puede volver a conectar de inmediato. El usuario recibe uno de los mensajes de error siguientes:

  - El cliente no se ha podido conectar al servidor de Terminal Server debido a un error de seguridad. Asegúrate de que has iniciado sesión en la red y, después, intenta conectarte de nuevo.
  - Escritorio remoto desconectado. Debido a un error de seguridad, el cliente no se pudo conectar al equipo remoto. Comprueba que has iniciado sesión en la red y vuelve a intentar conectarse.

Cuando el cliente de Escritorio remoto se vuelve a conectar, el servidor RDSH vuelve a conectar el cliente a una sesión nueva en lugar de a la sesión original. Pero cuando se comprueba el servidor RDSH, indica que la sesión original sigue activa y no ha entrado en un estado desconectado.

Para solucionar este problema, puede habilitar la en la carpeta de directivas de grupo **Configurar intervalo entre mensajes de mantenimiento de conexión**  en **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto\\Conexiones**. Si habilitas esta directiva, es preciso especificar un intervalo entre mensajes de mantenimiento. Dicho intervalo determina la frecuencia, en minutos,con que el servidor comprueba el estado de sesión.

Este problema también se puede corregir si se vuelven a configurar las opciones de autenticación y configuración. Puedes volver a configurar estas opciones en el nivel de servidor o mediante objetos de directiva de grupo (GPO). Aquí se muestra cómo volver a configurar las opciones: carpeta de directivas de grupo **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto\\Seguridad**.

1. En el servidor Host de sesión de Escritorio remoto, abre **Configuración del host de sesión de Escritorio remoto**.
2. En **Conexiones**, haz clic con el botón derecho en el nombre de la conexión y, después, selecciona **Propiedades**.
3. En el cuadro de diálogo **Propiedades** de la conexión, en la pestaña **General**, en la capa de **Seguridad**, seleccione un método de seguridad.
4. Vete a **Nivel de cifrado** y selecciona el nivel que quieras. Puede seleccionar **Bajo**, **Compatible con el cliente** , **Alto** o **Compatible con FIPS**.

> [!NOTE]  
>  - Cuando las comunicaciones entre los clientes y los servidores de Host de sesión de Escritorio remoto requieran el máximo nivel de cifrado, usa el cifrado compatible con FIPS.
>  - Todos los valores de nivel de cifrado que configures en la directiva de grupo anulan los que establezcas mediante la herramienta de configuración de Servicios de Escritorio remoto. Además, si habilita la directiva[Criptografía de sistema: usar algoritmos que cumplan FIPS para el cifrado, la firma y las operaciones hash ](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), esta configuración anula la directiva**Establecer el nivel de cifrado de conexión de cliente** . La directiva de criptografía del sistema está en la carpeta **Configuración del equipo\\Configuración de Windows\\Configuración de seguridad\\Directivas locales\\Opciones de seguridad**.
>  - Al cambiar el nivel de cifrado, el nuevo nivel de cifrado surte efecto la próxima vez que un usuario inicie sesión. Si requiere varios niveles de cifrado en un servidor, instale varios adaptadores de red y configure cada uno de ellos por separado.
>  - Para comprobar que el certificado tiene una clave privada correspondiente, vete a Configuración de Servicios de Escritorio remoto, haz clic con el botón derecho en la conexión para la que quieras ver el certificado, selecciona **General** y después **Editar**. Después de eso, selecciona **Ver certificado**. En la pestaña **General**, si hay una clave deberías ver el mensaje "Tiene una clave privada correspondiente a este certificado". También puedes ver esta información mediante el complemento Certificados.
>  - El cifrado compatible con FIPS (la directiva **Criptografía de sistema: usar algoritmos que cumplan la norma FIPS para cifrado, aplicación de algoritmo hash y operaciones hash** o el valor **Compatible con FIPS** de la configuración de Servidor de Escritorio remoto) cifra y descifra los datos enviados entre el servidor y el cliente con los algoritmos de cifrado del Estándar federal de procesamiento de información (FIPS) 140-1 que usan módulos criptográficos de Microsoft. Para más información, consulte [Validación FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - El valor **Alto** cifra los datos enviados entre el servidor y el cliente mediante cifrado de 128 bits de alta seguridad.
>  - El valor **Compatible con el cliente** cifra los datos enviados entre el cliente y el servidor en el nivel máximo de seguridad de la clave que admita el cliente.
>  - El valor **Baja** cifra los datos enviados desde el cliente al servidor mediante cifrado de 56 bits.
