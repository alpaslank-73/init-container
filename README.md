Wait for a DB demo (using "job" and "initrContainers")

# Bu demoda s2i yok; hazir db ve todo imaji kullaniyorsun! (hizli olmasi icin)

$ git clone https://github.com/alpaslank-73/init-container.git

$ cd init-container

$ oc new-project init-container

# Create Service Account (hem "job" hem de "initContainer" icin gerekli yoksa "oc ..." yapamiyor container)

$ oc create sa checkdb 

$ oc policy add-role-to-user edit -z checkdb  

# Once Job'i goster:

$ oc create -f wait-for-db.yaml
$ watch oc get po      ==> db deploy olana kadar "Running" oluyor; alp-mysql deployment'i "Available" oldugu ancak "Completed" oluyor

# DB'yi deploy et: 

$ oc create -f alp-mysql.yaml

=================
# initContainer demo'su #
=================

$ oc delete all --all

# Once todo'yu deploy et ve 
$ oc create -f todo.yaml

$ watch oc get po    ==> STATUS ==> "Init:0/1"

# Sonra DB'yi deploy et:

$ oc create -f alp-mysql.yaml

DB'yi deploy ettiginde "Init:0/1" status'una sahip olan pod "Running" oluyor.

===============================================
Opsiyonel: Istersen once deploy'u sonra da service'i expose edebilirsin:
===============================================
$ oc expose deploy todo --port 8080

$ oc expose svc todo

$ oc get route
