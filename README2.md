# Diplomado En Arquitectura de Software - Trabajo Final k8s

## Grupo:


## Capitulos:
[1. Instalación del chart manualmente con Helm](#user-content-1-instalación-del-chart-manualmente-con-helm)

[2. Configuración para sincronizar ArgoCD](#user-content-2-configuración-para-sincronizar-argocd)

[3. Endpoints de acceso (backend)](#user-content-3-endpoints-de-acceso-backend)

[4. Demostración en vivo](#user-content-4-demostracion-en-vivo)

[Test](.github/workflows/CI.yml)





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
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "pedido-app.fullname" . }}
  labels:
    {{- include "pedido-app.labels" . | nindent 4 }}
spec:
  {{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}
  selector:
    matchLabels:
      {{- include "pedido-app.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      {{- with .Values.podAnnotations }}
      annotations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      labels:
        {{- include "pedido-app.labels" . | nindent 8 }}
        {{- with .Values.podLabels }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
    spec:
      {{- with .Values.imagePullSecrets }}
      imagePullSecrets:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      serviceAccountName: {{ include "pedido-app.serviceAccountName" . }}
      {{- with .Values.podSecurityContext }}
      securityContext:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      containers:
        - name: {{ .Chart.Name }}
          {{- with .Values.securityContext }}
          securityContext:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: {{ .Values.service.port }}
              protocol: TCP
          env:
            - name: DATABASE_URL
              valueFrom:
                configMapKeyRef:
                  name: {{ include "pedido-app.fullname" . }}-config
                  key: DATABASE_URL
            - name: DATABASE_USERNAME
              valueFrom:
                secretKeyRef:
                  name: {{ include "pedido-app.fullname" . }}-secret
                  key: DATABASE_USERNAME
            - name: DATABASE_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: {{ include "pedido-app.fullname" . }}-secret
                  key: DATABASE_PASSWORD
            - name: SPRING_PROFILES_ACTIVE
              value: "prod"
          {{- with .Values.livenessProbe }}
          livenessProbe:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          {{- with .Values.readinessProbe }}
          readinessProbe:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          {{- with .Values.resources }}
          resources:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          {{- with .Values.volumeMounts }}
          volumeMounts:
            {{- toYaml . | nindent 12 }}
          {{- end }}
      {{- with .Values.volumes }}
      volumes:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
```

```
kubectl create ...
```

##  2. Configuración para sincronizar ArgoCD


##  3. Endpoints de acceso (backend)

##  4. Demostracion en vivo
