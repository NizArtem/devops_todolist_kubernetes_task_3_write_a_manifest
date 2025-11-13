## To apply all Kubernetes manifests for the ToDo application, follow these steps:

1. Make sure you are in the root directory of the project where the `.infrastructure` folder is located.

2. Apply the namespace manifest first to create the `todoapp` namespace:

kubectl apply -f .infrastructure/namespace.yml

3. Apply the busybox pod manifest (test pod with curl):

kubectl apply -f .infrastructure/busybox.yml

4. Apply the ToDo app pod manifest:

kubectl apply -f .infrastructure/todoapp-pod.yml


5. Check that all pods are running and in the correct namespace:

kubectl get pods -n todoapp


You should see two pods: busybox-test and todoapp-pod.
todoapp-pod should be in Running state and readiness/liveness probes will automatically start.

## Testing ToDo application using port-forward

To test the ToDo application from your local machine, follow these steps:

1. Forward the pod port to your local machine:

kubectl port-forward pod/todoapp-pod 8000:8000 -n todoapp
The first 8000 is the port on your local machine.

The second 8000 is the container port exposed by the todoapp-pod.

2. Test the liveness and readiness endpoints using curl:

curl http://localhost:8000/health/
curl http://localhost:8000/readiness/

You can also open a browser and go to:

http://localhost:8000/

Stop port-forwarding when done by pressing Ctrl+C.

## Testing ToDo application using the busyboxplus:curl container

To test the ToDo application from inside the cluster using the busybox pod, follow these steps:

1. Connect to the busybox pod:

kubectl exec -it busybox-test -n todoapp -- sh

This opens a shell inside the busybox container.

Use curl to test the liveness and readiness endpoints of the ToDo app pod:

curl http://todoapp-pod:8000/health/
curl http://todoapp-pod:8000/readiness/

todoapp-pod is the name of your ToDo application pod.

You can verify that the pod is responding correctly inside the cluster.

Exit the busybox container shell when done:

exit