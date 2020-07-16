---
#Front matter (metadata).
abstract:               # REQUIRED

authors:
 - name: "Rahul Reddy Ravipally"
   email: "raravi86@in.ibm.com"
 - name: "Manoj Jahgirdar"
   email: "manoj.jahgirdar@in.ibm.com"
 - name: "Srikanth Manne"
   email: "srikanth.manne@in.ibm.com"
 - name: "Manjula G. Hosurmath"
   email: "mhosurma@in.ibm.com"

completed_date: 2020-07-03

components:
- slug: "Crunchy PostgreSQL for Kubernetes"
  name: "Crunchy PostgreSQL for Kubernetes"
  url: "https://marketplace.redhat.com/en-us/products/crunchy-postgresql-for-kubernetes"
  type: "component"
- slug: "redhat-marketplace"
  name: "Red Hat Marketplace"
  url: "https://marketplace.redhat.com/"
  type: "component"

draft: true|false       # REQUIRED

excerpt:                # REQUIRED

keywords:               # REQUIRED - comma separated list

last_updated:           # REQUIRED - Note: date format is YYYY-MM-DD

primary_tag:          # REQUIRED - Note: Choose only only one primary tag. Multiple primary tags will result in automation failure. Additional non-primary tags can be added below.

pta:                    # REQUIRED - Note: can be only one
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/primary-technology-area.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
 # - "cloud, container, and infrastructure"

pwg:                    # REQUIRED - Note: can be one or many
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/portfolio-working-group.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
# - "containers"

related_content:        # OPTIONAL - Note: zero or more related content
  - type: announcements|articles|blogs|patterns|series|tutorials|videos
    slug:

related_links:           # OPTIONAL - Note: zero or more related links
  - title:
    url:
    description:

runtimes:               # OPTIONAL - Note: Select runtimes from the complete set of runtimes below. Do not create new runtimes. Only use runtimes specifically in use by your content.
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/runtimes.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
 # - "asp.net 5"

series:                 # OPTIONAL
 - type:
   slug:

services:               # OPTIONAL - Note: please select services from the complete set of services below. Do not create new services. Only use services specifically in use by your content.
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/services.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
# - "blockchain"

subtitle:               # REQUIRED

tags:
# Please select tags from the complete set of tags below. Do not create new tags. Only use tags specifically targeted for your content. If your content could match all tags (for example cloud, hybrid, and on-prem) then do not tag it with those tags. Less is more.
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/tags.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
 # - "blockchain"

title:                  # REQUIRED

translators:             # OPTIONAL - Note: can be one or more
  - name:
    email:

type: tutorial

---

# Perform CRUD operations using Crunchy PostgreSQL for Kubernetes Operator on Red Hat Marketplace

In this tutorial, we will learn how to Perform CRUD operations using Crunchy PostgreSQL for Kubernetes Operator hosted on Red Hat marketplace using python runtime and Jupyter notebook.

# About Crunchy PostgreSQL for Kubernetes Operator

Deploy trusted open source PostgreSQL at scale. Crunchy PostgreSQL for OpenShift Container Platform (OCP) includes Crunchy PostgreSQL Operator and Crunchy PostgreSQL Container Suite supporting hybrid cloud, open source PostgreSQL-as-a-Service. [Learn more](https://marketplace.redhat.com/en-us/products/crunchy-postgresql-for-kubernetes).

# Learning objectives

When you have completed this tutorial, you will understand how to:

* Deploy CrunchyDB Operator on OpenShift Cluster
* Create a CrunchyDB cluster and database
* Access the cluster on your localhost
* Perform Create, Read, Update and Delete Operations on CrunchyDB

# Estimated time

Completing this tutorial should take about 30 minutes.

# Pre-requisites

1. [Red Hat Marketplace Account](https://marketplace.redhat.com/en-us/registration/om).
2. [Red Hat OpenShift Cluster](https://cloud.ibm.com/kubernetes/catalog/create?platformType=openshift). 
3. [OC & kubectl CLI](https://docs.openshift.com/container-platform/3.6/cli_reference/get_started_cli.html).

# Steps

### Step 1: Connect to the Openshift Cluster in CLI (command Line Interface)

- Login to the ROKS(IBM Managed) Openshift cluster through CLI(command line Iterface). 
To login you would require token which can be genrated after you login to Openshift Cluster web console. See below screenshot to `copy the path`.
![](doc/source/images/Login-CopyCommand.png)

- A new window will open with the login token details. See below screenshot for details. Copy the login token as per the below screenshot.
![](doc/source/images/Login-Token.png)

- Once you login you would see a similar screen as shown below.
![](doc/source/images/CLI-Login.png)

### Step 2: Deploy CrunchyDB Operator on OpenShift Cluster

- Use the new namespace where we have isntalled the Crunchy Postgres operator.
- Run the below command in CLI(command line Iterface)
``oc create -f postgres-operator.yml, wait for the pod state to change to complete state.
oc get po
NAME               READY   STATUS      RESTARTS   AGE
pgo-deploy-zl6sz   0/1     Completed   0          24h
``
3. check for the logs and be sure there are no errors in the ansible script.
4. switch to pgo namespace.
5. Edit pgo-config configmap and update DisableFSGroup to false .
6. Restart postgress operator pod. postgres-operator-f7d8c5667-4hhrk
Reason for above step (5,6):
Crunchy PostgreSQL for Kubernetes is set up to work with the "restricted" SCC by default, but we may need to make modifications. In this mode, we will want to ensure that "DisableFSGroup" is set to false mentioned "pgo-config" ConfigMap.
Making changes to the "pgo-config" ConfigMap, we will have to restart the "postgres-operator" Pod. 
7. Download the pgo binary mentioned in the below document
https://access.crunchydata.com/documentation/postgres-operator/latest/quickstart/
8. Make sure the pvc are in bound state.
oc get pvc
NAME                  STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
hippo                 Bound    pvc-1661d588-2065-43a3-a701-de3247bed97e   20Gi       RWO            ibmc-block-gold   23h
hippo-pgadmin         Bound    pvc-7c02f063-8eb6-43f2-9862-6d05c929ed2b   20Gi       RWO            ibmc-block-gold   23h
hippo-pgbr-repo       Bound    pvc-c32515c9-b088-4695-8ac3-245c12d182a6   20Gi       RWO            ibmc-block-gold   23h
hippotest             Bound    pvc-d8665d40-470e-4a92-aaa8-09000d0bab0c   20Gi       RWO            ibmc-block-gold   23h
hippotest-pgbr-repo   Bound    pvc-20754790-50f6-4759-9a7a-0463ce2d422f   20Gi       RWO            ibmc-block-gold   23h
9. Create database using below command.
pgo create cluster -n pgo hippo
This will create database (pods) in pgo namespace.
10. To validate run below commands.
    a . pgo show cluster -n pgo hippo.
    b.  pgo test -n pgo hippo
Attached is the postgres-operator.yml updated file. (edited) 
postgres-operator.yml 


### Step 2: Create a CrunchyDB cluster and database
### Step 3: Access the cluster on your localhost

- Let us view the results of the commands we ran in the earlier steps via the pgAdmin 4 console. The console can be accessed at localhost with port forwarding.

- Run the following command in Terminal:
```bash
$ pgo create pgadmin hippo
```
- This creates a pgAdmin 4 deployment unique to this PostgreSQL cluster and synchronizes the PostgreSQL user information into it.

- To access pgAdmin 4, you can set up a `port-forward` to the Service, which follows the pattern `<clusterName>-pgadmin`, to port `5050`:
```bash
$ kubectl port-forward -n pgo svc/hippo-pgadmin 5050:5050 
```

```
Forwarding from 127.0.0.1:5050 -> 5050
Forwarding from [::1]:5050 -> 5050
```

- Open <http://localhost:5050> on your browser and use your database username (e.g. `hippo`) and password (e.g. `datalake`) to log in.

![](doc/source/images/login-pgo.png) 

(Note: if your password does not appear to work, you can retry setting up the user with the [pgo update](https://access.crunchydata.com/documentation/postgres-operator/4.3.2/pgo-client/reference/pgo_update_user/) user command: `pgo update user -n pgo --username=hippo --password=datalake hippo`)

- Once logged in, you can see the pgAdmin 4 console as shown.

![](doc/source/images/pgadmin4.png)

### Step 4: Perform CRUD Operations on CrunchyDB using python

- Once we have the CrunchyDB UP and running, `user` and `database` created, we can now explore the CRUD operations on Crunchy PostgreSQL in a python runtime using Jupyter Notebook.

- In Terminal run the following command to port forward `5432` port from the OpenShift cluster which we will be using in our Jupyter Notebook to establish a connection with the Crunchy PostgreSQL instance.
```bash
$ kubectl port-forward -n pgo svc/hippo 5432:5432 
```

```
Forwarding from 127.0.0.1:5432 -> 5432
Forwarding from [::1]:5432 -> 5432
```

- We will be working with Jupyter Notebook, you can use tools like [Anaconda](https://www.anaconda.com/products/individual) to open the Jupyter Notebook or [install Jupyter Notebook from python-pip](https://jupyter.org/install).

- Download and open the notebook [CrunchyDB CRUD Operations.ipynb](CrunchyDB%20CRUD%20Operations.ipynb) in your local machine.

- Click on the **Cell** tab and click on **Run All** as shown.

![](doc/source/images/run.png)

- You can now follow the notebook instructions for more details on what is happening in each cell.

- After we have execuited the notebook, we can verify the table in the pgAdmin 4 console. 

- - To access pgAdmin 4, you can set up a `port-forward` to the Service, which follows the pattern `<clusterName>-pgadmin`, to port `5050`:
```bash
$ kubectl port-forward -n pgo svc/hippo-pgadmin 5050:5050 
```

```
Forwarding from 127.0.0.1:5050 -> 5050
Forwarding from [::1]:5050 -> 5050
```

- In the pgAdmin 4 console, expand your `cluster` (eg: in our case `cpdemo`), expand `Databases`, expand `database_name`, expand `Schemas`, expand `username` (eg: in our case `hippo`), expand `Tables`, you can now see the `accounts` table that we created from the notebook. Right click on the `accounts` table and select **View/Edit Data > All Rows** and the table with all rows will be displayed as shown.

![](doc/source/images/view.png)

>NOTE: You can view this table after each CRUD operation performed in notebook in order to visualize the changes.

![](doc/source/images/results.png) 

# Summary

In this tutorial you have learnt the basics of how to use `Crunchy postgreSQL for kubernetes Operator` deployed on OpenShift Cluster. You have learnt how to perform CRUD operations using the `Crunchy postgreSQL for kubernetes Operator` in python.

# Reference

The documentation for [Crunchy PostgreSQL Operator](https://access.crunchydata.com/) is available at,

  - https://access.crunchydata.com/documentation/postgres-operator/latest/

