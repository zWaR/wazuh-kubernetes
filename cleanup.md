# Clean up

Steps to perform a clean up of our deployments, services and volumes used in our environment.

## Wazuh managers

The deployment of the Wazuh cluster of managers involves the use of various statefulSet elements as well as configuration maps and services.


To delete your Wazuh cluster just use: `kubectl delete -k .` from this repository directory.


### Delete the persistent volumes manually.

Since we use `reclaimPolicy: Retain` in the storage class definition you must delete volumes manually if you want to clean these as well.


```
ubuntu@k8s-control-server:~$ kubectl get persistentvolume
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS        CLAIM                                                         STORAGECLASS             REASON    AGE
pvc-024466da-f7c5-11e8-b9b8-022ada63b4ac   10Gi       RWO            Retain           Released      wazuh/wazuh-manager-worker-wazuh-manager-worker-1             gp2-encrypted-retained             6d
pvc-b3226ad3-f7c4-11e8-b9b8-022ada63b4ac   30Gi       RWO            Retain           Bound         wazuh/wazuh-elasticsearch-wazuh-elasticsearch-0               gp2-encrypted-retained             6d
pvc-fb821971-f7c4-11e8-b9b8-022ada63b4ac   10Gi       RWO            Retain           Released      wazuh/wazuh-manager-master-wazuh-manager-master-0             gp2-encrypted-retained             6d
pvc-ffe7bf66-f7c4-11e8-b9b8-022ada63b4ac   10Gi       RWO            Retain           Released      wazuh/wazuh-manager-worker-wazuh-manager-worker-0             gp2-encrypted-retained             6d
```

```
ubuntu@k8s-control-server:~$ kubectl delete persistentvolume pvc-b3226ad3-f7c4-11e8-b9b8-022ada63b4ac
```

Once these steps are completed, our Kubernetes environment will be clean of deployments relating to the Wazuh cluster and related Elastic Stack components.