

OPENSHIFT_USER ?= kubeadmin
OPENSHIFT_PASSWORD ?= dRSgC-cSnch-ekyyD-ayoqc
OPENSHIFT_HOST ?= https://api.crc.testing:6443

MANIFEST_FILENAME ?= atmoz-sftp.yaml
PROJECT ?= sftp
SCC ?= anyuid
SVC_ACC_NAME ?= $(PROJECT)-$(SCC)-service-account
SERVICE_NAME ?= service/sftp-service



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



.PHONY: secrets
secrets:
	@echo "======================================================================"
	@echo "                      Creating Secrets                                "
	@echo "======================================================================"
	oc create secret generic sftp-client-public-keys --from-file=client_keys
	oc create secret generic sftp-host-keys --from-file=host_keys


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
sftp-server: project service-account secrets deploy