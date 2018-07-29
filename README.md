# challenge-config-repo

As configurações que servem às aplicações no repositorio https://github.com/morganyvm/jsprbt-fa8-k8s serão expostas no modelo:

/{label}/{application}-{profile}.yml

onde:

1. label = refere-se ao repositorio

2. application = refere-se à propriedade application.name configurada no arquivo bootstrap.yml de cada spring boot application.

2.1. deve ser uma composição {todo_service}, onde:

2.1.1. todo_service deve identificar o nome do artefato maven referente à aplicação

3. profile = refere-se ao contexto/ambiente

3.1. deve ser uma composição {runtime_env}, onde:

3.1.1. runtime = local, kubernetes

3.1.2. env = (e.g. challenge, dev, prod, etc).


---

label = jsprbt-fa8-k8s

application = todo_service

runtime = local

env = challenge

profile = {runtime}_{env} = local_challenge


/jsprbt-fa8-k8s/todo_service-local_challenge.yml 



/jsprbt-fa8-k8s/todo_service-kubernetes_challenge.yml 

No caso de local o repositorio poderá ser configurado diretamente no arquivo bootstrap.yml

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/spring-cloud-samples/config-repo
          repos:
            development:
              pattern:
                - '*/development'
                - '*/staging'
              uri: https://github.com/development/config-repo
              
            staging:
              pattern:
                - '*/qa'
                - '*/production'
              uri: https://github.com/staging/config-repo
```


---
deployment.yml
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

