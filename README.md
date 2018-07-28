# challenge-config-repo

As configurações que servem às aplicações no repositorio https://github.com/morganyvm/jsprbt-fa8-k8s serão expostas no modelo:

/{label}/{application}-{profile}.yml

onde:

1. label = refere-se ao repositorio
2. application = refere-se à propriedade application.name configurada no arquivo bootstrap.yml de cada spring boot application.
2.1. 
3. profile = refere-se ao contexto/ambiente
3.1. devem ser uma composição {runtime_env}, onde:
3.1.1. runtime = local, kuberntes
3.1.2. env = (e.g. desafio, dev, prod, etc).

/jsprbt-fa8-k8s/todo_service-kubernetes_challenge.yml 

No caso de local o repositorio poderá ser configurado diretamente no arquivo bootstrap.yml

```yaml
spec:
  replicas: 1
  template:
    spec:
      volumes:
        - name: config
          gitRepo:
            repository: 'https://github.com/morganyvm/challenge-config-repo.git'
            revision: 667ee4db6bc842b127825351e5c9bae5a4fb2147
            directory: .
      containers:
        - volumeMounts:
            - name: config
              mountPath: /app/config
          env:
            - name: KUBERNETES_NAMESPACE
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.namespace
```

Veja mais em: http://cloud.spring.io/spring-cloud-static/spring-cloud-config/2.0.0.RELEASE/single/spring-cloud-config.html

