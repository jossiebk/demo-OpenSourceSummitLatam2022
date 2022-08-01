# demo-OpenSourceSummitLatam2022
Ejemplo de despliegue de una arquitectura simple con Kubernetes.


### **Herramientas necesarias**
- [Google Cloud Platform](https://cloud.google.com/sdk/docs/install-sdk)
- [Kubernetes](https://kubernetes.io/es/)
  

## Configuración de entorno

### Instalación y configuracion de Google Cloud en entorno

1. Agregar el sdk de Google cloud como un recurso de paquetes dentro del sistema.
   
   ```
   echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
   ```

2. Importar la llave publica de Google Cloud dentro del entorno.
   
   ```
   curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
   ```

3. Actualizar la lista de paquetes e instalar Cloud SDK, tomar en cuenta que esta instalacion puede tardar varios minutos.
   
   ```
   sudo apt-get update && sudo apt-get install google-cloud-sdk
   ```
4. Inicializar el SDK, luego de algunos segundos pedirá iniciar sesión con una cuenta para lo cual se debe presionar "Y" y se abrira una ventana en el navegador para ingresar con la cuenta deseada.
   
   ```
   gcloud init
   ```
    -  Al inicializar se mostrara un grupo de opciones similar a la siguiente. 
  
    ![Paso4.1](imagenes/Paso4.1.png)

    - Se abrira una ventana en el navegador en caso aplique donde pedira seleccionar una cuenta con la cual continuar.
  
    ![Paso4.2](imagenes/Paso4.2.png)

     - Permitimos el acceso y se mostrara una ventana en la cual confirma que hemos podido acceder, para lo cual podemos regresar a la terminal.
  
    ![Paso4.3](imagenes/Paso4.3.png)

5. Al regresar a la terminal solicita seleccionar un proyecto existente en nuestra cuenta de Google Cloud, el cual se debe seleccionar ingresando su respectivo numero.
   
    ![Paso5](imagenes/Paso5.png)

6. Seleccionamos la opcion para configurar una region y zona por default para trabajar y presionar "Y".



### Implementación de cluster de Kubernetes

7.  Instalar el cliente de Kubernetes.
   
    ```
    sudo apt-get install kubectl
    ```

8. Verificar que la instalacion haya sido correcta.
   
    ```
    kubectl version --short
    ```

9. Crear un clúster para el despliegue de la arquitectura.

    ```
    gcloud container clusters create tesisdemo --num-nodes=2 --tags=allin,allout --enable-legacy-authorization --issue-client-certificate --machine-type=n1-standard-2 --no-enable-network-policy
    ```

    Banderas utilizadas:

    - create [nombre_del_cluster]
    - **--num-nodes=2**: Esta bandera sirve para definir la cantidad de nodos que tendrá el cluster, en este caso es generaron únicamente 2.
    - **--tags=allin,allout**: Se indican tags referentes a reglas de firewall con la apertura de puertos, en este caso se permitió todo el tráfico de entrada o salida (considerar que esto se realizo debido a que es un ambiente de ejemplo).
    - **--enable-legacy-authorization**: Esta política de kubernetes otorga permisos definidos a todos los usuarios del cluster.
    - **--machine-type=n1-standard-2**: Esta etiqueta sirve para definir el tipo de hardware que se utilizara, el tipo n1 standard 2 es de uso general el cual posee 2 nucleos, 7.50Gb de memoria RAM, un máximo de 128 discos persistentes, un almacenamiento local de 257TB en SSD y un ancho de banda de 10Gbps.

10. Al finalizar la creacion debemos obtener las credenciales para el manejo del clúster.

    ```
    gcloud container clusters get-credentials tesisdemo --zone us-central1-c --project tesis-demo
    ```
Los datos van a cambiar segun la zona en donde se conecten en Google Cloud, asi como el nombre del proyecto que creen, en mi caso utilice "tesis-demo".

### Clonacion de repositorio

11. Se puede clonar el repositorio en el cual se encuentran los archivos para desplegar la arquitectura de ejemplo y los experimentos definidos.

    ```
    git clone https://github.com/jossiebk/demo-OpenSourceSummitLatam2022
    ```


### Vista de contenedores Docker
Los contenedores utilizados para la definicion de la arquitectura en ExperimentosTesis/Arquitectura/Servidor/apache.yaml pueden verificarse en los siguientes enlaces:

> https://hub.docker.com/repository/docker/jossie/osslatam2022


## Desplegar Arquitectura

12.  Ingresar a la carpeta ExperimentosTesis/Arquitectura/Servidor/ y ejecutar el despliegue.
```
kubectl get namespaces
```    
```
kubectl get deployments
```    
```
kubectl get pods
```    
```
kubectl  get services
```    
```
kubectl describe deployment
```    
```
kubectl get nodes
```    
