# Implementación/Configuración de Jenkins en Kubernetes mediante ArgoCD
# Autor: Francisco Javier Gutierrez | Unix/Linux Architect & Cloud Engineer

# Implementación de Jenkins en Kubernetes con ArgoCD y NGINX Ingress

Este documento proporciona instrucciones detalladas para desplegar Jenkins en un clúster de Kubernetes utilizando ArgoCD para la gestión de la configuración y NGINX como controlador de Ingress.

## Tabla de Contenidos

- [Prerequisitos](#prerequisitos)
- [Arquitectura de la Solución](#arquitectura-de-la-solución)
- [Configuración de Jenkins](#configuración-de-jenkins)
  - [1. Estructura del Repositorio Git](#1-estructura-del-repositorio-git)
  - [2. Archivos de Manifiesto](#2-archivos-de-manifiesto)
    - [a. Deployment de Jenkins](#a-deployment-de-jenkins)
    - [b. Ingress de Jenkins](#c-ingress-de-jenkins)
    - [c. Aplicación de ArgoCD](#d-aplicación-de-argocd)
- [Despliegue de la Aplicación](#despliegue-de-la-aplicación)
  - [1. Aplicar la Configuración de ArgoCD](#1-aplicar-la-configuración-de-argocd)
  - [2. Sincronizar la Aplicación en ArgoCD](#2-sincronizar-la-aplicación-en-argocd)
- [Verificación del Despliegue](#verificación-del-despliegue)
- [Notas Adicionales](#notas-adicionales)
- [Referencias](#referencias)

## Prerequisitos

Antes de comenzar, asegúrese de tener lo siguiente:

- **Clúster de Kubernetes** en funcionamiento.
- **kubectl** instalado y configurado para interactuar con tu clúster.
- **ArgoCD** instalado en el clúster.
- **Controlador Ingress NGINX** desplegado en el clúster.
- Acceso a un **repositorio Git** donde almacenarás los manifiestos de Kubernetes.

## Arquitectura de la Solución

### Configuración de Jenkins

- Estructura de directorios
   /Jenkins/
     ├── manifiestos/
     │   ├── jenkins-deployment.yaml
     │   └── jenkins-ingress.yaml
     └── argocd/
         └── application.yaml

### Despliegue de la Aplicación
1. Aplicar la Configuración de ArgoCD
Aplicar el manifiesto de la aplicación de ArgoCD para que comience a gestionar los recursos de Jenkins:
  kubectl apply -f jenkins/argocd/application.yaml

2. Sincronizar la Aplicación en ArgoCD
Se Puede sincronizar la aplicación desde la interfaz web de ArgoCD o utilizando la línea de comandos.

 a. Usando la Interfaz Web
    Acceder al dashboard de ArgoCD.
    Buscar la aplicación jenkins.
    Hacer clic en Sync y luego en Synchronize.
 b. Usando la Línea de Comandos
    Primero, instalar el CLI de ArgoCD si aún no lo tienes. Luego, ejecuta:
     argocd login <ARGOCD_SERVER>  # Reemplazar <ARGOCD_SERVER> con la dirección del servidor ArgoCD
      argocd app sync jenkins
    Verificación del Despliegue
    Verificar que todos los pods estén corriendo:
      kubectl get pods -n jenkins
    Verificar el estado del Ingress:
      kubectl get ingress -n jenkins
    Asegúrese de que el Ingress tenga una dirección IP asignada.

### Acceder a Jenkins:
Abrir un navegador web e ir a http://jenkins.bg.com.bo (reemplazar con el dominio real).

- Obtener la contraseña inicial de Jenkins:
  Para obtener la contraseña inicial de Jenkins, siguir estos pasos:
   - Identificar el nombre del pod de Jenkins: 
     Ejecuta el siguiente comando para listar los pods en el namespace de Jenkins:
      kubectl get pods -n jenkins

  Obtener la contraseña inicial: Una vez que tenga el nombre del pod, usar el siguiente comando para obtener la contraseña desde el archivo secrets dentro del contenedor de Jenkins:
   kubectl exec -n jenkins <nombre-del-pod> -- cat /var/jenkins_home/secrets/initialAdminPassword
   Reemplazar <nombre-del-pod> con el nombre real del pod de Jenkins que se identificó en el paso anterior.
  Esto mostrará la contraseña en la terminal, la cual podrás usar para acceder a Jenkins por primera vez.

## Referencias
Documentación Oficial de Jenkins
Documentación Oficial de ArgoCD
Documentación Oficial de Kubernetes
NGINX Ingress Controller

