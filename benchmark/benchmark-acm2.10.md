# ACM Agents Benchmark

This doc provides the testing steps and benchmark data of ACM agents.

## Testing environment

1 hub + 2 managed cluster, each cluster has: 

- allocatable cpu: 7500m
- allocatable memory: 31230880Ki
- capacity cpu: 8
- capacity memory: 32381856Ki

## Testing version 

- OCP 4.14.3
- ACM 2.10.0
- MCE 2.5.1

## Testing flow

Prepare TWO ACM 2.10.0 environments, each has 1 hub + 2 managed clusters, follow the testing steps in [acm-workload](https://github.com/stolostron/acm-workload/blob/release-2.10/README.md) to deploy workload and get resource usage of each agent. 

We will finally get 4 groups of data on managed cluster resource usage, each group contains 4 use cases (idle, creating 10 applications, creating 10 policies, and creating 100 manifestworks). Calculate the average as a result. 

## Benchmark

**_Note_**
- Each addon below can be turned on/off on demand, curate which addon(s) to leverage, to control the footprint.
- The original metrics are collected at https://drive.google.com/drive/folders/1PdHoS9eMo1KvmOiPHNZOHJfuoPQ71AxW.

**CPU usage (millicore) of each agent**

| Pod                             | Idle     | 10 applications | 10 policies (each contains 5 config policies) | 100 manifestworks |
|---------------------------------|----------|-----------------|-----------------------------------------------|--------------------|
| klusterlet                      | 1.725    | 1.75            | 1.825                                         | 1.925              |
| klusterlet-agent                | 4.35     | 4.85            | 4.575                                         | 18.825             |
| klusterlet-addon-workmgr        | 0.95     | 1               | 1.025                                         | 1.05               |
| application-manager             | 3.1      | 5.875           | 3.2                                           | 3.175              |
| klusterlet-addon-search         | 15.7     | 15.925          | 21.375                                        | 18.725             |
| config-policy-controller        | 0.675    | 0.7             | 45.6                                          | 0.775              |
| governance-policy-framework     | 0.9      | 0.9             | 44.575                                        | 1.025              |
| cluster-proxy-proxy-agent       | 0.4      | 0.4             | 0.425                                         | 0.45               |
| endpoint-observability-operator | 0.55     | 0.575           | 0.6                                           | 0.6                |
| metrics-collector-deployment    | 0.7      | 0.7             | 0.7                                           | 0.75               |
| **Total**                       | **29.05**| **32.675**      | **123.9**                                     | **47.3**           |


**impact to control plane CPU usage (millicore)**
| Component        | no ACM   | Idle     | 10 applications | 10 policies (each contains 5 config policies) | 100 manifestworks |
|------------------|----------|----------|-----------------|-----------------------------------------------|--------------------|
| kube api server  | 235.675  | 252.125  | 260.175         | 565.875                                       | 306.925            |
| kubelet          | 121.25   | 129.65   | 138             | 134.85                                        | 205.575            |

**Memory usage (MB) of each agent**

| Pod                             | Idle    | 10 applications | 10 policies (each contains 5 config policies) | 100 manifestworks |
|---------------------------------|---------|-----------------|-----------------------------------------------|--------------------|
| klusterlet                      | 38.75   | 40.75           | 41.225                                        | 41.825             |
| klusterlet-agent                | 65.5    | 67.775          | 69.7                                          | 76.325             |
| klusterlet-addon-workmgr        | 27.15   | 29              | 28.75                                         | 30.05              |
| application-manager             | 23.3    | 41.925          | 39.15                                         | 39.525             |
| klusterlet-addon-search         | 99.1    | 103.125         | 103.275                                       | 109.85             |
| config-policy-controller        | 51.2    | 53.325          | 62.175                                        | 62.225             |
| governance-policy-framework     | 49.775  | 51.4            | 59.8                                          | 57.95              |
| cluster-proxy-proxy-agent       | 36.65   | 38.65           | 38.65                                         | 38.775             |
| endpoint-observability-operator| 35.975 | 35.3            | 35.65                                         | 36.425             |
| metrics-collector-deployment    | 50.725  | 54.225          | 52.325                                        | 54.95              |
| **Total**                       | **478.125** | **515.475**     | **530.7**                                     | **547.9**          |

| Component        | no ACM | Idle    | 10 applications | 10 policies (each contains 5 config policies) | 100 manifestworks |
|------------------|--------|---------|-----------------|-----------------------------------------------|--------------------|
| kube api server  | 2534.4 | 2743.2  | 2791.2          | 2764.8                                        | 2688               |
| kubelet          | 378    | 396.7   | 411.025         | 404.525                                       | 533.525            |