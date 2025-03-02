# EchoIP Helm Chart

## Introduction
[Echoip](https://github.com/mpolden/echoip) is the project behind [ifconfig.co](https://ifconfig.co), an IP address information site I have used for some time. When I wanted to practice creating a [Helm](https://helm.sh) chart from scratch, I decided to make one for this. 

## Using this chart
Simply add the repo to Helm, and install. 
```
$ helm repo add echo https://rikkoenig.github.io/echo-chart/charts 
$ helm install echo echoip/echoip 
```

## GeoIP information
Echoip can use [GeoLite databases](https://www.maxmind.com/en/geolite-free-ip-geolocation-data) to provide location information for IP addresses. To use this feature:
1. Sign up for a free login at MaxMind https://www.maxmind.com/en/geolite2/signup
2. Generate a license key
3. Create a Kubernetes secret with the data in the format of `AccountID:LicenceKey`
4. Set the values to enable geoDB and use the license key.

## Accessing the service
By default, this creates a ClusterIP service. You can switch this to a NodePort by the values file. For use with a load balancer or ingress, that is currently an exercise for the user. Ingress/LB options may be added in the chart later.

## Values file
As this is a simple application, there's not much needed to configure.


| entry                        | Description                             | Type                | Default                   |
| ---------------------------- | --------------------------------------- | ------------------- | ------------------------- |
| container.image              | The location of the container image     | string              | docker.io/mpolden/echoip  |
| container.tag                | The tag for the image                   | string              | latest                    |
| container.commandargs        | The arguments passed to the application | sequence of strings | ["-H, "X-Forwarded-For"]  |
| namespace                    | The namespace for the application       | string              | reflector                 |
| service.port                 | The TCP port to use for the service     | integer             | 8080                      |
| nodePort.enabled             | Whether to make the service a NodePort  | boolean             | false                     |
| nodePort.port                | If set, define the nodeport to use      | integer             | none                      |
| resources.cpu.request        | Set the cpu request for the container   | string              | 10m                       |
| resources.cpu.limit          | Set the cpu limit for the container     | string              | 100m                      |
| resources.memory.request     | Set the mem request for the container   | string              | 20Mi                      |
| resources.memory.limit       | Set the mem limit for the container     | string              | 50Mi                      |
| securityContext.runAsGroup   | Set the run-as group                    | integer             | 1000                      | 
| securityContext.runAsNonRoot | Set the container to run as std user    | boolean             | true                      |
| securityContext.runAsUser    | Set the run-as user                     | integer             | 1000                      |
| geoDB.enabled                | Enable/disable the use of geoIP db      | boolean             | false                     |
| geoDB.secret.name            | Name of the secret storing the MM auth  | string              | ""                        |
| geoDB.secret.key             | Name of the key for the MM auth string  | string              | ""                        |

## Requests/recommendations
If you have a request or recommendation to improve the chart, [please create a ticket in GitHub!](https://github.com/rikkoenig/echo-chart/issues)     