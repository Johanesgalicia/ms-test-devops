# Microservicio - ms-invertirffmm-neg

Microservicio que ejecuta el invertir de personas en fondos mutuos

1. [Ownership](/readme.md#1-ownership)
2. [Operaciones](/readme.md#2-operaciones)
3. [Especificación Técnica](/readme.md#3-especificaci%C3%B3n-t%C3%A9cnica)
4. [Dependencias](/readme.md#4-ejecuci%C3%B3n)
5. [Eventos](/readme.md#5-eventos)
6. [Ciclo de Vida](/readme.md#6-ciclo-de-vida)
7. [Change Log](/readme.md#7-change-log)

## 1. Ownership
Esta sección detalla quién es el responsable del ciclo de vida del microservicio.

### 1.1. Business Owner
Unidad de negocio responsable y dueña del microservicio.
  - Unidad de Negocio: Inversiones
  - Responsable:
  - Contacto:

### 1.2. Technical Owner
Unidad técnica referente del microservicio.
  - Unidad Técnica: Unidad de APIs y Microservicios
  - Responsable:
  - Contacto:

## 2. Operaciones
Detalla las operaciones que se pueden explotar a través del microservicio.

| Operación                             | Descripción Capacidad                                                                                             |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------------| 
| POST /invertir                        | Operacion que permite invertir en un fondo mutuo                                                                  |
| GET /obtenerCuentasFFMM/{codigoFondo} | Operacion que trae las cuentas de fondos mutuos asociadas a la persona                                            |								    |
| POST /apv/inversion                   | Operacion que permite invertir en fondos de Ahorro Previsional Voluntario APV                                     |
| GET /apv/precarga                     | Operacion que permite obtener los datos necesarios para precargar el formulario de inversion en APV               |
| POST /asesoria/inversion              | Operacion que permite invertir en fondos mutuo a traves de una asesoria                                           |
| POST /objetivo/inversion              | Operacion que permite ingresar un aporte de inversion en fondos mutuo desleccionado por inversion por objetivos   |
| POST /objetivo/aporte                 | Operacion que permite ingresar un aporte de inversion en fondos mutuo que ya pertenece a un objetivo de inversion |

## 3. Especificación Técnica
Detalle técnico del microservicio provisto por especificación Swagger.

http://api-dsr01.bci.cl/operaciones-y-ejecucion/gestion-de-inversion/ms-invertirffmm-neg/develop/v2/api-docs

## 4. Dependencias
Detalla los sistemas y otros microservicios que son necesarios para el funcionamiento de este microservicio.

### 4.1. Microservicios

| Dependencia | Descripción de la dependencia | Operación |
| ------  | ------  | ------  |
| ig-tdm-transaccionestransferencias | hace la transferencia de fondos  | CtaCteTrfEntNem  |
| ig-db-gestglobinv  | Guardar en gestglobinv el perfil fuera de rango si fuese el caso  | sp_ing_resp_inversion  |
| ms-validacionesinvertirffmm-neg  | Validación de requisitos  | /validarRequisitos  |
| ms-validacionesinvertirffmm-neg  | Validación de requisitos APV  | /validar-requisitos-apv  |
| ms-validacionesinvertirffmm-neg  | Validación del monto a invertir  | /validarMontos  |
| ms-notificacionescorreo-util  | envio de correos  | /notificarCorreoLatinia  |
| ms-reporteriapdf-util  | generar documentos pdf  | /pdf/generar  |
| api-eventos | Journalizar  | /eventos  |
| ms-firmadigital-util| firmar documentos pdf   | /firmadigital/simple |
| ms-precargaenrolamiento-orq | obtener informacion basica del cliente  | /ObtenerDatosPreCargaEnrolamiento  |
| ms-gestiondocumental-neg | gestion de respaldo de documentos  | /documento  |
| ms-repositoriodocumentos-neg | Respaldar documentos en filenet  | /repositorios/{repositorio}/documentos |


### 4.2. Repositorio de Datos

### 4.3. Backends

| Dependencia | Descripción de la dependencia | Operación |
| ------  | ------  | ------  |
| ServicioInversionesFFMM  | Obtiene un listado de las cuentas por fondo  | ObtenerCuentasCliente  |
| ServicioInversionesFFMM  | Obtiene el número de cuenta de un fondo  | ObtenerCuentaFondo  |
| ServicioInversionesFFMM  | Registra la inversión  | InvertirFondoMutuo  |
| ServicioInversionesFFMM  | Registra la firma del Contrato General de Fondos  | RegistrarCGFFondoMutuo  |

## 5. Eventos
En esta sección se especifican los eventos que utiliza el microservicio como parte su lógica de negocio.

| Evento | Descripcion | publicador/suscriptor |
| ------  | ------  | ------  |
| SOLIC  | Registra en el Journal la inversión realizada  | publicador  |

## 6. Ciclo de Vida
En esta sección se especifica el procedimiento para disponibilizar el microservicio.

### 6.1. Compilación
Detalla la forma en que el microservicio debe ser compilado:

./gradle 

### 6.2. Configuraciones
Detalla las configuraciones necesarias para que el microservicio pueda operar:

#### 6.2.1. Propiedades

#### 6.2.2. Secretos

### 6.3. Ejecución

#### 6.3.1. Contenedor
Especifica la forma en que se ejecuta la aplicación en un contenedor docker:

docker run -e SPRING_PROFILES_ACTIVE='<ambiente>' -e EUREKA_SERVICE_URL='http://172.31.0.157:8761/eureka' -e SPRING_CLOUD_CONFIG_URI='http://172.31.0.111:8888' -p 8080:8080 --log-driver=syslog --log-opt syslog-address=tcp+tls://169.48.183.41:5000 --name=ms-gestioncuentascliente-neg registry.ng.bluemix.net/mobcontainers/ms-gestioncuentascliente-neg:v1.0.0

#### 6.3.2. Recursos Utilizados
Detalla los recursos utilizados por una instancia de un contenedor docker.

## 7. Change Log


### Versión 1.0.0 
- Se agrega nuevo endpoint que permite la inversion inicial de inversion por objetivo

