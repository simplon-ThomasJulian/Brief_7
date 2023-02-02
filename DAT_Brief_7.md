DAT Brief 7 architecture


![dat_brief7_gabriel](https://user-images.githubusercontent.com/107990221/216087491-05870a28-aaf5-4695-a4e9-b0aa24c74fac.png)

![dat_brief7](https://user-images.githubusercontent.com/107990221/216079986-b6bca996-8cbf-44c6-85e0-0a12657a84ff.png)


Objectifs
On utilise Kubernetes et Azure pour fournir une application de vote, une base de données Redis et un stockage persistant pour la base de données. L'application de vote doit être disponible via une zone DNS grace à une application gateway.


listes des éléments déployé avec kubernetes

    Secrets
        mot de passe pour redis
        certificat TLS
        clé privée TLS
    storage
        storageclass
            azure disk
        PersistentVolumeClaim
            8Gi
            read write once
    redis
        deployment redis
            replicas: 1
            volume
            port 6379
            env:
                REDIS_PWD
                ALLOW_EMPTY_PASSWORD:no
        clusterIP redis
            port: 6379
    ingress
        ingress azure application gateway
            tls
            azure apllication gateway
    voting app
        deployment voteapp
            replicas: 2
            port: 80
            resources:
                request, cpu: 250m
                limits, cpu: 500m
            env:
                REDIS
                STRESS-SECS: 2
                REDIS_PWD
        clusterIP voteapp
            port 80
        HorizontalPodAutoscaler
            max replicas: 8
            min replicas: 2
            scale target ref: voteapp
            CPU utilisation %: 70

zone DNS

https://votingappkube.simplon-thomas.space/

éléments déployés sur azure

    azure kubernetes service
        2 nodes
        ingress application gateway

PipeLine

    Choix d'Azure Pipeline car intégré à l'environment Azure, gratuit et simple d'utilisation.
    Choix d'un trigger basé sur une crontbale de run toutes les heures à minute 0

Cost forecast monthly

    1 AKS...........................Pay as you go: €69.46
        1 nodepool
            2 node: Standard B2s
        1 SSD premium: 32 Gb
    blob storage.....................Pay as you go:€0.08
        blob storage: 1Go
        10 000 writing operation
        10 000 read operation

Total €69.53
