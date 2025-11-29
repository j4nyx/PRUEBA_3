# PRUEBA_3 / ET
Elías Delgado / Javiera Aguirre 
19.559.726-k / 20.983.491-k
prueba 1: https://github.com/j4nyx/Tu_primer_pipeline_de_despliegue
prueba 2: https://github.com/j4nyx/PRUEBA_2_DEVOPS



AWS EC2
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Creación y Configuración de la Instancia EC2

Esta primera parte consiste en el despliegue de nuestro servidor virtual.
Lanzamiento de la Instancia: Se creó una instancia de Amazon EC2 llamada "Práctica de monitoreo".
AMI (Imagen de Máquina de Amazon): Se seleccionó una Amazon Linux 2023 AMI.
Tipo de Instancia: Es un servidor pequeño, t2.micro, elegido probablemente por ser elegible para el nivel gratuito de AWS (Free tier eligible).
Almacenamiento: Se configuró un volumen raíz de 8 GiB de tipo gp3.
Perfil de Instancia IAM (esta parte es necesaria para seguir avanzando): En los detalles avanzados, se prefirió un perfil de instancia IAM llamado LabInstanceProfile. Esto es fundamental, ya que este perfil es el que otorga los 
permisos necesarios a la instancia para que pueda, por ejemplo, enviar datos y comunicarse con otros servicios de AWS, como CloudWatch y SSM, que usaremos más adelante.

Conectividad y Seguridad:
Se le asignó un par de claves (Key pair) llamado 22 para el acceso seguro (SSH).
Se creó un nuevo grupo de seguridad llamado launch-wizard-2.
Se habilitó el acceso SSH desde cualquier dirección IP (0.0.0.0/0).
Se habilitó el auto-asignación de una IP pública (54.163.193.98).

Debemos fijarnos si el estado de la instancia fue lanzada y está en estado Running, si en el caso no diga que está corriendo la instancia hay que repetir el paso nuevamente, hasta que quede andando.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Configuración del Agente CloudWatch
Para monitorear el uso interno de la memoria y el sistema operativo, se necesita un agente especial.
Validación del Agente SSM: Primero, se verificó que el agente SSM (Systems Manager) estuviera Online para poder gestionar la instancia de forma remota.
Validación del Agente CloudWatch: El sistema intentó validar el agente CloudWatch. Inicialmente, el estado fue "Did not respond", esto pasa porque la cuenta estudiantil no tiene todos los permisos pero igual se puede llevar a cabo la instalación exitosa.
Selección de Métricas: Se configuró el agente para que recoja métricas detalladas:
CPU: Se seleccionó Usage active y Usage system.
Memoria: Se seleccionó Used percent, Total, y Active.
Estado Final: La configuración del agente CloudWatch se envió exitosamente a la instancia.

Configuración del Servicio de Notificación (Amazon SNS)
Para recibir alertas por correo, se configuró el sistema de notificación.
Creación del Tópico SNS: Se creó un Tópico Standard de Amazon Simple Notification Service (SNS) llamado NotificacionPorCorreo.
Creación y Confirmación de Suscripción:
Se creó una suscripción para el tópico, usando el protocolo Email y la dirección javi.aguirre@duocuc.cl como Endpoint.
El sistema envió un correo de confirmación (que terminó en la carpeta de Spam).
Se confirmó la suscripción mediante el enlace del correo, y el estado de la suscripción cambió a Confirmed.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Creación de la Alarma de Monitoreo (CloudWatch Alarm)

Finalmente, se enlaza el monitoreo con la notificación para crear la alerta.
Creación de la Alarma: Se optó por Create an alarm para la instancia i-0d8e8b48d04371e4f.(práctica de monitoreo) 
Configuración de la Notificación: Se asoció la alarma al tópico SNS que creamos: “NotificacionPorCorreo”.
Definición de Umbrales: Se especificaron las condiciones para que la alarma se active:
Métrica: CPU utilization.
Agrupación de Muestras: Maximum.
Condición (Umbral): Cuando el porcentaje de utilización de la CPU sea mayor o igual a (> =) 80%.
Período: Durante 1 período consecutivo de 5 minutos.
Nombre de la Alarma: Se nombró la alarma como awsec2-i-0d8e8b48d04371e4f-GreaterThanOrEqualToThreshold-CPUUtilization.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Resultado Final

Al final del proceso, la alarma fue creada exitosamente, y ahora el sistema está listo para enviar una alerta por correo electrónico si el uso de la CPU del servidor Práctica de monitoreo se dispara por encima del 80% durante al menos 5 minutos.
Este es un ejemplo completo de cómo usar la infraestructura de monitoreo y notificación de AWS para asegurar la disponibilidad y rendimiento de un servidor.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



