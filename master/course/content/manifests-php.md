```
apiVersion: v1
kind: ConfigMap
metadata:
  name: phpinfo
data:
  index.php: <?php phpinfo();?>
```
```
apiVersion: v1
kind: Secret
metadata:
  name: phpinfo
stringData:
  index.php: <?php phpinfo();?>
```
```
apiVersion: v1
kind: Secret
metadata:
  name: phpinfo2
data:
  index.php: PD9waHAgcGhwaW5mbygpOz8+
```
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: phpinfo
spec:
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: phpinfo
```
```
apiVersion: v1
kind: Service
metadata:
  name: phpinfo
spec:
  ports:
  - 
    port: 80
    targetPort: 9000
  selector:
    app: phpinfo
  type: ClusterIP
```
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: phpinfo
  name: phpinfo
spec:
  containers:
  - 
    command:
    - php
    - -f
    - index.php
    - -S
    - 0.0.0.0:9000
    image: docker.io/library/php:latest
    name: phpinfo
    ports:
    -
      containerPort: 9000
      protocol: TCP
    resources:
      limits:
        cpu: 40m
        memory: 40M
      requests:
        cpu: 20m
        memory: 20M
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - 
      mountPath: /var/data/index.php
      name: phpinfo
      readOnly: true
      subPath: index.php
    -
      mountPath: /var/data/tmp/
      name: tmp
      readOnly: false
    workingDir: /var/data/
  volumes:
  - 
    configMap:
      defaultMode: 0400
      items: 
      -
        key: index.php
        mode: 0400
        path: index.php
      name: phpinfo
    name: phpinfo
  -
    emptyDir:
      medium: Memory
      sizeLimit: 10Mi
    name: tmp
```
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: phpinfo
spec:
  replicas: 2
  selector:
    app: phpinfo
    kind: rc
  template:
    metadata:
      labels:
        app: phpinfo
        kind: rc
        ha: 'true'
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: ha
                operator: In
                values:
                - 'true'
            topologyKey: topology.kubernetes.io/zone
      containers:
      - 
        command:
        - php
        - -f
        - index.php
        - -S
        - 0.0.0.0:9000
        image: docker.io/library/php:latest
        name: phpinfo
        ports:
        -
          containerPort: 9000
          protocol: TCP
        resources:
          limits:
            cpu: 40m
            memory: 40M
          requests:
            cpu: 20m
            memory: 20M
        securityContext:
          readOnlyRootFilesystem: true
        volumeMounts:
        - 
          mountPath: /var/data/index.php
          name: phpinfo
          readOnly: true
          subPath: index.php
        - 
          mountPath: /var/data/tmp/
          name: tmp
          readOnly: false
        workingDir: /var/data/
      volumes:
      - 
        configMap:
          defaultMode: 0400
          items: 
          -
            key: index.php
            mode: 0400
            path: index.php
          name: phpinfo
        name: phpinfo
      -
        emptyDir:
          medium: Memory
          sizeLimit: 10Mi
        name: tmp
```
```
apiVersion: apps.openshift.io/v1
kind: DeploymentConfig
metadata:
  name: phpinfo
spec:
  replicas: 2
  selector:
    app: phpinfo
    kind: dc
  template:
    metadata:
      labels:
        app: phpinfo
        kind: dc
        ha: 'true'
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: ha
                operator: In
                values:
                - 'true'
            topologyKey: topology.kubernetes.io/zone
      containers:
      - 
        command:
        - php
        - -f
        - index.php
        - -S
        - 0.0.0.0:9000
        image: docker.io/library/php:latest
        name: phpinfo
        ports:
        -
          containerPort: 9000
          protocol: TCP
        resources:
          limits:
            cpu: 40m
            memory: 40M
          requests:
            cpu: 20m
            memory: 20M
        securityContext:
          readOnlyRootFilesystem: true
        volumeMounts:
        - 
          mountPath: /var/data/index.php
          name: phpinfo
          readOnly: true
          subPath: index.php
        - 
          mountPath: /var/data/tmp/
          name: tmp
          readOnly: false
        workingDir: /var/data/
      volumes:
      - 
        configMap:
          defaultMode: 0400
          items: 
          -
            key: index.php
            mode: 0400
            path: index.php
          name: phpinfo
        name: phpinfo
      -
        emptyDir:
          medium: Memory
          sizeLimit: 10Mi
        name: tmp
```
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: phpinfo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: phpinfo
      kind: rs
  template:
    metadata:
      labels:
        app: phpinfo
        kind: rs
        ha: 'true'
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: ha
                operator: In
                values:
                - 'true'
            topologyKey: topology.kubernetes.io/zone
      containers:
      - 
        command:
        - php
        - -f
        - index.php
        - -S
        - 0.0.0.0:9000
        image: docker.io/library/php:latest
        name: phpinfo
        ports:
        -
          containerPort: 9000
          protocol: TCP
        resources:
          limits:
            cpu: 40m
            memory: 40M
          requests:
            cpu: 20m
            memory: 20M
        securityContext:
          readOnlyRootFilesystem: true
        volumeMounts:
        - 
          mountPath: /var/data/index.php
          name: phpinfo
          readOnly: true
          subPath: index.php
        - 
          mountPath: /var/data/tmp/
          name: tmp
          readOnly: false
        workingDir: /var/data/
      volumes:
      - 
        configMap:
          defaultMode: 0400
          items: 
          -
            key: index.php
            mode: 0400
            path: index.php
          name: phpinfo
        name: phpinfo
      -
        emptyDir:
          medium: Memory
          sizeLimit: 10Mi
        name: tmp
```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: phpinfo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: phpinfo
      kind: deploy
  template:
    metadata:
      labels:
        app: phpinfo
        kind: deploy
        ha: 'true'
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: ha
                operator: In
                values:
                - 'true'
            topologyKey: topology.kubernetes.io/zone
      containers:
      - 
        command:
        - php
        - -f
        - index.php
        - -S
        - 0.0.0.0:9000
        image: docker.io/library/php:latest
        name: phpinfo
        ports:
        -
          containerPort: 9000
          protocol: TCP
        resources:
          limits:
            cpu: 40m
            memory: 40M
          requests:
            cpu: 20m
            memory: 20M
        securityContext:
          readOnlyRootFilesystem: true
        volumeMounts:
        - 
          mountPath: /var/data/index.php
          name: phpinfo
          readOnly: true
          subPath: index.php
        - 
          mountPath: /var/data/tmp/
          name: tmp
          readOnly: false
        workingDir: /var/data/
      volumes:
      - 
        configMap:
          defaultMode: 0400
          items: 
          -
            key: index.php
            mode: 0400
            path: index.php
          name: phpinfo
        name: phpinfo
      -
        emptyDir:
          medium: Memory
          sizeLimit: 10Mi
        name: tmp
```
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: phpinfo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: phpinfo
      kind: sts
  serviceName: phpinfo
  template:
    metadata:
      labels:
        app: phpinfo
        kind: sts
        ha: 'true'
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: ha
                operator: In
                values:
                - 'true'
            topologyKey: topology.kubernetes.io/zone
      containers:
      - 
        command:
        - php
        - -f
        - index.php
        - -S
        - 0.0.0.0:9000
        image: docker.io/library/php:latest
        name: phpinfo
        ports:
        -
          containerPort: 9000
          protocol: TCP
        resources:
          limits:
            cpu: 40m
            memory: 40M
          requests:
            cpu: 20m
            memory: 20M
        securityContext:
          readOnlyRootFilesystem: true
        volumeMounts:
        -
          mountPath: /var/data/claim/
          name: claim
          readOnly: false
        - 
          mountPath: /var/data/index.php
          name: phpinfo
          readOnly: true
          subPath: index.php
        - 
          mountPath: /var/data/tmp/
          name: tmp
          readOnly: false
        workingDir: /var/data/
      volumes:
      - 
        configMap:
          defaultMode: 0400
          items: 
          -
            key: index.php
            mode: 0400
            path: index.php
          name: phpinfo
        name: phpinfo
      -
        emptyDir:
          medium: Memory
          sizeLimit: 10Mi
        name: tmp
  volumeClaimTemplates:
    -
      metadata:
        name: claim
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 1Gi
        volumeMode: Filesystem
```
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: phpinfo
spec:
  selector:
    matchLabels:
      app: phpinfo
      kind: ds
  template:
    metadata:
      labels:
        app: phpinfo
        kind: ds
    spec:
      containers:
      - 
        command:
        - php
        - -f
        - index.php
        - -S
        - 0.0.0.0:9000
        image: docker.io/library/php:latest
        name: phpinfo
        ports:
        -
          containerPort: 9000
          protocol: TCP
        resources:
          limits:
            cpu: 40m
            memory: 40M
          requests:
            cpu: 20m
            memory: 20M
        securityContext:
          readOnlyRootFilesystem: true
        volumeMounts:
        - 
          mountPath: /var/data/index.php
          name: phpinfo
          readOnly: true
          subPath: index.php
        - 
          mountPath: /var/data/tmp/
          name: tmp
          readOnly: false
        workingDir: /var/data/
      volumes:
      - 
        configMap:
          defaultMode: 0400
          items: 
          -
            key: index.php
            mode: 0400
            path: index.php
          name: phpinfo
        name: phpinfo
      -
        emptyDir:
          medium: Memory
          sizeLimit: 10Mi
        name: tmp
```
```
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: phpinfo
spec:
  minAvailable: 1
  selector:
    matchLabels:
      app: phpinfo
      kind: sts
```