# Tomcat example


httpd --- (AJP) ---> Tomcat

## Tomcat

Build via Dockerbuild / Containerfile.

Image available at: <quay.io/openshift-examples/tomcat-example:tomcat>

## httpd

Image available at: <quay.io/openshift-examples/tomcat-example:httod>





K8s Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
 ports:
  - name: 8009-tcp
    port: 8009
    protocol: TCP
    targetPort: 8009
  selector:
    app: tomcat
    deployment: tomcat
```

### Build

On OpenShift

```
oc new-build registry.access.redhat.com/ubi9/httpd-24:1-311~https://github.com/openshift-examples/tomcat-example --context-dir=httpd
```

Local
```
s2i build ./httpd/ registry.access.redhat.com/ubi9/httpd-24:1-311 httpd --as-dockerfile httpd.Containerfile
podman build -t quay.io/openshift-examples/tomcat-example:httpd . -f httpd.Containerfile
```

# Resources

- ajping <https://www.joedog.org/2012/06/06/ajp-functional-test/>
