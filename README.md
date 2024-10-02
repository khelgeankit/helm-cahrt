apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "my-app-chart.fullname" . }}
  labels:
    app: {{ include "my-app-chart.name" . }}
    chart: {{ include "my-app-chart.chart" . }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ include "my-app-chart.name" . }}
  template:
    metadata:
      labels:
        app: {{ include "my-app-chart.name" . }}
    spec:
      containers:
        - name: {{ include "my-app-chart.name" . }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
