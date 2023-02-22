# DAT Brief_6

## Objectif

Déployer l’application de vote avec une base de donnée redis, un stockage persistant pour la base de donnée.
Cette dernière doit être disponible via notre zone DNS en passant par une Application Gateway.

## Solution 

Utilisation d’Ingress de Kubernetes, obtention d’un nom de domaine via Gandi.

## Topologie

### Part 1

```mermaid
flowchart LR

    internet[Internet]--> load_balancer

    subgraph azure[Azure]
        
        voting_app <---> load_balancer((Load Balancer))
        redis[Redis] --> storage_account[(Compte de stockage)]
        voting_app[Voting App] --> cluster_ip((Cluster IP)) --> redis
    end
```

### Part 2

```mermaid
flowchart LR

    internet[Internet] --> ingress((AGIC))

    subgraph azure[Azure]
        ingress --> load_balancer((Load Balancer))
        voting_app <---> load_balancer
        redis[Redis] --> storage_account[(Compte de stockage)]
        voting_app[Voting App] --> cluster_ip((Cluster IP)) --> redis
    end
```

## Ressources

- 1 groupe de ressources
- 1 cluster AKS
- 4 Noeuds 
- 1 AZURE GATEWAY
- + AGIC (Part 2)


## Stockage 

```mermaid 
flowchart BT

subgraph Azure
    subgraph Node 
    redis((Pods Redis))
    end
    pvc(pvc)
    sc[/storageClass\]
    pv(pv)
    AF[Azure File]
end

redis <-.->pvc-.->pv
sc <-.->pvc
pv -.->AF
```
