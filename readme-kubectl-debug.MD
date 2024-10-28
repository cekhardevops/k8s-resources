```
kubectl create -f 01-namespace.yaml
kubectl get ns
kubectl describe ns expense
kubectl create -f namespace.yaml --v=6
kubectl delete -f 01-namespace.yaml
kubectl create -f 01-namespace.yaml --v=6
kubectl create -f 01-namespace.yaml --v=10
kubectl create -f 01-namespace.yaml --v=7
kubectl create -f 01-namespace.yaml --v=6
kubectl create -f 01-namespace.yaml --v=8
```

gives detailed information about what's happening internally when we execute the kubectl command. we can provide 1-10 based on log level we required.
  
***how kubectl connects to our eks cluster without informing anything about our cluster?***

***When a Kubernetes cluster is created using the eksctl command in AWS, it generates a ~/.kube/config file that contains the necessary cluster and user information for accessing the cluster. Once the cluster is set up, executing the command kubectl create -f <resource-manifest-file>.yaml will connect to the Kubernetes cluster specified in the current-context of this kubeconfig file."***

**sample kubeconfig file:**

```  
  cat ~/.kube/config
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURERNQVFTVTR3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkR......
	Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://CAACC064F91730872A2378475808913C.gr7.us-east-1.eks.amazonaws.com
  name: expense-1.us-east-1.eksctl.io
contexts:
- context:
    cluster: expense-1.us-east-1.eksctl.io
    user: eks@expense-1.us-east-1.eksctl.io
  name: eks@expense-1.us-east-1.eksctl.io
current-context: eks@expense-1.us-east-1.eksctl.io
kind: Config
preferences: {}
users:
- name: eks@expense-1.us-east-1.eksctl.io
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - eks
      - get-token
      - --output
      - json
      - --cluster-name
      - expense-1
      - --region
      - us-east-1
      command: aws
      env:
      - name: AWS_STS_REGIONAL_ENDPOINTS
        value: regional
      provideClusterInfo: false

```

debug log:
-----------
```
$ kubectl create -f 01-namespace.yaml --v=8

I1028 05:11:49.358755    3731 loader.go:395] Config loaded from file:  /home/ec2-user/.kube/config
I1028 05:11:49.359666    3731 round_trippers.go:463] GET https://CAACC064F91730872A2378475808913C.gr7.us-east-1.eks.amazonaws.com/openapi/v3?timeout=32s
I1028 05:11:49.359682    3731 round_trippers.go:469] Request Headers:
I1028 05:11:49.359731    3731 round_trippers.go:473]     Accept: application/json, */*
I1028 05:11:49.359742    3731 round_trippers.go:473]     User-Agent: kubectl/v1.31.0 (linux/amd64) kubernetes/da2ed8d
I1028 05:11:50.434182    3731 round_trippers.go:574] Response Status: 200 OK in 1074 milliseconds
I1028 05:11:50.434226    3731 round_trippers.go:577] Response Headers:
I1028 05:11:50.434245    3731 round_trippers.go:580]     Cache-Control: no-cache, private
I1028 05:11:50.434255    3731 round_trippers.go:580]     Content-Type: text/plain; charset=utf-8
I1028 05:11:50.434264    3731 round_trippers.go:580]     Last-Modified: Mon, 28 Oct 2024 05:11:50 GMT
I1028 05:11:50.434272    3731 round_trippers.go:580]     X-Kubernetes-Pf-Flowschema-Uid: a100abb1-2a27-4c53-b81e-ff14fc8f6a35
I1028 05:11:50.434280    3731 round_trippers.go:580]     X-Kubernetes-Pf-Prioritylevel-Uid: 34f9615d-c63b-4407-bf62-f8ebfbad8836
I1028 05:11:50.434289    3731 round_trippers.go:580]     Content-Length: 11171
I1028 05:11:50.434298    3731 round_trippers.go:580]     Accept-Ranges: bytes
I1028 05:11:50.434305    3731 round_trippers.go:580]     Audit-Id: 800505ad-9923-4d53-ac03-03e94e025801
I1028 05:11:50.434312    3731 round_trippers.go:580]     Date: Mon, 28 Oct 2024 05:11:50 GMT
I1028 05:11:50.435342    3731 request.go:1351] Response Body: {"paths":{".well-known/openid-configuration":{"serverRelativeURL":"/openapi/v3/.well-known/openid-configuration?hash=547B7A27CCECFC95452F868FE6858965A8F03AD45F43B1C514B03B11D530C4A561D7E6CCB1B7AFB2DACE6571855E103D65268EC477D8D1D910"},"api":{"serverRelativeURL":"/openapi/v3/api?hash=73FD8CD06D7A5465AB85682192C7B53A1025A5927B5CFAAFDB8F3F667D135C93380907165E622107BE3170D5772A16D32C2B92E1CE5C7A"},"api/v1":{"serverRelativeURL":"/openapi/v3/api/v1?hash=ACB12CB02CF9D1F67E460080FD0CB1F61851818983019A812C180EE0BEF50177B594CCC8A1D759E8E2CDE4B67CC39FBAEBC3178A77AC893E"},"apis":{"serverRelativeURL":"/openapi/v3/apis?hash=66EBAFABDD8195F2462F3BD1FA694DEAA5B3FD73279364E6C1C55D72A4A374199F539D2BA6C14D1954AC237D963A9E24ABF0E32A98B7F63"},"apis/admissionregistration.k8s.io":{"serverRelativeURL":"/openapi/v3/apis/admissionregistration.k8s.io?hash=74477EF088E408987C70CE253E25E180A5E62615780828AF690F8174F7B5558BDD5BD712D75C835C296530AD501AAF55E579 [truncated 10147 chars]
I1028 05:11:50.435820    3731 round_trippers.go:463] GET https://CAACC064F91730872A2378475808913C.gr7.us-east-1.eks.amazonaws.com/openapi/v3/api/v1?hash=ACB12CB095D6D59A3AF8E98D2CF9D1F67E460080FD0CB1F61851818983019A812C180EE0BEF50177B594CCC8A1D759E8E2CDE4B67CC39FBAEBC3178A77AC893E&timeout=32s
I1028 05:11:50.435838    3731 round_trippers.go:469] Request Headers:
I1028 05:11:50.435850    3731 round_trippers.go:473]     User-Agent: kubectl/v1.31.0 (linux/amd64) kubernetes/da2ed8d
I1028 05:11:50.435862    3731 round_trippers.go:473]     Accept: application/json
I1028 05:11:50.456738    3731 round_trippers.go:574] Response Status: 200 OK in 20 milliseconds
I1028 05:11:50.456980    3731 round_trippers.go:577] Response Headers:
I1028 05:11:50.457311    3731 round_trippers.go:580]     Content-Length: 1383492
I1028 05:11:50.457331    3731 round_trippers.go:580]     Etag: "ACB12CB095D6D59A3AF8E98D2CF9D1F67E460080FD0CB1F61851818983019A812C180EE0BEF50177B594CCC8A1D759E8E2CDE4B67CC39FBAEBC3178A77AC893E"
I1028 05:11:50.457342    3731 round_trippers.go:580]     X-Kubernetes-Pf-Prioritylevel-Uid: 34f9615d-c63b-4407-bf62-f8ebfbad8836
I1028 05:11:50.457350    3731 round_trippers.go:580]     Audit-Id: a994f4d9-ae15-4b76-a442-bc434f7bfdb8
I1028 05:11:50.457359    3731 round_trippers.go:580]     Vary: Accept
I1028 05:11:50.457367    3731 round_trippers.go:580]     Last-Modified: Mon, 28 Oct 2024 04:47:33 GMT
I1028 05:11:50.457376    3731 round_trippers.go:580]     X-Kubernetes-Pf-Flowschema-Uid: a100abb1-2a27-4c53-b81e-ff14fc8f6a35
I1028 05:11:50.457384    3731 round_trippers.go:580]     X-From-Cache: 1
I1028 05:11:50.457391    3731 round_trippers.go:580]     Accept-Ranges: bytes
I1028 05:11:50.457403    3731 round_trippers.go:580]     Cache-Control: public, immutable
I1028 05:11:50.457421    3731 round_trippers.go:580]     Date: Mon, 28 Oct 2024 04:59:25 GMT
I1028 05:11:50.457429    3731 round_trippers.go:580]     Expires: Tue, 28 Oct 2025 04:59:25 UTC
I1028 05:11:50.457439    3731 round_trippers.go:580]     Content-Type: application/json
I1028 05:11:50.457451    3731 round_trippers.go:580]     X-Varied-Accept: application/json
I1028 05:11:50.467654    3731 request.go:1351] Response Body: {"openapi":"3.0.0","info":{"title":"Kubernetes","version":"v1.30.5"},"paths":{"/api/v1/":{"get":{"tags":["core_v1"],"description":"get available resources","operationId":"getCoreV1APIResources","responses":{"200":{"description":"OK","content":{"application/json":{"schema":{"$ref":"#/components/schemas/io.k8s.apimachinery.pkg.apis.meta.v1.APIResourceList"}},"application/vnd.kubernetes.protobuf":{"schema":{"$ref":"#/components/schemas/io.k8s.apimachinery.pkg.apis.meta.v1.APIResourceList"}},"application/yaml":{"schema":{"$ref":"#/components/schemas/io.k8s.apimachinery.pkg.apis.meta.v1.APIResourceList"}}}},"401":{"description":"Unauthorized"}}}},"/api/v1/componentstatuses":{"get":{"tags":["core_v1"],"description":"list objects of kind ComponentStatus","operationId":"listCoreV1ComponentStatus","responses":{"200":{"description":"OK","content":{"application/json":{"schema":{"$ref":"#/components/schemas/io.k8s.api.core.v1.ComponentStatusList"}},"application/json;stream=watch":{"schema":{"$ref":"#/components/schemas/i [truncated 1382468 chars]


I1028 05:11:50.523900    3731 request.go:1351] Request Body: {"apiVersion":"v1","kind":"Namespace","metadata":{"labels":{"environment":"dev","project":"expense"},"name":"expense"}}
I1028 05:11:50.523974    3731 round_trippers.go:463] POST https://CAACC064F91730872A2378475808913C.gr7.us-east-1.eks.amazonaws.com/api/v1/namespaces?fieldManager=kubectl-create&fieldValidation=Strict
I1028 05:11:50.523989    3731 round_trippers.go:469] Request Headers:
I1028 05:11:50.524003    3731 round_trippers.go:473]     User-Agent: kubectl/v1.31.0 (linux/amd64) kubernetes/da2ed8d
I1028 05:11:50.524014    3731 round_trippers.go:473]     Accept: application/json
I1028 05:11:50.524034    3731 round_trippers.go:473]     Content-Type: application/json
I1028 05:11:50.535976    3731 round_trippers.go:574] Response Status: 409 Conflict in 11 milliseconds
I1028 05:11:50.536004    3731 round_trippers.go:577] Response Headers:
I1028 05:11:50.536045    3731 round_trippers.go:580]     Date: Mon, 28 Oct 2024 05:11:50 GMT
I1028 05:11:50.536054    3731 round_trippers.go:580]     Audit-Id: 5e3d153a-101c-4cc2-83d0-efc2c1ba24f2
I1028 05:11:50.536062    3731 round_trippers.go:580]     Cache-Control: no-cache, private
I1028 05:11:50.536072    3731 round_trippers.go:580]     Content-Type: application/json
I1028 05:11:50.536079    3731 round_trippers.go:580]     X-Kubernetes-Pf-Flowschema-Uid: a100abb1-2a27-4c53-b81e-ff14fc8f6a35
I1028 05:11:50.536087    3731 round_trippers.go:580]     X-Kubernetes-Pf-Prioritylevel-Uid: 34f9615d-c63b-4407-bf62-f8ebfbad8836
I1028 05:11:50.536095    3731 round_trippers.go:580]     Content-Length: 204
I1028 05:11:50.536131    3731 request.go:1351] Response Body: {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"namespaces \"expense\" already exists","reason":"AlreadyExists","details":{"name":"expense","kind":"namespaces"},"code":409}
I1028 05:11:50.536393    3731 helpers.go:246] server response object: [{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "error when creating \"01-namespace.yaml\": namespaces \"expense\" already exists",
  "reason": "AlreadyExists",
  "details": {
    "name": "expense",
    "kind": "namespaces"
  },
  "code": 409
}]
Error from server (AlreadyExists): error when creating "01-namespace.yaml": namespaces "expense" already exists
```