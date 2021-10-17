

OPENSHIFT_USER ?= 
OPENSHIFT_PASSWORD ?= 
OPENSHIFT_HOST ?= 

MANIFEST_FILENAME ?= atmoz-sftp.yaml
PROJECT ?= atmoz
SCC ?= anyuid
SVC_ACC_NAME ?= $(PROJECT)-$(SCC)-service-account
SERVICE_NAME ?= service/atmoz-sftp-service



.PHONY: login
login:
	@echo "======================================================================"
	@echo "           Logging Into '$(OPENSHIFT_HOST)' With User '$(OPENSHIFT_USER)'                 "
	@echo "======================================================================"
	oc login -u $(OPENSHIFT_USER) $(OPENSHIFT_HOST) -p $(OPENSHIFT_PASSWORD)



.PHONY: project
project:
	@echo "======================================================================"
	@echo "                   Creating Project '$(PROJECT)'                      "
	@echo "======================================================================"
	oc new-project $(PROJECT) 



.PHONY: service-account
service-account:
	@echo "======================================================================"
	@echo "                   Creating Service Account                           "
	@echo "======================================================================"
	oc create serviceaccount $(SVC_ACC_NAME)
	@echo "======================================================================"
	@echo "  Associating '$(SVC_ACC_NAME)' with SCC '$(SCC)'                     "
	@echo "======================================================================"
	oc adm policy add-scc-to-user $(SCC) -z $(SVC_ACC_NAME)



.PHONY: deploy
deploy:
	@echo "======================================================================"
	@echo "                      Deploying SFTP Server                           "
	@echo "======================================================================"
	oc apply -f $(MANIFEST_FILENAME)



.PHONY: delete-project
delete-project:
	@echo "======================================================================"
	@echo "                Deleting Project '$(PROJECT)'                        "
	@echo "======================================================================"
	oc delete project $(PROJECT)



.PHONY: sftp-server
sftp-server: project service-account deploy