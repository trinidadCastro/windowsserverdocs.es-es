---
title: Solución de problemas de clientes de ADBA
description: Te guía por un proceso de solución de problemas para un problema de activación de Windows
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: d56b2d002b0403971dc50fab639f77bddf1f8809
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959887"
---
# <a name="example-troubleshooting-active-directory-based-activation-adba-clients-that-do-not-activate"></a>Ejemplo: Solución de problemas de clientes de activación basada en Active Directory (ADBA) que no se activan

> [!NOTE]
> Este artículo se publicó originalmente como blog de TechNet el 26 de marzo de 2018.

¡Hola a todos! Mi nombre es Mike Kammer y he sido PFE de plataformas en Microsoft durante tan solo dos años. Recientemente, he ayudado a un cliente a implementar Windows Server 2016 en su entorno. Aprovechamos esta oportunidad para también migrar su metodología de activación desde un servidor de KMS a la [activación basada en Active Directory](/previous-versions/windows/hh852637(v=win.10)).

Como procedimiento adecuado para hacer todos los cambios, comenzamos la migración en el entorno de prueba del cliente. Para comenzar la implementación, seguimos las instrucciones de esta excelente publicación de blog de Charity Shelbourne, la activación basada en directorios [Active Directory-Based Activation vs. Key Management Services](https://techcommunity.microsoft.com/t5/Core-Infrastructure-and-Security/Active-Directory-Based-Activation-vs-Key-Management-Services/ba-p/256016) (Activación basada en Active Directory frente a Servicios de administración de claves). Los controladores de dominio del entorno de prueba se ejecutaban todos en Windows Server 2012 R2, por lo que no fue necesario preparar el bosque. Instalamos el rol en un controlador de dominio de Windows Server 2012 R2 y elegimos la activación basada en Active Directory como método de activación por volumen. Instalamos la clave de KMS y le asignamos el nombre "KMS AD Activation ( ** LAB)". Prácticamente seguimos la publicación de blog paso a paso.

Empezamos por crear cuatro máquinas virtuales, dos con Windows 2016 Standard y dos con Windows 2016 Datacenter. Hasta este punto, todo fue fantástico y todo el mundo estaba satisfecho. Creamos un servidor físico con Windows 2016 Standard, y la máquina se activó correctamente. Y allí es donde termina la historia.

¡Je je! ¡Era broma! Nada es tan fácil. A decir verdad, la instalación y configuración fueron muy fáciles, así que esa parte fue sencilla y directa. Volví a la oficina el lunes, y todas las máquinas virtuales que había creado la semana anterior mostraban que no se habían activado. ¡Vaya! ¡Eso no está bien! Volví a la máquina física y estaba bien. Fui con el cliente para conversar sobre lo sucedido. Por supuesto, la primera pregunta fue "¿Qué cambió durante el fin de semana?". Y, como de costumbre, la respuesta fue "Nada". Esta vez, era verdad que no cambiado nada, y tuvimos que averiguar lo que estaba ocurriendo.

Fui a uno de los servidores problemáticos, abrí un símbolo del sistema y comprobé la salida del comando **slmgr /ao-list**. El modificador **/ao-list** muestra todos los objetos de activación en Active Directory.

![Imagen que muestra el comando slmgr](./media/032618_1700_Troubleshoo1.png)

![Imagen que muestra los resultados del comando slmgr](./media/032618_1700_Troubleshoo2.png)

Los resultados muestran que tenemos dos objetos de activación: uno para Server 2012 R2 y el creado recientemente, KMS AD Activation ( ** LAB), que es nuestra licencia de Windows Server 2016. Esto confirma que Active Directory está configurado correctamente para activar clientes de KMS de Windows

Sabiendo que el comando **slmgr** es mi amigo para la activación de licencias, continué con distintas opciones. Probé el modificador **/dlv**, que mostrará información detallada de las licencias. Me parecía correcta, estaba ejecutando la versión Standard de Windows Server 2016, hay un identificador de activación, un identificador de instalación, una dirección URL de validación, incluso una clave de producto parcial.

![Imagen que muestra los resultados de slmgr /dlv](./media/ActivationTroubleshoot2b.jpg)

¿Alguien ve lo que me estaba perdiendo en este momento? Volveremos a ello después de ver mis otros pasos de solución de problemas, pero basta decir que la respuesta se encuentra en esta captura de pantalla.

Mi idea entonces es que, por algún motivo, la clave se ha dañado, así que uso el modificador **/upk**, que desinstala la clave actual. Si bien fue efectivo para eliminar la clave, por lo general no es la mejor manera de hacerlo. Si el servidor se reinicia antes de obtener una nueva clave, puede que quede en un estado incorrecto. Descubrí que usar el modificador **/ipk** (lo que hago más adelante en la solución del problema) sobrescribe la clave existente y es una ruta mucho más segura. ¡Aprende de mis errores!

![Imagen que muestra el comando slmgr /upk y su resultado](./media/032618_1700_Troubleshoo3.png)

Ejecuté de nuevo el modificador **/dlv** para ver la información detallada de la licencia. Desgraciadamente para mí, no me proporcionó información útil, solo un error de clave de producto no encontrada. Porque, por supuesto, no hay ninguna clave, puesto que acabo de desinstalarla.

![Imagen que muestra el comando slmgr /dlv y su resultado](./media/032618_1700_Troubleshoo4.png)

Me pareció que podía ser inútil, pero intenté el modificador **/ato**, que debería activar Windows en los servidores de KMS conocidos (o Active Directory según sea el caso). De nuevo, solo un error de producto no encontrado.

![Imagen que muestra el comando slmgr /ato y su resultado](./media/032618_1700_Troubleshoo5.png)

Mi siguiente idea fue que, a veces, basta con detener un servicio e iniciarlo para que funcione, así que fue lo siguiente que intenté. Tengo que detener e iniciar el Servicio de plataforma de protección de software de Microsoft (servicio SPPSvc). Desde un símbolo del sistema administrativo, uso los comandos de confianza **net stop** y **net start**. En primer lugar, observo que el servicio no se está ejecutando, así que creo que debe ser eso.

![Imagen que muestra los resultados de los comandos net stop y net start](./media/032618_1700_Troubleshoo6.png)

Pero no. Después de iniciar el servicio e intentar volver a activar Windows, aún obtengo el error de producto no encontrado.

Luego, examino el registro de eventos de aplicación en uno de los servidores con problemas. Encuentro un error relacionado con la activación de la licencia, el Id. de evento 8198, que tiene un código de 0x8007007B.

![Imagen que muestra los detalles del Id. de evento 8198](./media/032618_1700_Troubleshoo7.png)

Al buscar este código, encontré un artículo que dice que el código de error indica que la sintaxis del nombre de archivo, del nombre de directorio o de la etiqueta de volumen es incorrecta. Al leer los métodos descritos en el artículo, no parecía que ninguno de estos casos se ajustara a mi situación. Cuando ejecuté el comando **nslookup -type=all _vlmcs._tcp**, descubrí el servidor de KMS existente (todavía muchas máquinas con Windows 7 y Server 2008 en el entorno, por lo que era necesario conservarlo), pero también los cinco controladores de dominio. Esto indicaba que no se trataba de un problema de DNS y que mis problemas estaban en otra parte.

![Imagen que muestra los resultados del comando nslookup](./media/032618_1700_Troubleshoo8.png)

Así que sé que el DNS está bien. Active Directory está configurado correctamente como origen de activación de KMS. El servidor físico se ha activado correctamente. ¿Podría tratarse de un problema solo con las VM? Como nota colateral interesante en este momento, mi cliente me informa que alguien de otro departamento ha decidido crear también más de una decena de máquinas virtuales de Windows Server 2016. Así que ahora supongo que tengo que lidiar con otra decena de servidores que no se van a activar. Pero no. Esos servidores se activaron sin problemas.

Bueno, volví al comando **slmgr** para descubrir cómo se activan estos monstruos. Esta vez voy a usar el modificador **/ipk**, que me permitirá instalar una clave de producto. Fui a [este sitio](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612867(v=ws.11)) para obtener las claves adecuadas para la versión Standard de Windows Server 2016. Algunos de los servidores son Datacenter, pero necesito corregir este primero.

![Imagen que muestra la lista de claves de configuración de cliente KMS](./media/032618_1700_Troubleshoo9.png)

Usé el modificador **/ipk** para instalar una clave de producto y elegí la clave de Windows Server 2016 Standard.

![Imagen que muestra el comando slmgr /ipk y su resultado](./media/032618_1700_Troubleshoo10.png)

De aquí en más, solo capturé los resultados de las experiencias de Datacenter, pero eran los mismos. Usé el modificador **/ato** para forzar la activación. Obtenemos el mensaje maravilla de que el producto se ha activado correctamente.

![Imagen que muestra el comando slmgr /ato y su resultado](./media/032618_1700_Troubleshoo11.png)

Otra vez con el modificador **/dlv**, podemos ver que ahora Active Directory nos ha activado.

![Imagen que muestra el comando slmgr /dlv y su resultado](./media/032618_1700_Troubleshoo12.png)

Ahora, ¿qué ha ido mal? ¿Por qué tengo que quitar la clave instalada y agregar esas claves genéricas para que estas máquinas se activen correctamente? ¿Por qué la otra decena de máquinas se activó sin problemas? Como dije anteriormente, me perdí de algo fundamental en las etapas iniciales del análisis del problema. Estaba totalmente confundido, así que me puse en contacto con Charity de la publicación de blog inicial para ver si podía ayudarme. Vio el problema de inmediato y me ayudó a comprender lo que me faltó en un principio.

Cuando ejecuté el primer modificador **/dlv**, la clave estaba en la descripción. La descripción era Windows® Operating System, RETAIL Channel. Había visto eso y pensé que RETAIL Channel significaba que se había comprado y era una clave válida.

![Imagen que muestra los resultados de slmgr /dlv, con la información de canal resaltada](./media/032618_1700_Troubleshoo13.png)

Al examinar la salida del modificador **/dlv** de un servidor activado correctamente, debemos observar que la descripción indica VOLUME_KMSCLIENT channel. Esto nos permite saber que es realmente una licencia por volumen.

![Imagen que muestra los resultados de slmgr /dlv, con la información de canal resaltada](./media/032618_1700_Troubleshoo14.png)

Entonces, ¿qué significa ese RETAIL Channel? Bueno, significa que el medio que se usó para instalar el sistema operativo era una ISO de MSDN. Volví con el cliente y le pregunté si, por alguna oportunidad, había una segunda ISO de Windows Server 2016 flotante en la red. Resultó que sí, había otra imagen ISO en la red y se había usado para crear la otra decena de máquinas. Compararon las dos ISO y, lo más probable es que la que me dieron para crear los servidores virtuales fuera, de hecho, una ISO de MSDN. Quitaron esa ISO de MSDN de la red y ahora tenemos todos los servidores existentes activados y no más preocupaciones sobre el error de activación en futuras compilaciones.

Espero que esto te haya resultado útil y puede ahorrarte tiempo en adelante.

Mike
