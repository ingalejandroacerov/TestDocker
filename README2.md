# Diplomado En Arquitectura de Software - Trabajo Final k8s

## Grupo:


## Capitulos:
[1. Instalación del chart manualmente con Helm](#user-content-1-instalación-del-chart-manualmente-con-helm)

[2. Configuración para sincronizar ArgoCD](#user-content-2-configuración-para-sincronizar-argocd)

[3. Endpoints de acceso (backend)](#user-content-3-endpoints-de-acceso-backend)

[4. Demostración en vivo](#user-content-4-demostracion-en-vivo)







## 1. Instalación del chart manualmente con Helm.

### 1.1 En la Aplicación establecemos las dependecias (repositorio Bitnami) en el archivo Chart.yaml

```
apiVersion: v2
name: pedido-app
description: Sistema de gestión de pedidos

type: application
version: 0.1.0
appVersion: "1.0.0"

dependencies:
  - name: postgresql
    version: "12.12.10"
    repository: https://charts.bitnami.com/bitnami
    alias: db
    condition: db.enabled
```

Creamos e instalamos el entorno de desarrollo de k8s configurado anteriormente en deployment.yaml

* Codigo:
  
 :link: [Ir a deployment.yaml](...)


* Consola:

```
kubectl create ...
```

##  2. Configuración para sincronizar ArgoCD


##  3. Endpoints de acceso (backend)

##  4. Demostracion en vivo
