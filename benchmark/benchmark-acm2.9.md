# ACM Agents Benchmark

This doc provides the testing steps and benchmark data of ACM agents.

## Testing environment

1 hub + 2 managed cluster, each cluster has: 

- allocatable cpu: 15500m
- allocatable memory: 63400724Ki
- capacity cpu: 16
- capacity memory: 64551700Ki

## Testing version 

- OCP 4.12.18
- ACM 2.9.2

## Testing flow

Prepare TWO ACM 2.9 environments, each has 1 hub + 2 managed clusters, follow the testing steps in [acm-workload](https://github.com/stolostron/acm-workload/blob/acm-2.9/README.md) to deploy workload and get resource usage of each agent. 

We will finally get 4 groups of data on managed cluster resource usage, each group contains 4 use cases (idle, creating 10 applications, creating 6 config policies, and creating 100 manifestworks). Calculate the average as a result. 

## Benchmark

**_Note_**
- Each addon below can be turned on/off on demand, curate which addon(s) to leverage, to control the footprint.
- The original metrics are collected at https://drive.google.com/drive/folders/1Dv8Q_8q1wDI3nuvMMeecR6nP60W6judw. 

**CPU usage (millicore) of each agent**

| Pod                            | idle    | 10 applications | 2 policies (each contains 3 config policy) | 100 manifestworks |
|--------------------------------|---------|-----------------|--------------------------------------------|-------------------|
| klusterlet                     | 2.1     | 2.1             | 2.1                                        | 2.1               |
| klusterlet-agent               | 4.5     | 4.7             | 4.6                                        | 14.1              |
| klusterlet-addon-workmgr       | 1.3     | 1.3             | 1.3                                        | 1.3               |
| application-manager            | 2.3     | 5.0             | 2.3                                        | 2.3               |
| klusterlet-addon-search        | 12.3    | 12.1            | 12.2                                       | 12.9              |
| config-policy-controller       | 1.0     | 1.0             | 1.1                                        | 1.0               |
| governance-policy-framework    | 1.0     | 1.1             | 1.1                                        | 1.0               |
| cluster-proxy-proxy-agent      | 0.6     | 0.5             | 0.6                                        | 0.5               |
| cluster-proxy-service-proxy    | 0.1     | 0.1             | 0.1                                        | 0.1               |
| endpoint-observability-operator| 0.7     | 0.7             | 0.7                                        | 0.7               |
| metrics-collector-deployment   | 0.5     | 0.6             | 0.5                                        | 0.6               |
| **Total**                      | **26.35**| **29**          | **26.475**                                 | **36.575**        |

**impact to control plane CPU usage (millicore)**
| Component                     | no ACM   | idle    | 10 applications | 2 policies (each contains 3 config policy) | 100 manifestworks |
|--------------------------------|----------|---------|-----------------|--------------------------------------------|-------------------|
| kube api server                | 244.225  | 256.7   | 256.3           | 255.475                                    | 278.35            |
| kubelet                        | 121.85   | 131.775 | 137.425         | 132.475                                    | 186.225           |

**Memory usage (MB) of each agent**

| Pod                            | idle    | 10 applications | 2 policies (each contains 3 config policy) | 100 manifestworks |
|-------------------------------|---------|-----------------|--------------------------------------------|-------------------|
| klusterlet                    | 49.9    | 51.1            | 51.4                                       | 51.5              |
| klusterlet-agent              | 70.9    | 73.0            | 72.6                                       | 78.6              |
| klusterlet-addon-workmgr      | 43.6    | 45.2            | 45.3                                       | 45.5              |
| application-manager           | 35.2    | 55.6            | 51.3                                       | 51.7              |
| klusterlet-addon-search       | 95.4    | 100.0           | 99.7                                       | 102.8             |
| config-policy-controller      | 71.9    | 73.1            | 77.5                                       | 75.8              |
| governance-policy-framework   | 61.3    | 63.1            | 72.3                                       | 71.7              |
| cluster-proxy-proxy-agent     | 35.6    | 37.1            | 37.1                                       | 37.1              |
| cluster-proxy-service-proxy   | 19.5    | 20.0            | 20.0                                       | 20.0              |
| endpoint-observability-operator| 41.9    | 42.3            | 42.4                                       | 42.5              |
| metrics-collector-deployment  | 53.3    | 54.4            | 54.2                                       | 54.5              |
| **Total**                     | **578.4**| **614.7**      | **623.5**                                  | **631.5**         |

**impact to control plane Memory usage (MB)**
| Component                     | no ACM   | idle    | 10 applications | 2 policies (each contains 3 config policy) | 100 manifestworks |
|--------------------------------|----------|---------|-----------------|--------------------------------------------|-------------------|
| kube api server (GB)          | 3.7      | 4.0     | 3.9             | 4.1                                        | 4.1               |
| kubelet                       | 428.3    | 438.6   | 445.7           | 438.6                                      | 494.9             |

