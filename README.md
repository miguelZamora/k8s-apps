# Pasos para Desplegar en minikube los Kubernetes **

Se puede crea los recursos como una shell :



(Tips en wilson pueden usar la shell de git es guena) (windows o linux) 
en ambos debe funcionar claro esta todo esta hecho desde linux 
pero con paciencia se puede todo

bash 
```
apps_run_k8s.sh 

kubectl apply -f mysql-secret.yaml
kubectl apply -f mysql-pvc.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f php-nginx-configmap.yaml
kubectl apply -f php-nginx-deployment.yaml
kubectl apply -f php-nginx-service.yaml

```

Verifica los Pods y Services:

bash

```
kubectl get pods,svc

```
Accede a la aplicación:

Si estás en Minikube:

bash

```
minikube service php-nginx-service --url

```
Si estás en la nube, usa la IP externa del NodePort (puerto 30080).

¿Cómo reiniciar Minikube desde cero?

Si quieres limpiar todo y comenzar de nuevo (eliminar deployments, pods, configuraciones previas):

a) Eliminar todo en Minikube
bash
# 1. Eliminar todos los recursos de Kubernetes en el clúster actual

```
kubectl delete all --all  # Borra pods, services, deployments, etc.
kubectl delete pvc --all  # Borra PersistentVolumeClaims (si los hay)
kubectl delete secrets --all  # Borra Secrets
kubectl delete configmaps --all  # Borra ConfigMaps
```

# 2. Reiniciar Minikube (opcional, si quieres un clúster totalmente nuevo)

```
minikube stop  # Detiene Minikube
minikube delete  # Elimina el clúster actual
minikube start  # Crea un nuevo clúster
```

b) Verificar que todo esté limpio

bash
```
kubectl get all  # No debería mostrar recursos
kubectl get ns   # Solo debería aparecer "default", "kube-system", etc.
```
¿Qué son los "Namespaces" (Espacios de Nombres) en Kubernetes?

Los namespaces son una forma de dividir un clúster de Kubernetes en entornos virtuales aislados. Son útiles para:
Separar aplicaciones (ej: producción, desarrollo, testing).

Evitar conflictos de nombres (ej: dos equipos usando el mismo nombre de Deployment).
Controlar acceso con RBAC (permisos por namespace).
Namespaces por defecto en Minikube
Al iniciar Minikube, estos namespaces existen:
default: Donde se crean los recursos si no especificas otro.
kube-system: Contiene los componentes del sistema (CoreDNS, métricas, etc.). No lo modifiques.
kube-public: Para recursos accesibles globalmente (rara vez usado).
kube-node-lease: Para controlar nodos (avanzado).

¿Cómo usar namespaces?

a) Crear un nuevo namespace

bash
```
 	kubectl create namespace desarrollo
```
b) Trabajar en un namespace específico

bash

# 1. Crear un Deployment en el namespace "desarrollo"
```
	kubectl create deployment mi-app --image=nginx -n desarrollo
```
# 2. Ver recursos solo en "desarrollo"
```
	kubectl get pods -n desarrollo
```
# 3. Cambiar el namespace por defecto (evita usar -n siempre)
```	
	kubectl config set-context --current --namespace=desarrollo
```
