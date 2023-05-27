# successful-ic-repro


```
e.ausente@C02DR4L1MD6M ~ % kubectl get svc
NAME                                    TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    PORT(S)                      AGE
coffee-svc                              ClusterIP      10.100.148.94    <none>                                                                         80/TCP                       8d
ingress-nginx-controller                LoadBalancer   10.100.212.142   a1091f3647cd4481b8828061320c9bc0-2034089447.ap-southeast-1.elb.amazonaws.com   80:31497/TCP,443:30868/TCP   45d
ingress-plus-nginx-ingress-controller   LoadBalancer   10.100.52.175    a9fa871c21fb7424b9633c7c14f3d094-140401031.ap-southeast-1.elb.amazonaws.com    80:31306/TCP,443:32205/TCP   29m
kubernetes                              ClusterIP      10.100.0.1       <none>                                                                         443/TCP                      45d
tea-svc                                 ClusterIP      10.100.73.16     <none>                                                                         80/TCP                       8d
```


Go to GoDaddy and create a cname under the kushikimi.xyz zone: 
```
cname	nginxplusnic	a9fa871c21fb7424b9633c7c14f3d094-140401031.ap-southeast-1.elb.amazonaws.com.	1/2 Hour	
```

# Use Case # 1: 
Split traffic based on weights (100%) and with A/B testing
90% of requests will go to blue, 10% will go to green, if they have sent it with a request header: 
user: testgroup
Else, all passed to green.


```
kubectl apply -f traffic-splitting/
```

You can now access to see 
http://nginxplusnic.kushikimi.xyz/ 
if it works!
