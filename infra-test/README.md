# infra test app
this app deploys various infra components via operator CRDs, in order to test if the CRDs work correctly

## shell commands for testing
The following commands are consolidated from various yaml files, for easier cut-n-paste

Test connectivity to redis and postgresql
```shell
# shell into minimum-linux pod first

# connect to redis
redis-cli -h redis-standalone

# connect to postgresql
psql -h test-postgres-rw -U app app
```

Test  ```knative-serving```
```shell

curl -v $(kubectl get  ksvc testfunc -n test -o json | jq -r .status.url):8080/check

# pod will be terminated shortly
```

Test  ```knative-eventing```

```shell

# to reach the broker, launch a new pod and shell into it
kubectl run linux -n default --rm -it --image=alpine:latest \
     --command -- sh -c 'apk add --no-cache curl bash bind-tools busybox-extras && sh'

# inside the pod
curl -v http://broker-ingress.knative-eventing.svc.cluster.local/test/default \
  -X POST \
  -H "Ce-Id: test-$(date +%s)" \
  -H "Ce-Source: test/testfunc" \
  -H "Ce-Type: dev.vino9.infra.test.infra.ping" \
  -H "Ce-Specversion: 1.0" \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello from trigger test", "data": "some test data"}'

# a new pod should be started and its log should show a POST is received
```
