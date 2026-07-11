# Openshift

## if image cant able to pull registry server then i will verify the token and public key is valid or not  

Openshift insllation using two method
1> IPI method which is automated
2> upi method manual way we have configure

## OCP inslation on bare metal using UPI method.

Add-on services:

1. DHCP : assign ip to nodes (Master & worker & bootstrap ) where we bind mac static ip address
2. HAProxy: s/w define Loadbalncer req when we have 3 master node.  its set high availablity across control plane.
3. DNS Server: Using for API server resolve, ingress
   all master node and Bootstrap server should resolve properly whic is mentioned on forward zone , ha proxy & ingress controler domain , Oauth record should
   resolve & console service.

4. HTTPD: download ignition file to each nodes which defines routes.

5. Quay.io registry image access.

6. RHEL-9 : Machine : helper Node (service):

   NIC1: Internet
   NIC2: Private Network DHCP ----Master Node /workers/

   Master1: Nic1-Private Network
            Nic2- Internet

   Step1: Service Nodes. 
