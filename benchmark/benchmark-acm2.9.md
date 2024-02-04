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

Prepare TWO ACM 2.9 environments, each has 1 hub + 2 managed clusters, follow the testing steps in [acm-workload](https://github.com/haoqing0110/acm-workload/blob/main/README.md) to deploy workload and get resource usage of each agent. 

We will finally get 4 groups of data on managed cluster resource usage, each group contains 4 use cases (idle, creating 10 applications, creating 6 config policies, and creating 100 manifestworks). Calculate the average as a result. 

## Benchmark

**_Note_**
- Each addon below can be turned on/off on demand, curate which addon(s) to leverage, to control the footprint.
- The original metrics are collected at https://drive.google.com/drive/folders/1jkUGD0mkYq4ibqYN2IK9mqougb8ZuW4g. 

**CPU usage (millicore) of each agent**

| Pod                             | idle       | 10 applications | 2 policies (each contains 3 config policy) | 100 manifestworks |
|----------------------------------|------------|-----------------|--------------------------------------------|---------------------|
| klusterlet                       | 2.0        | 1.9             | 1.9                                        | 1.9                 |
| klusterlet-agent                 | 4.2        | 4.4             | 4.1                                        | 13.2                |
| klusterlet-addon-workmgr         | 1.1        | 1.1             | 1.1                                        | 1.1                 |
| application-manager              | 2.2        | 4.6             | 2.2                                        | 2.2                 |
| klusterlet-addon-search          | 12.0       | 12.8            | 12.4                                       | 13.0                |
| config-policy-controller         | 0.9        | 1.0             | 1.0                                        | 1.0                 |
| governance-policy-framework      | 1.0        | 1.0             | 1.0                                        | 1.0                 |
| cluster-proxy-proxy-agent         | 0.5        | 0.5             | 0.5                                        | 0.5                 |
| cluster-proxy-service-proxy       | 0.1        | 0.1             | 0.1                                        | 0.1                 |
| endpoint-observability-operator   | 0.6        | 0.6             | 0.6                                        | 0.6                 |
| metrics-collector-deployment      | 0.5        | 0.6             | 0.5                                        | 0.6                 |
| **Total**                        | **27.1**   | **28.6**        | **25.4**                                   | **35.2**            |

**Memory usage (MB) of each agent**

| Pod                             | idle       | 10 applications | 2 policies (each contains 3 config policy) | 100 manifestworks |
|----------------------------------|------------|-----------------|--------------------------------------------|---------------------|
| klusterlet                       | 50.7       | 52.7            | 52.8                                       | 52.9                |
| klusterlet-agent                 | 71.0       | 73.2            | 72.8                                       | 78.8                |
| klusterlet-addon-workmgr         | 42.6       | 44.5            | 44.8                                       | 44.9                |
| application-manager              | 31.4       | 55.4            | 52.3                                       | 52.9                |
| klusterlet-addon-search          | 100.4      | 103.9           | 103.3                                      | 104.5               |
| config-policy-controller         | 66.1       | 68.5            | 70.4                                       | 69.6                |
| governance-policy-framework      | 60.9       | 63.7            | 69.1                                       | 68.5                |
| cluster-proxy-proxy-agent         | 33.2       | 35.4            | 35.4                                       | 35.4                |
| cluster-proxy-service-proxy       | 19.6       | 20.5            | 20.5                                       | 20.5                |
| endpoint-observability-operator   | 41.3       | 42.7            | 42.7                                       | 43.0                |
| metrics-collector-deployment      | 50.7       | 53.4            | 54.1                                       | 54.5                |
| **Total**                        | **567.9**  | **572.9**       | **578.2**                                  | **585.5**           |

**API server**

There's almost no impact on the managed cluster API server:
- The `apirequestcount` did not increase much before and after ACM deployed.
- The API server related metrics did not increase much before and after the ACM workload was deployed, the peek value only occurred when deploying 100 manifest works. For example, [apiserver-resource-count](https://drive.google.com/drive/folders/1eopRGJyQQ3JFHbxDFwGnmhRKT-k1MWZC), [apiserver-request-count-by-object](https://drive.google.com/drive/folders/1eopRGJyQQ3JFHbxDFwGnmhRKT-k1MWZC), [kubeapi-cpu-usage](https://drive.google.com/drive/folders/18LWvcIbDk7-EDd8x2vDpkmN9uJRWipe1), [kubeapi-mem-usage-wss](https://drive.google.com/drive/folders/18LWvcIbDk7-EDd8x2vDpkmN9uJRWipe1)

